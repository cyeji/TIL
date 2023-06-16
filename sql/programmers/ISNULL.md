###  [나이 정보가 없는 회원 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131528)

USER_INFO 테이블에서 나이 정보가 없는 회원이 몇 명인지 출력하는 SQL문을 작성해주세요. 이때 컬럼명은 USERS로 지정해주세요.

```mysql

SELECT count(*) as users
from user_info
where age is null;

```

### [경기도에 위치한 식품창고 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131114)

FOOD_WAREHOUSE 테이블에서 경기도에 위치한 창고의 ID, 이름, 주소, 냉동시설 여부를 조회하는 SQL문을 작성해주세요. </br>
이때 냉동시설 여부가 NULL인 경우, 'N'으로 출력시켜 주시고 결과는 창고 ID를 기준으로 오름차순 정렬해주세요.

```mysql

SELECT warehouse_id, warehouse_name, address, IFNULL(freezer_yn,'N') as freezer_yn
from food_warehouse
where warehouse_name like '%경기%'
order by warehouse_id;

```

### [null 처리하기](https://school.programmers.co.kr/learn/courses/30/lessons/59410?language=mysql)

```mysql

SELECT animal_type, ifnull(name, "No name") as name, sex_upon_intake
from animal_ins;

```

### [저자 별 카테고리 별 매출액 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/144856)

2022년 1월의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(TOTAL_SALES = 판매량 * 판매가) 을 구하여, </br>
저자 ID(AUTHOR_ID), 저자명(AUTHOR_NAME), 카테고리(CATEGORY), 매출액(SALES) 리스트를 출력하는 SQL문을 작성해주세요. </br>
결과는 저자 ID를 오름차순으로, 저자 ID가 같다면 카테고리를 내림차순 정렬해주세요.

1. 1번 풀이
```mysql

select b.author_id, a.author_name, b.category, SUM((bs.sales * b.price)) as total_sales
from book as b
inner join author as a
on b.author_id = a.author_id
inner join book_sales as bs
on b.book_id = bs.book_id
where bs.sales_date between '2022-01-01' and '2022-01-31'
group by b.author_id, b.category
order by b.author_id , b.category desc

```

2. 2번 풀이

```mysql

select ab.author_id, c.author_name, ab.category, sum(sales*price) as total_sales
from (select * from book_sales as a natural join book as b where date_format(sales_date,'%m') = '01') as ab
inner join author as c
on ab.author_id = c.author_id
group by ab.author_id, c.author_name, ab.category
order by ab.author_id asc, ab.category desc;

```

### [성분으로 구분한 아이스크림 총 주문량](https://school.programmers.co.kr/learn/courses/30/lessons/133026)

```mysql

select ice.INGREDIENT_TYPE, sum(ice.TOTAL_ORDER) as total_order
from (select * from first_half as f natural join icecream_info as i) as ice
group by ice.INGREDIENT_TYPE
order by TOTAL_ORDER

```