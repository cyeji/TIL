 ## 스프링 배치 아키텍처
 
1. Application
- 스프링 배치 프레임워크를 통해 개발자가 만든 모든 배치 Job과 커스텀 코드 포함
- 개발자는 업무로직의 구현에만 집중하고 공통적인 기반 기술은 프레임워크가 담당

2. Batch Core
- Job을 실행, 모니터링, 관리하는 API로 구성
- JobLauncher, Job, Step, Flow등이 속한다.

3. Batch Infrastructure
- Application, Core 모두 공통 Infrastructure위에서 빌드한다.
- Job의 실행 흐름과 처리를 위한 틀 제공
- Reader, PRocessor, Writer, Skip, Retry등이 속한다.