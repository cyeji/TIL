
## Transaction 격리성 레벨
### READ UNCOMMITED
트랜잭션이 수정한 데이터를 즉시 반영하는 방식. 즉, 다른 트랜잭션도 "커밋되지 않은 변경 사항"을 볼 수 있게 됩니다.

### READ COMMITED
"커밋된 최신 데이터"를 보여주는 격리 수준입니다. 즉, 트랜잭션이 조회할 떄마다 최신 COMMIT된 데이터를 가져와야 합니다.

### REPEATABLE READ
트랜잭션이 끝날 때까지 동일한 데이터를 조회하도록 보장하는 격리 수준입니다. 이를 위해 트랜잭션이 시작될 때, UNDO LOG의 데이터를 유지하면서 같은 결과를 반환합니다.

### Serializable (가장 엄격한 격리성)

1. Transaction A가 계좌 목록을 조회
2. Transaction B가 새 계좌를 추가할려고 하면, Transaction A가 끝날때까지 대기해야됨

모든 트랜잭션이 직렬화되어 실행됩니다. 동시성을 포기하는 대신 데이터 정합성을 완벽하게 보장합니다. 이로인해 성능이 크게 저하될 수 있습니다.



| 격리성 레벨           | Dirty Read | Non-Repeatable Read | Phantom Read | 성능      | 예시       |
| ---------------- | ---------- | ------------------- | ------------ | ------- | -------- |
| Read Uncommitted | 발생         | 발생                  | 발생           | 최고 성능   | 로그 시스템   |
| Read Committed   | 방지         | 발생                  | 발생           | 일반적인 성능 | 게시판      |
| Repeatable Read  | 방지         | 방지                  | 발생           | 적당한 성능  | 은행 계좌 조회 |
| Serializable     | 방지         | 방지                  | 방지           | 최저 성능   | 없음       |

> phantom read (팬텀리드)
> 트랜잭션 도중 새로운 행이 삽입되어, 같은 WHERE 조건으로 다시 조회했을 때 결과 집합이 달라지는 현상을 말합니다.
> 하지만 MYSQL의 MVCC(REPEATABLE READ)에서는, 트랜잭션 시작 지점의 스냅샷을 계속 참조하기 때문에, 중간에 Insert된 새로운 버전을 다른 세션에서 보는 것을 기본적으로 차단합니다.


### REDO LOG

- UPDATE 실행 -> 디스크에 바로 반영하지 않고, REDO LOG에 기록
- COMMIT하면 -> REDO LOG를 보고 디스크에 정식 반영
- 만약 시스템이 갑자기 꺼졌다면 ? -> REDO LOG를 보고 다시 데이터를 복구할 수 있습니다.

### UNDO LOG
- 백업본 역할
- UPDATE 실행 -> 기존 데이터를 UNDO LOG(백업본)로 저장
- 만약 ROLLBACK 하면 ? -> UNDO LOG를 보고 원래 상태로 복구
- COMMIT 하면 ? -> UNDO LOG는 필요 없어지므로 삭제됨

