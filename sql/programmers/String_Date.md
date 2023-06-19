## [카테고리 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131529)

```mysql

select left(product_code,2) as category, count(*) as products
from product
group by category;

```

## [특정 옵션이 포함된 자동차 리스트 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157343)

```mysql

SELECT CAR_ID, CAR_TYPE, DAILY_FEE, OPTIONS
FROM CAR_RENTAL_COMPANY_CAR
where options like '%네비게이션%'
order by car_id desc

```

## [조건에 맞는 사용자 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164670)

```mysql

select ugu.USER_ID as USER_ID, 
       ugu.NICKNAME as NICKNAME ,
       concat_ws(' ',CITY,STREET_ADDRESS1,STREET_ADDRESS2) as 전체주소 ,
       concat_ws('-',left(tlno,3),substr(tlno,4,4),right(tlno,4)) as 전화번호
from USED_GOODS_BOARD as ugb
inner join USED_GOODS_USER as ugu
on ugb.WRITER_ID = ugu.USER_ID
group by user_id
having count(*) >= 3
order by user_id desc

```

## [대여횟수가 많은 자동차들의 월별 대여횟수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151139)

```mysql

SELECT month(start_date) as MONTH, car_id , count(*) as RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE CAR_ID = ANY(SELECT CAR_ID 
                 FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY 
                 WHERE start_date BETWEEN '2022-08-01' AND '2022-10-31'
                 group by CAR_ID 
                 having count(*) >= 5
                ) 
AND START_DATE BETWEEN '2022-08-01' AND '2022-10-31'
group by month, car_id
having records >0
order by MONTH asc, car_id desc;

```