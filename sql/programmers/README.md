## SQL

## 날짜 조건 걸기

1. 특정 날짜 조회

```mysql

select *
from sample
where register_time = '2022-08-15'

```

2. 특정 날짜 이전 또는 이후 조회

```mysql

select * 
from sample
where register_time < '2022-08-15'

```

3. 특정 기간 포함한 데이터 조회

```mysql

select * 
from sample
where register_time between '2022-08-15' and '2022-12-31'

```
4. 