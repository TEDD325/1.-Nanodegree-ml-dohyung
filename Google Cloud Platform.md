# Google Cloud Platform

태그: GCP, MLE

---

# 용어 정리

## 호스트

네트워크에 연결되어 있는 통신 가능한 모든 장치

## 인스턴스

클라우드에서 구동되는 서버 사이드 컴퓨터. 대부분의 인스턴스는 가상 머신으로 이루어져 있다.

## 컨테이너

애플리케이션의 코드, 라이브러리 및 애플리케이션 실행 환경을 구성하는 기타 종속 구성 요소가 포함된 소프트웨어 코드 패키지다.

운영 체제를 가상화하여 애플리케이션이 모든 플랫폼에서 독립적으로 실행될 수 있도록 한다.

단일 애플리케이션을 실행하는 데 필요한 리소스만 패키징하기 때문에 가상 머신보다 크기가 더 작다(MB 단위)
컨테이너 외부 환경에 대한 제어 권한이 적다.

컨테이너 이미지는 읽기 전용이며 컴퓨터 시스템에서 변경할 수 없다.

가상 머신은 동적으로 리소스 제어가 가능한 반면, 컨테이너는 최상의 구성이 완료된 후에 이에 대한 정적 정의를 제공한다.

새 기능을 자주 빌드, 테스트 및 릴리스하려면 가상 머신이 아닌 컨테이너를 사용하는 것이 좋다. 소프트웨어만 포함하기 때문에, 수정하고 반복하는 속도가 매우 빠르기 때문이다.

## 가상 머신

물리적 컴퓨터를 호스트 컴퓨터라고 하며 가상 머신을 게스트라고 한다. 하나의 물리 머신을 두고, 이 물리 머신에 가상화 기술을 도입하여 가상의 여러 컴퓨터를 구현한 컴퓨터가 가상 머신이다. 호스트 운영 체제에 영향을 미치지 않으면서 필요에 따라 게스트 운영 체제와 그 애플리케이션을 구성하고 업데이트할 수 있다.

가상 머신은 기반 물리적 시스템 인프라 자체를 가상화한다. 따라서 호스트의 하드웨어 리소스를 효율적으로 사용할 수 있다.

자체 운영 체제가 포함되어 있으므로 컨테이너보다 크기가 훨씬 더 크다(GB 단위).

전체 환경을 보다 효과적으로 제어할 수 있다.

개발자에게 애플리케이션 환경에 대한 제어 권한을 컨테이너보다 더 많이 부여한다. 따라서 개발자가 수동으로 시스템 소프트웨어를 설치하고, 구성 상태에 대한 스냅샷을 생성하고, 필요한 경우 이전 상태로 복원할 수 있다.

가상 머신은 풀 스택 시스템이라 컨테이너에 비해 구축하고 재생성하는 데 시간이 많이 걸린다. 또한, 환경을 재생성해야 하기 때문에 수정 내용을 검증하는 데에도 시간이 많이 걸린다.

## IP 주소

네트워크 상에서 컴퓨터를 식별할 수 있는 논리적인 주소

<aside>
💡 **ex)** 대한민국 서울특별시 강남구 봉은사로 100 …

</aside>

## 포트 번호

컴퓨터 외부에서, 접근하려는 컴퓨터 내부의 특정 프로그램을 찾을 때 사용한다. 또한, 컴퓨터 내부에서는 외부의 요청을 포트 번호를 통해 특정 프로그램으로 접근할 수 있게 해준다. 네트워크 통신을 하는 프로그램들은 모두 특정 포트를 듣고(listen) 있으며, 해당 포트번호로 요청을 보내면, 그 포트를 듣고 있는 프로그램과 통신할 수 있다.

> *“IP(컴퓨터)의 port(특정 프로그램)로 요청할거야”.*
> 

<aside>
💡 **ex) …** 202호

</aside>

## shell

컴퓨터에 명령을 직접 보낼 수 있는 프로그램. 많이 사용되는 shell은 bashell(바쉘), zshell(즤셀) 등이 있다.

# GCP

Google Cloud Platform

구글이 제공하는 클라우드 컴퓨팅 플랫폼

컴퓨팅, 스토리지, 네트워킹, 빅데이터, 머신러닝, 인공지능 등 다양한 서비스를 제공

기업들이 자체 애플리케이션을 개발, 배포, 관리하는 데 사용된다.

- Compute Engine: 서버 관련
- Cloud SQL: DB 관련
- Cloud Run: API 관련
- GCR 서비스: 도커 관련

## Compute Engine

클라우드에서 서버 컴퓨터를 구축할 때 사용하는 서비스
다양한 옵션의 가상 머신(VM) 서비스를 제공한다. -> 개발자의 필요에 맞는 컴퓨팅 환경을 구축할 수 있다.

## Cloud SQL

클라우드에서 데이터베이스를 구축 할 때 사용하는 서비스

MySQL, PostgreSQL 등 다양한 데이터베이스 엔진을 완전 관리형 서비스*로 제공한다. → 개발자는 데이터베이스 관리에 대한 부담 없이 애플리케이션 개발에만 집중할 수 있다.

### *완전 관리형 서비스

서버의 DB 설정을 알아서 쉽게 해주는 것

Cloud SQL은 데이터베이스 엔진의 설치, 설정, 유지 관리, 백업 등을 자동으로 처리하여 개발자가 데이터베이스 관리에 대한 부담 없이 애플리케이션 개발에만 집중할 수 있게 한다. 

개발자는 필요한 경우 데이터베이스에 대한 설정을 변경하거나 모니터링할 수 있지만, 기본적인 운영 관리는 Google이 수행한다.

## Cloud Run

도커 이미지를 기반으로 서버를 구동할 수 있도록 도와주는 서비스

서버리스 컴퓨팅 서비스를 제공한다. → 개발자가 코드만 제공하면 Google에서 자동으로 서버를 프로비저닝*하고 관리하며, 코드가 실행될 때에만 비용을 지불한다.

- 무중단 배포* 등이 가능하다.
- 트래픽 관리/제어 및 여러 컨테이너로 로드밸런싱
- 버그로 인한 데드에 대응하기 위한 자동 스케일링 기능

### *프로비저닝

컴퓨터 시스템을 준비하고 구성하는 과정. 클라우드 컴퓨팅에서는 가상 머신을 생성하고 설정하는 과정을 포함한다. Cloud Run을 이용하면, 개발자가 코드를 제공하면 Google이 서버를 자동으로 프로비저닝한다.

### *무중단 배포

특정 서버를 업데이트해야 할 때, 이미 들어온 요청은 기존의 업데이트 이전의 서버가 처리할 수 있게 살려두되, 그 다음의 요청들부터는 업데이트 버전의 다른 서버가 처리하게 하여 유저 입장에서는 서비스가 계속 끊어지지 않는 경험을 할 수 있게 하는 기술

## Google Container Registry(GCR)

Google Cloud Platform에서 제공하는 도커 컨테이너 이미지 저장 및 관리 서비스

개발자는 GCR을 사용하여 컨테이너 이미지를 저장, 관리, 배포할 수 있다. 모든 게 도커 이미지화 된다.

회사 코드 등의 민감한 코드를 public 도커 레포에 올리는 것 대신에 사용할 수 있다.

# GCP 개발 환경 구성

## 가입 및 설정

한 계정 당 300달러 크레딧으로 90일 동안만 무료로 사용 가능

- [**https://cloud.google.com/?hl=ko](https://cloud.google.com/?hl=ko) 접속**

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled.png)

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%201.png)

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%202.png)

- 카드 정보를 입력하더라도 청구되지 않음
- **한글 이름으로 작성해야 함**

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%203.png)

여기까지 됐으면 그냥 **닫기**를 누른다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%204.png)

완료된 모습

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%205.png)

상단 중앙 검색바를 통해 필요한 서비스 페이지로 이동 가능하다.

이 노트에선 VM 인스턴스를 만들기 위해 **Compute Engine API**로 이동한다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%206.png)

일부 서비스들은 별도 권한이 필요하다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%207.png)

## Compute Engine을 이용하여 서버 생성

**Compute Engine → VM인스턴스 → 인스턴스 만들기**

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%208.png)

서버를 생성할 지역, 서버의 스펙, 스토리지 용량 등을 설정할 수 있는 페이지다.

- Zone: 분산된 데이터 센터 중 한 곳을 고르는 것
- Machine configuration
    - 일반적인 용도 → **범용**
    - 많은 계산, 시뮬레이션, 고CPU연산 → 컴퓨팅 최적화
    - 데이터 빠른 처리/리딩 → 메모리 최적화
    - 많은 디스크I/O → 스토리지 최적화
- Series
    - **N1**이 제일 저렴
    - 원하는 목적에 따라서 고르도록 한다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%209.png)

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2010.png)

**g1-small 옵션과 Standard를 선택한다.**

- **Standard**: 서버를 항상 띄워놓기. On demand
- **Spot**: 다른 서버로 부하가 걸릴 경우, 리소스를 빼앗길 수 있지만 가격이 비교적 저렴하다. 서버 다운 가능성 항상 있다.

예상 가격은 월 32.44 달러임을 볼 수 있다.

이러한 설정들은 나중에 변경 가능하다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2011.png)

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2012.png)

운영체제 종류와 버전을 선택한다.

***LTS**: Long Term Support. 안정화되어 오랫동안 지원되는 버전

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2013.png)

- HTTP: 80포트
- HTTPS: 433포트

추후 API 요청을 처리하기 위해서 HTTP 트래픽 허용이 필요하다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2014.png)

프라이빗 네트워크를 설정하는 공간

이 노트에선 설정하지 않는다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2015.png)

외장 하드처럼 추가 디스크 부착 여부를 선택할 수 있다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2016.png)

추후 도커 이미지를 쓸 것이며, Automation은 도커 이미지를 쓴다면 필요없는 자동화 기능을 제공하는 부분이다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2017.png)

위와 같이 Status가 초록색 체크 라디오 버튼으로 뜨면 성공적으로 인스턴스가 생성된 것이다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2018.png)

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2019.png)

SSH 접속을 허용하여 접근하면 위와 같이 뜬다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2017.png)

External IP는 해당 VM 인스턴스 외부에서 접근할 수 있는 IP주소로서, SSH 접근 시 필요하다.

---

## 클라우드 서버 외부에서 SSH 접근하기

외부에서 원격으로 서버 컴퓨터로 접속하려면 SSH(Secure Shell) 프로토콜을 사용한다.

방식은 두 가지다.

- 접속하려는 서버의 계정과 PW를 아는 경우
- SSH key를 미리 등록하는 경우 → 굳이 매번 계정명과 PW를 입력하여 접속할 필요가 없다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2020.png)

SSH로 접속하기 위해서는 서버 쪽에 우리의 SSH Key를 미리 등록해야 한다. → 파일로 저장됨

ssh-keygen 명령어를 통해 만들 수 있다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2021.png)

```bash
ssh-keygen -t rsa -f ~/.ssh/*dohk325-gcp-01* -C *ubuntu* -b 2048
```

- `t`: 암호화 type
- `f`: 키를 저장할 경로
- `~`: 홈 경로
- `C`: 메모. 코멘트
- `b`: 비트수

`--help` 옵션을 통해 더 많은 옵션에 대한 설명을 볼 수 있다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2022.png)

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2023.png)

dohk325-gcp-01**.pub**: 공개키 파일

dohk325-gcp-01: .pub 확장자가 붙지 않은 파일은 비밀키로서 공유되어선 안됨

서버로의 요청 시엔 비밀키로 암호화되어 서버로 요청이 전달된다.

서버는 암호화된 요청을 공개키로 복호화한다.

`cat dohk325-gcp-01.pub` 명령어로 해당 파일의 텍스트(즉, **공개키)를 콘솔에 출력하여 복사**해둔다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2024.png)

**Compute Engine → 메타데이터 → SSH KEYS 탭 → SSH 키 추가**

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2025.png)

앞서 **복사해 둔 SSH 공개키를 붙여넣는다.** → 서버에 공개키를 제공하는 것으로서, SSH 통신이 가능하게 한다.

---

이제 터미널에서 xternal IP 정보로 해당 GCP VM 인스턴스에 SSH로 접속해보자.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2026.png)

<aside>
💡 .pub 파일로는 애초에 접속이 허용조차 안 된다.

서버는 클라이언트가 제공한 공개키와 일치하는 개인키를 가진 클라이언트에게만 접속을 허용하기 때문이다.

</aside>

---

## 로컬의 IDE에서 원격의 파이썬 인터프리터 사용하도록 구성하기

SSH 접속이 잘 되는 것을 확인했으니, 이제 원하는 IDE 환경에서 클라우드 인스턴스에서의 작업이 가능하도록 해야한다.

나는 PyCharm을 좋아하기 때문에 PyCharm에 구성해보고자 한다.

### 서버에 miniconda 설치하기

![[https://docs.anaconda.com/free/miniconda/](https://docs.anaconda.com/free/miniconda/)](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2027.png)

[https://docs.anaconda.com/free/miniconda/](https://docs.anaconda.com/free/miniconda/)

**Miniconda3 Linux 64-bit 링크를 복사한다.**

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2028.png)

서버에 접속하여 `**wget` 명령으로 복사한 링크의 파일을 다운로드 받는다.**

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2029.png)

다운로드 된 파일은 별도의 파일 권한 수정 없이 bash 명령으로 실행할 수 있다. **실행한다.**

설치 과정을 간단히 요약하면 다음과 같다.

- `Enter` → `q`(라이센스의 맨 밑으로 이동) → `yes`(상업적으로 쓸 거면 어떻게 써야하는지 등에 대한 내용) → 쉘에서 기본으로(base로) 활성화 시킬 것인가 여부: 기존 것이 있다면 꼬이지 않도록 no. 여기선 `yes`. 나중에 수정 가능하다([쉘 base 설정 그림](https://www.notion.so/Google-Cloud-Platform-eddd37624f314662a47e2f82c4c6ecd8?pvs=21) 참고)→ miniconda 설치 경로는 수정할 필요 없다.
    
    ![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2030.png)
    

![**쉘 base 설정 그림**
나중에라도 false로 설정 가능하다.](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2031.png)

**쉘 base 설정 그림**
나중에라도 false로 설정 가능하다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2032.png)

기본 활성화를 `yes`로 하였기 때문에 `(base)`가 뜬 것을 볼 수 있다.

---

miniconda를 이용하여 가상 환경을 생성해준다.

```bash
conda create -n mle_course python=3.11 -y
```

base 환경에서 생성한 환경으로 이동한다.

```bash
conda activate mle_course
```

파이썬 의존성 관리를 위해 **poetry**를 설치한다.

```bash
pip install poetry
```

ml_basic 폴더를 만들고, poetry를 초기화한다.

```bash
$ mkdir ml_basic
$ cd ml_basic
$ poetry init
```

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2033.png)

각종 poetry 옵션이 나오는데, 위의 ‘Compatible Python versions’를 3.11로 수정하지 말고 그대로 진행하는 것 빼고는 위와 동일하게 진행하면 된다.

옵션 설정이 완료되면 해당 폴더 내에 toml 확장자의 파일이 생성된다.

<aside>
💡 **poetry로 의존성을 관리하는 이유**

의존성은 conda와 pip으로도 설치 및 제거 등이 가능하지만, 어떤 라이브러리는 conda로, 어떤 라이브러리는 pip으로 설치만 가능한 상황 등으로 인해, 보통은 conda와 pip 둘 모두를 동시에 번갈아가면서 필요에 따라 사용한다. 하지만 이럴 경우 패키지가 쉽게 꼬인다. 이를 해결하기 위한 대안이 peotry다.

</aside>

각종 필요한 의존성들을 설치한다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2034.png)

설치된 의존성들은 `pip list` 로 확인 가능하다.

### PyCharm에 서버의 파이썬 인터프리터 연결하기

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2035.png)

먼저, 새로운 프로젝트를 만들어준다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2036.png)

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2037.png)

원격 서버의 파이썬 인터프리터를 추가하기 위해 오른쪽 하단의 파이썬 인터프리터를 클릭하고 Add New Interpreter → On SSH를 클릭한다.

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2038.png)

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2039.png)

SSH 정보를 입력한다. 이미 등록되어 있는 SSH 접속정보의 경우에는 Existing을 통해 확인 가능하다. 오른쪽의 `…`을 누르면 자세한 설정이 가능하다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2040.png)

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2041.png)

Next

---

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2042.png)

Conda Environment → Use existing environment → mle_course

Sync folders는 서버 상의 원하는 경로를 지정

---

위 과정까지 하면 완료된다.

## GCP 인스턴스 SUSPEND하기(필수)

사용하지 않는 시간의 크레딧 낭비를 최소화하기 위해서는 인스턴스를 SUSPEND시켜야 한다.

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2043.png)

![Untitled](Google%20Cloud%20Platform%20eddd37624f314662a47e2f82c4c6ecd8/Untitled%2044.png)

정지시키면 위와 같이 노란색의 라디오 박스로 STATUS가 변경된다.

단, SUSPEND하고 다시 재개하면 External IP 주소가 변경됨에 주의한다.