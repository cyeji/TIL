
## 도커를 쓰는 이유

1. 프로그램을 다운 받는 과정을 간단하게 만들기 위해서

- 기존 </br>
갖고 있는 서버, 패키지 버전, 운영체제 등 따라 프로그램 설치하는 과정 중에 </br>
많은 에러들이 발생하게 됩니다. 또한 설치과정이 다소 복잡

## 도커란 무엇인가?

컨테이너를 사용해 응용프로그램을 더 쉽게 생성, 배포, 실행 할 수 있도록 설계된 도구이며, </br>
컨테이너 기반의 오픈소스 가상화 플랫폼이며 생태계입니다.

### 서버에서의 컨테이너 개념

컨테이너 안에 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공해 </br>
프로그램의 배포 및 관리를 단순화 </br>
일반 컨테이너 개념에서 물건을 손쉽게 운송 해주는 것 처럼 프로그램을 손쉽게 이동, 배포, 관리를 할 수 있게 해줍니다.

## 도커 이미지와 컨테이너 정의

도커 이미지는 프로그램을 실행하는데 필요한 설정이나 종속성을 가지고 있으며 도커이미지를 이용해서 컨테이너를 생성하며 </br>
도커 컨테이너를 이용하여 프로그램을 실행합니다.

## 도커 사용 흐름

1. 먼저 도커 CLI 커맨드 입력
2. 도커 서버(도커 Daemon)이 그 커맨드를 받아서 그것에 따라 이미지를 생성하거나 컨테이너를 실행하는 등 모든 작업을 실행
   - CLI 커맨드를 통해 컴퓨터 내부로 이동 도커 클라이언트에 전달
   - 도커 클라이언트는 도커 서버에 전달
   - 이미지 Cache 보관소에서 요청받은 정보가 있는지 확인
   - 없을 경우 이미지 저장소인 도커 허브에서 가져옴

## 도커와 가상화 기술 차이를 통한 컨테이너 이해

- 가상화 기술 나오기 전 </br>
한대의 서버를 하나의 용도로만 남은 서버 공간은 그대로 방치, 하나의 서버에 하나의 운영체제, 프로그램만을 운영하여 안정적이지만 비효율적
- 하이퍼바이저 기반의 가상화 기술 출현
논리적으로 공간을 분할하여 VM이라는 독립적인 가상 환경의 서버 이용 가능 </br>
하이퍼바이저는 호스트시스템에서 다수의 게스트OS를 구동할 수 있게 하는 소프트웨어, 하드웨어를 가상화하면서 하드웨어와 VM을 </br>
모니러팅하는 중간 관리자

- 하이퍼바이저 기술 종류 2가지

| 이름          | 설명                                                                                                                     | 비고      |
|-------------|------------------------------------------------------------------------------------------------------------------------|---------|
| 네이티브 하이퍼바이저 | 하이퍼 바이저가 직접 하드웨어를 제어하기에 자원 효율적으로 사용가능하며 </br> 별도의 호스트OS가 없으므로 오버헤드가 적다. </br> 하지만 여러 하드웨어 드라이버를 세팅해야되므로 설치 어렵다.        |         |
|호스트형 하이퍼바이저| 일반적인 소프트웨어처럼 호스트OS위에서 실행되며, 하드웨어 자원을 VM내부의 게스트OS에 </br> 에뮬레이트하는 방식으로 오버헤드가 크다. </br> 하지만 게스트OS종류에 대한 제약이 없고 구현이 다소 쉽다. | 현재 많이쓰임 |

하이퍼바이저에 구동되는VM은 VM마다 독립된 가상 하드웨어 자원을 받습니다. </br>
논리적으로 분리되어 있어서 한 VM에 오류가 발생해도 다른 VM으로 퍼지지 않는다는 장점이 있습니다.

- 가상화 기술과 컨테이너 기술의 공톰정과 차이점
  - 공통점 </br>
  도커 컨테이너와 가상머신은 기본 하드웨어에서 격리된 환경 내 어플리케이션을 배치하는 방법
  - 차이점 </br>
  가장 큰 차이점은 격리된 환경을 얼마나 격리시키는지 차이

- 가상머신 </br>
가상머신과 함께 VM내부에서 실행되는 모든 것은 호스트 운영체제 또는 하이퍼바이저와 독립되어 있다. </br>
가상머신플랫폼은 특정 VM에 대한 가상화 프로세스를 관리하기 위해 프로세스를 시작하고, </br>
호스트 시스템은 그것의 하드웨어 자원의 일부를 VM에 할당한다. </br>
그러나 VM과 근본적으로 다른 것은 시작시간에 이 VM환경을 위해 새롭고 특정VM만을 위한 커널을 부팅하고 운영체제 프로세스 세트를 시작한다는 것 </br>
이것은 응용 프로그램만 포함하는 일반적인 컨테이너보다 VM의 크기를 훨씬 크게 만든다. </br>
OS까지 가상화함. 이러한 방법은 비교적 간단할 수 있지만 굉장히 느리다.

- 도커컨테이너 </br>
컨테이너가 제공하는 격리기능 내부에 샌드박스가 있지만 여전히 같은 호스트의 다른 컨테이너와 동일한 커널을 공유합니다. </br>
결과적으로 컨테이너 내부에서 실행되는 프로세스는 호스트 시스템(모든 프로세스를 나열할 수 있는 충분한 권한있음)에서 볼 수 있다. </br>

- 도커컨테이너 격리 방법 </br>
CGroup & Namespace : 컨테이너와 호스트에서 실행되는 다른 프로세스 사이에 벽을 만드는 리눅스 커널 기능 </br>
- CGroup </br>
CPU, 메모리, Network BandWidth, HD I/O등 프로세스 그룹의 시스템 리소스 사용량 관리
- Namespace </br>
하나의 시스템에서 프로세스를 격리시킬 수 있는 가상화 기술 </br>
별개의 독립된 공간을 사용하는 것처럼 격리된 환경을 제공하는 경량 프로세스 가상화 기술


