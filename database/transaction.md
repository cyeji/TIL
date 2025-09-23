
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

# 락이란?

## 비관적 락(Pessimistic Lock)
하나의 트랜잭션이 데이터를 수정하는 동안, 다른 트랜잭션은 해당 데이터를 수정할 수 없다.

- 장점 : 안전하게 데이터 수정을 보호
- 단점 : 대기 시간이 길어질 수 있음

## 낙관적 락(Optimistic Lock)


장점 : 락을 걸지 않으므로 충돌이 많지 않으면 빠름
단점 : 충돌이 발생하면 다시 시도해야 함.

## 테이블락
MySQL InnoDB는 기본적으로 "행 단위(Row-Level Lock)"를 사용하지만, 테이블 단위의 락(Table Lock)도 존재합니다.
테이블 락은 테이블 전체를 잠그는 방식으로 동시성이 낮아지는 대신 데이터 정합성을 강력하게 보장할 수 있습니다.
테이블 락이 걸려 있는 동안, 다른 트랜잭션은 해당 테이블에 대한 SELECT, INSERT, UPDATE, DELETE 등의 연산을 수행할 수 없습니다.

```SQL
LOCK TABLES table_name READ; -- 읽기 전용 락
LOCK TABLE table_name WRITE; -- 쓰기 락
UNLOCK TABLES; -- 락 해제
```

- 실제 사용 예시
1. 테이블을 백업 할때 (READ LOCK)
- 데이터를 백업하는 동안, 테이블이 변경되면 안된다.
- 따라서, 테이블 전체에 READ LOCK을 걸고 백업을 수행할 수 있습니다.

2. 데이터 마이그레이션 시 (WRITE LOCK)
- 특정 테이블의 데이터를 마이그레이션 할 때, 데이터를 읽거나 변경되면 안됨
- 따라서, 테이블 전체에 WRITE LOCK을 걸고 마이그레이션 수행

## 배타 락(Exclusive Lock, X-Lock)
한 트랜잭션이 데이터를 수정하는 동안, 다른 트랜잭션이 수정하지 못하도록 막음.
이제 다른 트랜잭션은 묶어놓은 것들에 대해 UPDATE를 실행할 수 없음.

X-Lock이 걸린 레코드에는:
- 다른 트랜잭션이 X-Lock(배타락)을 획득할 수 없음.
- 다른 트랜잭션이 S-Lock(공유락)을 획득할 수 없음.
- 다른 트랜잭션이 해당 레코드를 수정(UPDATE/DELETE)할 수 없음
- 단, 락 없는 일반 SELECT 쿼리는 허용됨 (MySQL의 MVCC 특성 때문)

```SQL
START TRANSACTION;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE; -- X-Lock 적용
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

## 공유 락(Shared Lock, S-Lock)
여러 트랜잭션이 데이터를 조회하는 동안, 데이터를 수정하지 못하도록 막음

S-Lock은 공유 잠금이라고도 불리며, 이름 처럼 여러 트랜잭션이 동시에 같은 리소스에 대한 S-Lock을 가질 수 있음.
다른 트랜잭션과 동시에 lock을 잡을 수 있다는 것은 다른 트랜잭션도 단순 조회를 할 경우이기 때문.
즉, 여러 트랜잭션이 데이터를 동시에 읽을 수 있지만, 수정은 불가능 합니다.

S-Lock이 걸린 레코드에는:
- 다른 트랜잭션도 S-Lock을 획득할 수 있음 (공유 가능)
- 다른 트랜잭션이 X-Lock을 획득할 수 없음
- 다른 트랜잭션이 해당 레코드를 수정(UPDATE/DELETE)할 수 없음.
- 락 없는 일반 SELECT 쿼리는 허용됨.