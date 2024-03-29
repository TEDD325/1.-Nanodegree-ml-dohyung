# Data Preprocessing

데이터 전처리는 모델 학습 전에 수행된다.

# EDA

```python
import os
from os.path import join
from pathlib import Path
import copy
import warnings
warnings.filterwarnings('ignore')

import numpy as np
import pandas as pd

# from google.colab import drive
# drive.mount('/content/drive')

ROOT_PATH = Path('/content/drive/MyDrive/data')
example_file = join(ROOT_PATH, 'Hospital', 'train.csv')

data = pd.read_csv(example_file)
data.head()
```

사용하고자 하는 데이터는 Hospital 데이터로서, imbalanced data다. 

```
train.csv - 의료기관이 폐업했는지 여부를 포함하여 최근 2개년의 재무정보와 병원 기본정보
test.csv - 폐업 여부를 제외하고 train.csv와 동일
sample_submission.csv - inst_id와 open과 close를 예측하는 OC 두개의 열로 구성. OC의 값은 open 예측일 경우 1, close 예측일 경우 0.

inst_id - 각 파일에서의 병원 고유 번호
OC – 영업/폐업 분류, 2018년 폐업은 2017년 폐업으로 간주함
sido – 병원의 광역 지역 정보
sgg – 병원의 시군구 자료
openDate – 병원 설립일
bedCount - 병원이 갖추고 있는 병상의 수
instkind – 병원, 의원, 요양병원, 한의원, 종합병원 등 병원의 종류
·        종합병원 : 입원환자 100명 이상 수용 가능
·        병원 : 입원 환자 30명 이상 100명 미만 수용 가능
·        의원 : 입원 환자 30명 이하 수용 가능
·        한방 병원(한의원) : 침술과 한약으로 치료하는 의료 기관.
revenue1 – 매출액, 2017(회계년도)년 데이터를 의미함
salescost1 – 매출원가, 2017(회계년도)년 데이터를 의미함
sga1 - 판매비와 관리비, 2017(회계년도)년 데이터를 의미함
salary1 – 급여, 2017(회계년도)년 데이터를 의미함
noi1 – 영업외수익, 2017(회계년도)년 데이터를 의미함
noe1 – 영업외비용, 2017(회계년도)년 데이터를 의미함
Interest1 – 이자비용, 2017(회계년도)년 데이터를 의미함
ctax1 – 법인세비용, 2017(회계년도)년 데이터를 의미함
Profit1 – 당기순이익, 2017(회계년도)년 데이터를 의미함
liquidAsset1 – 유동자산, 2017(회계년도)년 데이터를 의미함
quickAsset1 – 당좌자산, 2017(회계년도)년 데이터를 의미함
receivableS1 - 미수금(단기), 2017(회계년도)년 데이터를 의미함
inventoryAsset1 – 재고자산, 2017(회계년도)년 데이터를 의미함
nonCAsset1 – 비유동자산, 2017(회계년도)년 데이터를 의미함
tanAsset1 – 유형자산, 2017(회계년도)년 데이터를 의미함
OnonCAsset1 - 기타 비유동자산, 2017(회계년도)년 데이터를 의미함
receivableL1 – 장기미수금, 2017(회계년도)년 데이터를 의미함
debt1 – 부채총계, 2017(회계년도)년 데이터를 의미함
liquidLiabilities1 – 유동부채, 2017(회계년도)년 데이터를 의미함
shortLoan1 – 단기차입금, 2017(회계년도)년 데이터를 의미함
NCLiabilities1 – 비유동부채, 2017(회계년도)년 데이터를 의미함
longLoan1 – 장기차입금, 2017(회계년도)년 데이터를 의미함
netAsset1 – 순자산총계, 2017(회계년도)년 데이터를 의미함
surplus1 – 이익잉여금, 2017(회계년도)년 데이터를 의미함
revenue2 – 매출액, 2016(회계년도)년 데이터를 의미함
salescost2 – 매출원가, 2016(회계년도)년 데이터를 의미함
sga2 - 판매비와 관리비, 2016(회계년도)년 데이터를 의미함
salary2 – 급여, 2016(회계년도)년 데이터를 의미함
noi2 – 영업외수익, 2016(회계년도)년 데이터를 의미함
noe2 – 영업외비용, 2016(회계년도)년 데이터를 의미함
interest2 – 이자비용, 2016(회계년도)년 데이터를 의미함
ctax2 – 법인세비용, 2016(회계년도)년 데이터를 의미함
profit2 – 당기순이익, 2016(회계년도)년 데이터를 의미함
liquidAsset2 – 유동자산, 2016(회계년도)년 데이터를 의미함
quickAsset2 – 당좌자산, 2016(회계년도)년 데이터를 의미함
receivableS2 - 미수금(단기), 2016(회계년도)년 데이터를 의미함
inventoryAsset2 – 재고자산, 2016(회계년도)년 데이터를 의미함
nonCAsset2 – 비유동자산, 2016(회계년도)년 데이터를 의미함
tanAsset2 – 유형자산, 2016(회계년도)년 데이터를 의미함
OnonCAsset2 - 기타 비유동자산, 2016(회계년도)년 데이터를 의미함
receivableL2 – 장기미수금, 2016(회계년도)년 데이터를 의미함
Debt2 – 부채총계, 2016(회계년도)년 데이터를 의미함
liquidLiabilities2 – 유동부채, 2016(회계년도)년 데이터를 의미함
shortLoan2 – 단기차입금, 2016(회계년도)년 데이터를 의미함
NCLiabilities2 – 비유동부채, 2016(회계년도)년 데이터를 의미함
longLoan2 – 장기차입금, 2016(회계년도)년 데이터를 의미함
netAsset2 – 순자산총계, 2016(회계년도)년 데이터를 의미함
surplus2 – 이익잉여금, 2016(회계년도)년 데이터를 의미함
employee1 – 고용한 총 직원의 수, 2017(회계년도)년 데이터를 의미함
employee2 – 고용한 총 직원의 수, 2016(회계년도)년 데이터를 의미함
ownerChange – 대표자의 변동
```

## 모든 column 보기

`head()` 메서드만으로는 jupyter notebook 등의 IDE에서 모든 컬럼을 확인할 수 없다. `set_option()`을 사용하면 모든 컬럼을 볼 수 있다.

```python
pd.set_option('display.max_rows',500)
pd.set_option('display.max_columns',500)
data.head()
```

---

```python
data.shape # (301, 58)
```

## 데이터 전처리

데이터를 전처리하기 위해서는 각 feature가 의미하는 바를 일일이 파악해야 한다. 이 과정에는 도메인 지식이 필요하다.

또한 어느 한 feature가 다른 feature와 연관을 가질 수도 있기 때문에 feature engineering 등을 수행해야 한다.

## NaN값 여부 확인

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled.png)

이 데이터는 SQL을 이용하여 outer join이 일어난 결과물로 추정된다. 이 NaN 값들을 어떻게 채워야 할까?

## 라벨의 구분

현재 이 데이터에서 라벨은 `OC` 컬럼으로서, 병원의 개/폐업 유무다.

라벨은 아래와 같이 별도로 구분하는 게 좋다.

```python
label = data['OC']
```

그리고 데이터에 있는 라벨 컬럼을 제거해야 한다. `del` 또는 `drop()`을 쓴다.

```python
# del data['OC']
data.drop(columns=['OC'], inplace=True)
```

## describe()

각 변수별 평균, 표준편차, 최대, 최소, 사분위수 등의 기초 통계량을 확인한다. 이를 통해 어떤 데이터가 이상한지 판단할 수 있다. 

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%201.png)

## info()

info() 메서드로 각 변수들의 자료형을 확인할 수 있다.

```python
data.info()
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%202.png)

변수에는 수치형 변수와 범주형 변수가 있다. object 타입을 가진 컬럼들은 모두 범주형 변수다. 이들은 구분되어 전처리가 진행된다.

또한, 라벨에 대한 자료형 확인도 필요하다.

```python
label.info()
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%203.png)

만약 trainset과 testset으로 이미 나눈 상태라면, 이들에 대해 info()를 모두 적용해야 한다.

## 범주형 변수와 수치형 변수 구분하기

```python
cat_columns = data.select_dtypes(include='object').columns # 범주형 변수
num_columns = data.select_dtypes(exclude='object').columns # 수치형 변수
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%204.png)

위 코드는 범주형 변수와 수치형 변수를 구분하여 컬럼 이름만을 뽑아내는 코드다.

scikit-learn을 쓰면 한 줄에 처리가 가능하며 현업에서 주로 쓴다.

# Scaling

데이터 스케일링은 데이터의 값이나 범위를 변경하여 해당 값들의 범위를 조정하는 작업이다. 이를 통해 모델 학습 과정에서 발생할 수 있는 수치적 불안정성을 완화하거나 최적화 알고리즘의 성능을 향상시킬 수 있다.

딥러닝의 normalization과 문맥상 비슷하다. 예를 들어, 딥러닝으로 CV 작업을 할 때, RGB 이미지에 대해서 255의 값으로 나누어주는 것과 같다. 이러한 작업은 텐서에서 알아서 이루어진다.

## 스케일링을 하는 이유

오버피팅을 방지하기 위해서다.

예를 들어 수치형 변수의 절대값의 크기가 너무 작거나, 혹은 절대값이 너무 큰 경우(즉 극단적인 분포를 보이는 경우), 해당 변수가 Target 에 미치는 영향력이 제대로 **표현**되지 않을 수 있다.

<aside>
💡 **feature representation**

모델은 feature를 통해 패턴을 파악하여 target을 맞추기 위한 학습을 진행한다. 이 때, 각 feature를 학습하기 좋게 표현하는 것이다. 

</aside>

값의 범위가 너무 크면 모델이 모든 값의 범위를 학습하므로 오버피팅된다. 즉 모델이 가진 가중치의 값이 극단적이게 된다. 예를 들어, 큰 값의 범위를 학습한 모델을 그래프로 표현하면 지나치게 굴곡진 그래프가 그려지게 된다.

따라서, 

1. feature가 모델에 학습하기 적합하도록 representation된 데이터(모델이 학습하기 좋은 스케일링 된 데이터)를 사용하거나, 
2. 규제를 적용하면 

가중치들이 지나치게 커지는 등의 오버피팅을 방지한다. 

스케일링은 trainset과 tsetset 모두에 적용한다.

## 스케일링의 적용: Pandas DataFrame → ndarray

스케일링을 적용하기 위해서는 Pandas의 DataFrame으로 된 데이터로부터 ndarray를 추출해야 한다. 아래와 같이 추출한다.

```python
numeric_data = data[num_columns].values # 이렇게 항상 numpy array로 하지는 않아도 된다.
numeric_data
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%205.png)

## min-max scaling

다음의 수식을 기반으로 값의 범위를 0~1 사이로 변경한다.

$x - Min(X) \over Max(X) - Min(X)$

- $X$: 데이터 셋
- $x$: 데이터 샘플

[sklearn.preprocessing.MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html?highlight=minmax#sklearn.preprocessing.MinMaxScaler)

---

이 데이터에 min-max scaling을 적용하기 위한 방법은 아래와 같다.

```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()

scaler.fit(numeric_data)

scaled_data = scaler.transform(numeric_data) #
scaled_data = pd.DataFrame(scaled_data, columns=num_columns)
```

스케일링 된 결과는 ndarray이기 때문에 이를 다시 Pandas DataFrame으로 만들어줘야 한다.

---

스케일링이 적용되기 전 데이터와 스케일링된 데이터를 비교해보자.

```python
data[num_columns].head()
```

```python
scaled_data.head()
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%206.png)

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%207.png)

---

```python
scaled_data.describe()
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%208.png)

스케일링 된 데이터는 위와 같이 min값은 0, max값은 1이 된다.

## standard scaling

Z-score 정규화

데이터 값을 표준정규분포 형태로 스케일링하는 방법이다. 그렇다고 해서 데이터의 분포를 바꾸지는 않는다. 단지 데이터의값 평균이 0, 표준 편차가 1이 되도록 다음의 수식을 기반으로 스케일링한다.

$z = {{x - \mu} \over {\sigma}}$

- $\mu$: 데이터의 평균
- $\sigma$: 데이터의 표준 편차
- $X$: 데이터 셋
- $x$: 데이터 샘플

[sklearn.preprocessing.StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler)

hospital 데이터에 standard scaling을 적용하기 위한 방법은 아래와 같다.

```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()

scaler.fit(numeric_data)

scaled_data = scaler.transform(numeric_data)
scaled_data = pd.DataFrame(scaled_data, columns=num_columns)
```

```python
data[num_columns].head()
```

![스탠다드 스케일링 적용 전 데이터](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%209.png)

스탠다드 스케일링 적용 전 데이터

```python
scaled_data.head()
```

![스탠다드 스케일링 적용 후 데이터](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2010.png)

스탠다드 스케일링 적용 후 데이터

```python
data[num_columns].describe()
```

![스탠다드 스케일링 적용 전 기초통계량](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2011.png)

스탠다드 스케일링 적용 전 기초통계량

```python
scaled_data.describe()
```

![스탠다드 스케일링 적용 후 기초통계량](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2012.png)

스탠다드 스케일링 적용 후 기초통계량

평균과 표준편차가 각각 0과 1로 바뀐 것을 볼 수 있다.

- 1e+2라는 표현은 1*10^(+2)의 의미다.
- 컴퓨터는 정확한 실수를 표현하지 못하는데 이를 부동소수점 문제라고 한다. 이 부동소수점 문제 때문에 수치 연산에 오차가 발생하기도 한다. 이러한 이유로, 블록체인에서는 0.001개 코인을 엄청나게 큰 정수로 표현한다. 실제 유저에게는 해당 decimal값으로 나누어주어 원래의 소수점 표현으로 보여준다.
    - 시스템에서 실수형을 민감하게 쓸 수밖에 없는 케이스에서는 부동소수점인 float 타입을 안쓰고 고정소수점인 decimal 타입을 쓴다. 이 둘은 실수를 표현하는 방식이 다르다. 부동소수점은 소수점의 위치가 고정되지 않는 반면, 고정소수점은 소수점과 자릿수를 강제한다. 때문에 decimal 타입에 대해서는 복잡한 연산이 이루어진다.
    - Tensor는 float에 가깝다. 보통은 소수점에 예민할 때에 decimal을 쓰는데, 소수점이 모델의 학습에 끼치는 영향이 그리 크지 않기 때문에 float 타입을 써도 무방하다. 오히려 decimal로 만들어주는 연산이 더 클 것이다.

## 어떤 방법을 써야 하나?

어느 상황에서나 가장 좋은 방법은 없다. 데이터에 따라서 다를 수 있고, 도메인에 따라 다를 수도 있다. 

# Transformation

데이터 스케일링은 데이터의 값이나 범위를 변경하여 해당 값들의 범위를 조정해주지만,  데이터 분포의 모양을 변경하지는 않는다. 데이터 치우친 분포(skew)의 형태를 보정해주려면 데이터 변환(transformation)을 사용해야 한다. 이러한 변환은 데이터가 정규 분포를 따르도록 만들거나 다른 형태의 분포를 가지도록 조정하는 것이다.

즉, 데이터 스케일링과 데이터 변환은 서로 다른 목적으로 사용된다.

```python
scaled_data['revenue2'].hist(bins=20) 
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2013.png)

현재 데이터 분포는 롱테일을 가진다. 

## Log transformation

로그를 취했을 때 노말분포가 되게 한다. 변수의 범위가 **양수인 경우에만** 사용할 수 있으며, 각 변수에 대해 자연 로그를 취한다.

```
scaled_data['log_revenue2'] = np.log1p(data['revenue2'])
scaled_data['log_revenue2'].hist(bins=50)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2014.png)

앞서 본 데이터 분포에 Log transformation를 적용하면 위와 같이 나온다. 무엇이 잘못된걸까? 0값이 지나치게 많은 상황이라, 0을 제외하고 보아야 한다.

```python
scaled_data['log_revenue2'].loc[scaled_data['log_revenue2'] > 0].hist(bins=50) # 그나마 모여있는게 좋다.
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2015.png)

위와 같이 정규분포의 모양에 가까워졌다는 것을 확인할 수 있다.

하지만 우리는 0을 제외했는데, 0 또는 음수가 있는 경우엔 로그를 취할 수 없기 때문에 실은 다른 방법을 써야 한다.

## Box-cox transformation

Log 변환과 동일하게, 양수값에만 적용할 수 있다.

Box-Cox 수식에서, Lambda 값이 0일 땐 Log 변환과 동일하다.

```python
from sklearn.preprocessing import PowerTransformer 

trans = PowerTransformer(method='box-cox')

scaled_data['box_cox_revenue2'] = trans.fit_transform(scaled_data['revenue2'].values.reshape(-1, 1) + 1)
scaled_data['box_cox_revenue2'].hist(bins=40)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2016.png)

## Yeo-Johnson transformation

Log 변환과 Box-Cox 변환의 한계를 개선한 방법으로서, 음수를 가진 변수값도 가능하다. 

```python
from sklearn.preprocessing import PowerTransformer

trans = PowerTransformer(method='yeo-johnson')

scaled_data['yeo_johnson_revenue2'] = trans.fit_transform(scaled_data['revenue2'].values.reshape(-1, 1))
scaled_data['yeo_johnson_revenue2'].hist(bins=40)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2017.png)

## Quantile transformation

가장 자주 발생하는 값(the most frequent values) 주위로 분포를 조정한다. 이상치의 영향을 감소시켜 준다.

[sklearn.preprocessing.QuantileTransformer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.QuantileTransformer.html#sklearn.preprocessing.QuantileTransformer)

```python
from sklearn.preprocessing import QuantileTransformer

trans = QuantileTransformer(output_distribution='normal')

scaled_data['quantile_revenue2'] = trans.fit_transform(scaled_data['revenue2'].values.reshape(-1, 1))
scaled_data['quantile_revenue2'].hist(bins=40)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2018.png)

## trainset, testset 모두에 적용하는가? YES

trainset 뿐만 아니라, testset에도 마찬가지로 각 feature에 대해 동일하게 scaling→transformation 과정을 완벽히 똑같이 적용해야 한다. 이를 위해 따로 함수로 trainset과 testset이 처리되도록 만들기도 한다.

## 어떤 방법을 써야 하나?

어떤 방법이 제일 좋다는 건 없다. 모양이 이쁘게 잘 나오게 만드는 방법을 선택해야 하며, 이는 feature마다 다른 방법을 적용해야함을 의미한다. 

‘모양이 이쁘게 잘 나온다는 것’은 skewness(`pandas.DataFrame.skew`) 값으로 정량적으로 판단 가능하며, 일반적으로는 이 값이 작은 것이 좋다. 하지만 skewness 값을 참고해서 전처리를 했음에도 모델링 결과가 좋지 않을 수도 있다는 점은 참고해야 한다.

![[https://scikit-learn.org/stable/auto_examples/preprocessing/plot_map_data_to_normal.html#sphx-glr-auto-examples-preprocessing-plot-map-data-to-normal-py](https://scikit-learn.org/stable/auto_examples/preprocessing/plot_map_data_to_normal.html#sphx-glr-auto-examples-preprocessing-plot-map-data-to-normal-py)

각 변환마다 결과가 조금씩 다른 것을 볼 수 있다.](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2019.png)

[https://scikit-learn.org/stable/auto_examples/preprocessing/plot_map_data_to_normal.html#sphx-glr-auto-examples-preprocessing-plot-map-data-to-normal-py](https://scikit-learn.org/stable/auto_examples/preprocessing/plot_map_data_to_normal.html#sphx-glr-auto-examples-preprocessing-plot-map-data-to-normal-py)

각 변환마다 결과가 조금씩 다른 것을 볼 수 있다.

[sklearn.preprocessing.PowerTransformer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PowerTransformer.html#sklearn.preprocessing.PowerTransformer)

## 스케일링 후 변환인가, 변환 후 스케일링인가?

두 가지 방법 모두 사용 가능하지만, 일반적으로는 스케일링 후에 변환하는 방법이 더 흔하게 사용된다. 스케일링이 데이터의 단위나 크기를 조정하는데 도움을 주고, 그 후에 분포 변환을 통해 데이터를 모델링하기 좋은 형태로 만들기 때문이다. 

# Imputation

결측치 처리 방법을 imputation이라 한다.

정형 데이터를 다루다보면, 현재 이 노트에서 언급한 데이터처럼, 값이 NaN(Not a Number or Null)으로 되어있는 경우가 있다([NaN값 여부 확인](https://www.notion.so/NaN-0a6e66fe36a34ec3ac2dc8c3e6c8533f?pvs=21)). 이러한 값을 결측치라 한다.

주의할 점은, 결측치 자체에 의미가 있을 수도 있다는 점이다. 즉, 설문조사 등에서 필수값이 아닌 값들은 심리적인 이유로 공백으로 남긴 경우 등이 그러한 경우다. 이럴 경우, 해당 결측치들을 별도의 새로운 feature로 만들어서 이러한 NaN값을 가진 샘플(row)들은 별도의 의미를 갖는 사람들이라는 식으로도 feature engineering을 진행할 수 있다. 따라서 도메인에 대한 이유가 필수적이다. 따라서 해당 결측치들을 채워서 써도 될지, 그대로 사용할지 등은 도메인 전문가들과 협의가 필수적이라고 할 수 있다.

하지만 왠만한 케이스에서는 결측치는 의미가 없다. 

## 결측치 확인: `isna()` & `sum()`

결측치를 확인하는 방법으로 Pandas의 `isna()` 와 `sum()` 메소드의 조합으로 확인할 수 있다.

`isna()`는 값이 null인지 체크한 후, T/F를 반화한다.

```python
pd.isna(data)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2020.png)

이에 대해 `sum()` 메서드를 적용해나가면 null값 체크가 쉬워진다. Pandas의`sum()`메서드는 자체적으로 값을 숫자로 바꿀 수 있는지 판단한 후, 값을 바꾸어 연산을 진행하기 때문이다.

```python
pd.isna(data).sum()
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2021.png)

```python
pd.isna(data).sum().sum() # 425
```

missingno라는 라이브러리 등을 쓰면 시각화도 가능하다.

```python
from missingno import matrix
matrix(data)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2022.png)

## 수치형 변수의 결측치에 대한 처리

### Mean

수치형 변수의 결측치에 적용하는 방법으로서, 평균 값으로 결측치를 채우는 것을 의미한다.

평균은 모든 값을 고려하기 때문에, 지나치게 작거나 큰 값(즉, 이상치값)들의 영향을 많이 받는다. 즉, 평균의 함정에 빠질 수 있다. 따라서, 이상치가 없다는 전제하에 써야 하며, 이상치가 있다면 이상치를 제거하여 적용해야 한다. 

평균은 아래 수식과 같이 모든 샘플의 값을 더하고, 샘플의 개수로 나누어 계산한다.

$E(x) = {\sum x \over n}$

작업을 위해 데이터를 복사해둔다.

```python
mean_df = data.copy()
```

---

다음과 같이 적용한다.

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy='mean') 
imputer.fit(mean_df[num_columns])
mean_df[num_columns] = imputer.transform(mean_df[num_columns])

pd.isna(mean_df[num_columns]).sum().sum() # 0
```

결측치가 모두 채워졌음을 확인할 수 있다.

### Median

수치형 변수의 결측치에 적용하는 방법으로서, 데이터들을 순서대로 줄 세웠을 때 딱 중간에 있는 값으로 채우는 것을 의미한다. 만약 중간값이 하나가 아니라면 전후의 두 값의 평균을 중간값으로 정한다. 

Median은 이상치에 영향을 덜 받는 방법이다. 따라서 평균의 함정에 빠지지 않는다.

Median은 값들을 정렬한 후, 중앙에 위치한 값으로 구한다.

```python
median_df = data.copy()

imputer = SimpleImputer(strategy='median')
imputer.fit(median_df[num_columns])
median_df[num_columns] = imputer.transform(median_df[num_columns])

pd.isna(median_df[num_columns]).sum().sum() # 0
```

### Iterative Impute

MICE(Multiple Imputation by Chained Equations)

수치형 변수의 결측치에 적용하는 방법으로서, 다른 row값들을 기반으로 결측치의 값을 iterative하게 개선시켜 나가는 방식(Round robin 방식)이다. 이를 위해 각 변수에 회귀 모델을 적용하여 결측치를 예측하고, 이러한 반복을 여러 번 수행하여 결측치를 추산한다. 

MICE는 내부적으로 회귀식이 적용된다(다른 변수들을 독립 변수로 사용하여 회귀식을 적합하고, 결측치 값을 채워야 하는 컬럼을 종속 변수로 한다).

1. 각 결측치를 일단은 해당 변수의 평균으로 초기화한다.
2. 대체할 변수의 결측치를 제외한 상태로, 다른 변수들을 모두 포함한 회귀모델을 만들고, 결측치값을 추산한다.
3. 다른 결측치 변수에 대해서 동일하게 반복한다.
4. 해당 이터레이션에서 맨 처음에 할당했던 값과의 차이를 계산한다.
5. 해당 값의 차이가 0이 될 때(수렴)까지 반복한다.

MICE 방법은 일반적으로 수치형 변수에 적용된다. 

MICE의 적용은 다음과 같다.

[sklearn.impute.IterativeImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.IterativeImputer.html?highlight=mice)

```python
impute_df = data.copy()

from sklearn.experimental import enable_iterative_imputer # 아직 공식 기능이 아니라서 이 라인을 넣어야 한다.
from sklearn.impute import IterativeImputer

# ?IterativeImputer

imp_mean = IterativeImputer(random_state=0) 
impute_df[num_columns] = imp_mean.fit_transform(impute_df[num_columns])

pd.isna(impute_df[num_columns]).sum().sum()
```

`from sklearn.experimental import enable_iterative_imputer`:이 부분이 필요한 이유는, 해당 기능이 아직 공식 기능이 아니기 때문이다.

IterativeImputer에서 주로 건들 파라미터는 `max_iter`와 `tol`이다. 이 두 파라미터를 조정함으로써 imputation에 걸리는 시간이 지나치게 오래 걸리지 않도록 조정할 수 있다.

## 범주형 변수의 결측치에 대한 처리

### Mode

범주형 변수의 결측치에 적용하는 방법으로서, 최빈값으로 결측치를 채우는 것을 의미한다.

```python
mode_df = data.copy()

imputer = SimpleImputer(strategy='most_frequent')
imputer.fit(mode_df[cat_columns])
mode_df[cat_columns] = imputer.transform(mode_df[cat_columns])

pd.isna(mode_df[cat_columns]).sum().sum()
```

# Encoding

인코딩이란 범주형 변수를 수치형 변수로 나타내는 것을 말한다. 범주형 데이터는 종종 문자형태이기도 한데, 컴퓨터가 숫자로만 데이터를 처리할 수 있기 때문이다.

## Label Encoding

라벨 인코딩은 n개의 범주형 데이터를 `0`~`n-1` 사이의 수로 표현하는 간단한 방법이다.

```
BMW : 0
VOLVO : 1
KIA : 2
```

라벨 인코딩은 간단한 방법이지만, '소형'과 '중형'이라는 의미의 차이를 반영하지는 않는다. 만약 순서가 중요한 범주형 데이터라면, Ordinal Encoding을 사용해야 한다.

[https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html?highlight=label encoder#sklearn.preprocessing.LabelEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html?highlight=label%20encoder#sklearn.preprocessing.LabelEncoder)

```python
data = pd.read_csv(example_file)
label = pd.DataFrame(data['OC'])

label.head() # 모델은 숫자를 요구한다.
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2023.png)

```python
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()

le.fit(label)

# 
le.classes_ # array([' close', 'open'], dtype=object)
```

맨 끝에 언더라인(`_`)이 생기는 것은 fit이 수행된 후에 생성된다.

Encoding의 번호 부여는, fit이 끝난 LabelEncoder 객체의 classes_ 속성에 있는 인덱스 순서대로 부여된다. 따라서, 위 결과에 의하면 close부터 0으로 인코딩 처리된다. 

이제, 범주형 변수를 실제 수치형 변수로 변환하자.

```python
label_encoded = le.transform(label)
```

---

```python
le_df = pd.DataFrame(label_encoded, columns = ['label_encoded'])

result = pd.concat([label, le_df], axis=1)
result.sort_values('label_encoded', inplace=True) # 특정 컬럼을 기준으로 정렬할 때 sort_values

result.head(20) # close가 개수가 적은 imbalanced 데이터다.
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2024.png)

## One-Hot Encoding

원핫 인코딩은 n개의 범주형 데이터를 n개의 비트(0,1) 벡터로 표현한다. 

```
BMW : [1, 0, 0]
VOLVO : [0, 1, 0]
KIA : [0, 0, 1]
```

원핫 인코딩으로 범주형 데이터를 나타내면, 각 벡터끼리는 그 내적 값이 0이 나온다. 이 말은 곧, 서로 다른 범주 데이터 끼리는 독립적인 관계를 형성하고 있다는 것을 의미한다.

단, 원핫 인코딩 방식은 두 가지 문제가 있다.

1. **차원의 저주**
    
    차원이 늘어날수록(feature의 개수가 많아질수록) 모델의 성능은 낮아진다.
    
2. **메모리 낭비 ⇒** `OneHotEncoder`의 `sparse` 파라미터를 `False`로 한다.
    
    0값이 대부분인 sparse matrix를 형성하고 이 정보를 메모리에 유지시켜야 한다.
    

[sklearn.preprocessing.OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder)

참고로, scikit-learn의 One-hot Encoder는 ndarray 행렬에 대해 정상적으로 작동하기 떄문에, Pandas DataFrame을 ndarray를 추출해서 사용해야 한다.

```python
from sklearn.preprocessing import OneHotEncoder
ohe = OneHotEncoder(sparse=False) 
ohe.fit(label)

one_hot_encoded = ohe.transform(label)
one_hot_encoded
# array([[0., 1.],
#        [0., 1.],
#        [0., 1.],
#        [0., 1.],
#        [1., 0.],
#        [0., 1.],
# 	     ....
```

`OneHotEncoder`의 `sparse` 파라미터를 `False`로 하면, 값이 있어야 할 위치와 해당 값만 표현하기 때문에, 메모리 효율성을 높일 수 있다. 

default값은 True이다.(아래 코드 참고)

```python
from sklearn.preprocessing import OneHotEncoder
ohe_sparse = OneHotEncoder()
ohe_sparse.fit(label)

ohe_hot_sparse = ohe_sparse.transform(label)
ohe_hot_sparse
# <301x2 sparse matrix of type '<class 'numpy.float64'>'
#	with 301 stored elements in Compressed Sparse Row format>
```

OneHotEncoder의 결과로 나온 객체의 내용을 보려면 array로 바꿔야 한다.

```python
ohe_hot_sparse.toarray()

# array([[0., 1.],
#        [0., 1.],
#        [0., 1.],
#        [0., 1.],
#        [1., 0.],
#        [0., 1.],
# 	     ....
```

---

scikit-learn에 의해 One-Hot Encoding된 라벨 데이터를 다시 Pandas DataFrame으로 바꿔준다.

```python
ohe_df = pd.DataFrame(one_hot_encoded, columns = ohe.categories_[0])
result = pd.concat([label, ohe_df], axis=1)

result.head(10)
```

![Untitled](Data%20Preprocessing%20eb7a0990c3344d32922f9a09055b3501/Untitled%2025.png)

## Ordinal Encoding

카테고리 간의 순서가 잘못 부여되면 모델이 잘못된 관계를 학습할 수 있다. 따라서 올바른 순서를 결정해야 한다. 

또한, 오디널 인코딩은 카테고리 간의 거리가 일정함을 가정한다. 따라서, 도메인에 따라 Ordinal Encoding을 변형해야 할 수도 있다.

```python
import pandas as pd
from sklearn.preprocessing import OrdinalEncoder

# 예시 데이터프레임 생성
data = pd.DataFrame({'category': ['A', 'B', 'C', 'B', 'A', 'C']})

# OrdinalEncoder 객체 생성
encoder = OrdinalEncoder()

# 범주형 변수에 대해 Ordinal Encoding 수행
data_encoded = encoder.fit_transform(data[['category']])

# 인코딩된 데이터를 데이터프레임으로 변환하여 기존 데이터프레임에 추가
data['category_encoded'] = data_encoded

# 결과 확인
print(data)
```