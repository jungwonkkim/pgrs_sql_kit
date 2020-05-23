# 프로그래머스 SQL Kit 내가 썼던 답 + 간단한 해설

@jungwonkkim on GitHub

제곧내네요. 깃허브에 제곧내라고 써도 되나요?

- 문제는 이곳 ! https://programmers.co.kr/learn/challenges?tab=sql_practice_kit

- 원래 PostgreSQL 과 sqlite를 씁니다! 이 문제에서는 MySQL을 썼지만, 표준 SQL을 썼기 때문에 크게 상관은 없을 것이라고 생각합니다. MySQL만 가능한 쿼리를 썼다면 나는 MySQL 천재다 (?????)
- 문제의 저작권은 모두 프로그래머스에 있습니다. 
- 컴알못에 의지라곤 가지고 있지도 않은 제가 할 말은 절대 아니지만 SQL은 사실 굉장히 직관적이기 때문에 해당 DML, DDL, DCL 문의 쓰임을 제대로 알고 있다면 풀 수 있는 문제가 대부분입니다. 모든 프로그래밍, 코딩이 다 그렇듯이 프로그래밍 SQL Kit이 그대로 시험문제로 나와서 30분 안에 제출해야하는 상황이 아닌이상, 그리고 당연히 다 찾아보고 오셨으리라 믿지만, 먼저 충분히 고민하는 시간을 가지는 것이 좋은 것 같습니다!
- 제가 쓴 답이기 때문에 프로그래머스가 원하는 output을 내놓는 답이긴 해도 효율적인 면에서 결코 좋은 답이라고 말할 수 없습니다!! 저는 너무 경고 했습니다..  그런 의미에서 이 문제 이게 나은 것 같은데 라는 이슈와 댓글 너무 감사하게 받겠습니다ㅠㅠ



# SELECT

 SQL의 기본 중의 기본이죠. 이걸 할 줄 모르면 SQL을 할 수 있다고 할 수 없습니다. 

```sql
SELECT 보고싶은 칼럼 FROM table명 (WHERE 조건 GROUP BY 묶는 기준 ORDER BY 정렬 기준 LIKE 문자열 조건 LIMIT 보고싶은 갯수(OFFSET 앞에서부터 ~개 빼고)... ) ;
```



## 1. 모든 레코드 조회하기

```mysql
SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID;
```

id를 기준으로 테이블에서 모든 칼럼(`*`) 가지고 오기



## 2. 역순 정렬하기 

> ORDER BY 칼럼 이후, 정렬 기준을 오름차순으로 하고 싶을 때는 ASC(생략가능, default 값), 내림차순으로 하고 싶을 때는 DESC를 붙여줘야 한다. 



```mysql
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC;
```



## 3. 아픈 동물 찾기

>  특정 조건에 맞는 로우를 찾기 위해서는 `WHERE` 문을 사용해준다.

```mysql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION='Sick' ORDER BY ANIMAL_ID;
```



## 4. 어린 동물 찾기

> 어린 동물이라는 것은 ? 여기에서 INTAKE_CONDITION이 AGED 가 아닌 모든 동물임을 뜻한다(문제 화면의 각주를 참고해주세요) 
>
> WHERE 에서 ~가 아닐 때는 WHERE NOT ~~~으로 해주면 된다

```mysql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE NOT INTAKE_CONDITION="Aged" ORDER BY ANIMAL_ID;
```



## 5. 동물의 아이디와 이름

> 복수의 칼럼을 보고 싶다면 SELECT Column1, column2 ... 로 처리해주면 끝!

```mysql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS ORDER BY ANIMAL_ID;
```



## 6. 여러 기준으로 정렬하기

> 여러 기준으로 정렬을 할 때에는 정렬의 우선순위대로 ORDER BY 뒤에 써주면 된다.

```mysql 
SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME, DATETIME DESC;
```

위의 경우에는 NAME은 그대로 오름차순으로 , DATETIME은 내림차순으로 전개된다.





## 7. 상위 n개 레코드

> LIMIT은 다 정리되고 정렬된 테이블에서 상위 몇개만 가지고 오는 조건문입니다. OFFSET을 붙이면 앞에 몇개는 제치고 라는 뜻이 됩니다. 예를 들어 3~5번째 레코드를 가지고 오고 싶다면 LIMIT 3 OFFSET 2라고 써주면 됩니다. 

```mysql
SELECT NAME FROM ANIMAL_INS ORDER BY DATETIME LIMIT 1;
```



# SUM, MIN, MAX

사실 여기에 AVG, COUNT를 같이 붙여서 이 아이들을 집계함수라고 부릅니다. 집계함수는 계산을 수행하고 단일 값을 반환합니다. 

**주의할점 : count 함수를 제외한 집계함수는 Null값을 무시합니다.(min값이 null값이 되지 않는다는 뜻) 또한 SELECT 문 혹은 HAVING 절에만 쓸 수 있습니다. **

SELECT 문에서는 칼럼이 필요없는데, 해당 조건을 WHERE에서 쓰고 싶다면?

> 최대값, 최소값은 LIMIT 문을 써주면 됩니다. 이 방법이 싫거나 쓸 수 없을 때가 있다면, `SELECT * FROM ANIMAL_INS WHERE DATETIME = (SELECT max(DATETIME) FROM ANIMAL_INS);` 처럼 조건문 안에 서브쿼리로 SELECT 문을 하나 더 넣어주면 됩니다! 다른 방법도 있지만 쓰는 DBMS에 따라 다른 방법들이 있기 때문에 우리들의 친구 구글을 찾아봅시다. 

Having절이란?

> GROUP BY 절에 의해 생성된 결과 값 중 원하는 조건에 부합하는 자료만 보고자 할 때 사용하는 절입니다. 곧 문제에 나오기 때문에 직접 보는 것이 더 다가오기 쉬울 것 같습니다. 



## 1. 최댓값 구하기

>  가장 최근에 들어온 동물의 날짜를 구하는 것. 그 값을 가진 row 전체가 아닌 그 값을 원하는 것이기 때문에 집계함수 `max`를 써주면 된다!

```mysql
SELECT MAX(DATETIME) FROM ANIMAL_INS;
```



## 2. 최솟값 구하기

> 1번 문제를 풀었다면 이 문제도 당연히 어렵지 않게 풀 것이라고 생각합니다. 만약 1번문제를 풀었는데 2번문제를 풀 수 없다면 영어공책을 펴놓고 `min`을 10번 써보도록 합시다. 

```mysql
SELECT MIN(DATETIME) FROM ANIMAL_INS;
```





## 3. 동물 수 구하기

> COUNT는 위 대분류에 이름을 하고 있지 않지만, 집계함수이기 때문에 당연히 알아두어야 합니다. NULL을 무시하지 않는다는 것!

```mysql
SELECT COUNT(*) as count FROM ANIMAL_INS;
```

as `count`는 칼럼명을 `count`로 해준다는 뜻



## 4. 중복 제거하기

> 마지막 문제인데 저는 아직 `SUM`을 쓰지 않았고 쓸 계획도 없습니다. 어쩌죠? 
>
> 중복을 제거할 때는 `SUM`이 아닌 `DISTINCT`를 써주면 됩니다. 

```mysql
SELECT COUNT(DISTINCT(NAME)) FROM ANIMAL_INS;
```



# GROUP BY

그룹별로 묶어서 보여주는 표현문입니다! 말 그대로라서 덧붙일 말도 없습니다. 하나하나 문제를 보면서 언제 쓰이는 아이인지 보는 것이 빠를 것 같습니다. 이부분 부터 조금씩 sql문이 복잡해지기 때문에 적절하게 엔터를 사용해보겠습니다. SQL문은 엔터가 몇번이든 마지막에 세미콜론을 붙여주면 그 부분이 끝이라는 것!



## 1. 고양이와 개는 몇 마리 있을까

> 몇마리를 세려면 COUNT를 써야할 것만 같은데, 만약 그냥 세어주면 100이라는 전체 결과를 내어주게 됩니다. DISTINCT를 쓰면? (`*`가 아닌 `ANIMAL_TYPE`을 기준으로 한다는 가정하에) 2가 되겠죠(다른 동물이 있지 않는 이상). 그러니까 이 COUNT 결과를 ANIMAL_TYPE 별로 나눠주어서 출력해주어야 합니다.

```mysql
SELECT ANIMAL_TYPE, COUNT(*) AS count 
FROM ANIMAL_INS 
GROUP BY ANIMAL_TYPE 
ORDER BY ANIMAL_TYPE;
```



## 2. 동명 동물 수 찾기

> 동명이 있는 동물의 이름과 카운트를 내뿜으면 됩니다. 자 여기에서는 그룹을 이름으로 하는 것 까지는 알겠는데, 그 중에서 카운트가 1보다 큰 것을 뽑아내면 될 것 같습니다. 그룹으로 되어있을 때 쓰는 조건절? 아까 스리슬쩍 언급한 `HAVING` 절입니다!



```mysql
SELECT NAME, COUNT(*) as COUNT
FROM ANIMAL_INS
GROUP BY NAME HAVING COUNT(NAME)>1
ORDER BY NAME;
```

NAME으로 다 묶어주는데 이름이 하나 이상인 아이들만 출력해준다. 



**주의할 점**

아래와 같이 해도 테스트 케이스는 같은 결과를 출력하지만 틀렸다고 나옵니다. 왜일까요?

```mysql
SELECT NAME, COUNT(*) as COUNT
FROM ANIMAL_INS
GROUP BY NAME HAVING COUNT(*)>1
ORDER BY NAME;
```



정답은 `*`로 카운트를 세어줄 때에는 이름이 `NULL`값인 로우들도 제외하지 않기 때문입니다. 문제에 Null 값은 제외한다고 쓰여있기 때문에 `COUNT(NAME)`이라고 써줍니다.



## 3. 입양 시각 구하기(1)

> DATETIME 데이터타입에 관한 약간의 이해가 필요한 문제입니다. 
>
> https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html
>
> 사실 hour() 함수를 안다면 앞의 문제보다 로우 갯수만 많다할 뿐이지 사실 간단합니다.
>
> 다만 9시부터 19시까지이기 때문에 BETWEEN 구문을 추가해주도록 합시다

```mysql
SELECT hour(datetime) as HOUR,count(datetime) as 'COUNT'
FROM ANIMAL_OUTS
where hour(datetime) between 9 and 19
group by HOUR
order by HOUR;
```



## 4. 입양 시각 구하기(2)

> 문제는 오히려 3번보다 쉬울 것 같아서 3번문제의 9~19를 빼주었더니 잘못된 결과가 나옵니다. 왜냐하면 count 값이 0인 시간들이 아예 무시받기 때문입니다. 해당 Row가 아예 존재하지 않기 때문입니다. 
>
> https://www.sitepoint.com/community/t/how-to-find-count-for-every-hour-in-mysql/4688/9
>
> 위 사이트를 참고하여 각 hour의 임시테이블을 만들고 값을 조인해주었습니다 .벌써부터 Join을 쓰고 싶지 않았어... 제가 쓴 조인은 Left join!
>
> https://futurists.tistory.com/17
>
> (아니 MySQL은 outer join 지원이 안된다구여..?)
>
> 위문제의 경우 답이 정말 여러가지가 나올 수 있고 Oracle인경우  MySQL인 경우 각각 DBMS에만 있는 SQL을 사용한다면 정말 답이 많아질 수 있기 때문에 문제를 푸셨다고 해도 해당 문제 페이지의 질문하기 페이지를 활용하는 것도 추천합니다!



```mysql
-- 코드를 입력하세요
SELECT theHour, count(DISTINCT(theHour)) as COUNT
FROM  ( SELECT 0 AS theHour
         UNION ALL SELECT 1
         UNION ALL SELECT 2
         UNION ALL SELECT 3
         UNION ALL SELECT 4
         UNION ALL SELECT 5
         UNION ALL SELECT 6
         UNION ALL SELECT 7
         UNION ALL SELECT 8
         UNION ALL SELECT 9
         UNION ALL SELECT 10
         UNION ALL SELECT 11
         UNION ALL SELECT 12
         UNION ALL SELECT 13
         UNION ALL SELECT 14
         UNION ALL SELECT 15
         UNION ALL SELECT 16
         UNION ALL SELECT 17
         UNION ALL SELECT 18
         UNION ALL SELECT 19
         UNION ALL SELECT 20
         UNION ALL SELECT 21
         UNION ALL SELECT 22
         UNION ALL SELECT 23)as h
 LEFT JOIN 
    (SELECT hour(datetime) as HOUR,count(datetime) as 'COUNT'
FROM ANIMAL_OUTS
where hour(datetime)
group by HOUR
order by HOUR)as ANIMALS ON h.theHour = ANIMALS.HOUR


```







# IS NULL

말 그대로 널인 값을 잡아내는 함수입니다. 조건절에서 쓸 수 있겠죠!



## 1. 이름이 없는 동물의 아이디

```mysql
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NULL
```



## 2. 이름이 있는 동물의 아이디

> 1번도, 2번도 너무 문제 자체가 SQL 그 자체입니다. 해당 칼럼 값이 없는 로우를 골라줄 때는 `IS NULL` , 값이 있는 아이를 고를 때는 `IS NOT NULL`

 

```mysql
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NOT NULL
```





## 3. Null 처리하기

> Null을 처리하는 건 복수의 방법이 있습니다. NVL을 써도 괜찮고, CASE를 써도 괜찮은 것 같은데 이곳에서는 CASE를 써보겠습니다. name이 null인 경우 문자열 'no name'으로 바꿔주기

```mysql
SELECT ANIMAL_TYPE, 
CASE WHEN NAME IS NULL THEN 'No name' 
ELSE NAME END AS NAME,
SEX_UPON_INTAKE
FROM ANIMAL_INS;
```





# JOIN

JOIN,,,,JOIN,,,, 사실 join이 결국 sql 코딩테스트를 가르는 기준이라고 많이들 하시더라고요! 조인부터는 DBMS에 따라서 구현 방법이 달라지는 경우도 있으니까 (복잡하지 않으면 똑같음..) 역시 공식 홈페이지에 있는 도큐멘테이션을 잘 보는 것도 좋은 방법인 것 같습니다. 

https://dev.mysql.com/doc/refman/8.0/en/join.html

앗 모르겠다 마이스퀼튜토리얼을 봅니다! 

https://www.mysqltutorial.org/mysql-join/

이것도 조금 어렵다면...(티스토리나 구글에는 정말 좋은 자료들이 많습니다ㅎ)





## 1. 없어진 기록 찾기

>(기록 잃어버린 사람 짜르고 시작하자)
>
>... 이게 내가 ISNULL 파트에 있는 줄 알고 조인을 아예 생각을 안했는데요 이렇게 하나의 조건정도만 가지고 있는 정도는 조인보다는 NOT IN 을 써줍니다!!

```mysql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_OUTS
WHERE ANIMAL_ID NOT IN (SELECT ANIMAL_ID FROM ANIMAL_INS);
```



##2. 있었는데요 없었습니다. 

> 자 이제 JOIN을 시작해봅시다! (뻔뻔)
>
> ANIMAL_INS 와 ANIMAL_OUTS 의 테이블을 조인 해준 후에 조건에 맞는 것만 뽑아주는 전형적인 아주 간단한 형태의 INNER JOIN입니다. 



```mysql
SELECT ANIMAL_INS.ANIMAL_ID, ANIMAL_INS.NAME FROM ANIMAL_INS 
INNER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID =  ANIMAL_OUTS.ANIMAL_ID
WHERE ANIMAL_INS.DATETIME > ANIMAL_OUTS.DATETIME
ORDER BY ANIMAL_INS.DATETIME
```



## 3.오랜 기간 보호한 동물(1)

>.... 이것도 조인 안 썼네요.. 사실 LEFT JOIN 쓰면 된다고는 하지만 이렇게 하면 훨씬..쉬운데... 

```mysql
SELECT NAME, DATETIME FROM ANIMAL_INS
WHERE ANIMAL_ID NOT IN (SELECT ANIMAL_ID FROM ANIMAL_OUTS) 
ORDER BY DATETIME LIMIT 3;
```



## 4. 보호소에서 중성화된 동물

>만약 WHERE 구문을 각각의 테이블에 써주고 싶으면 subquery에 써주도록 합시다.

```mysql

SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME FROM 
(SELECT * FROM  ANIMAL_INS  WHERE SEX_UPON_INTAKE LIKE 'Intact%') AS I 
INNER JOIN 
(SELECT ANIMAL_ID , ANIMAL_TYPE 
 FROM ANIMAL_OUTS
 WHERE SEX_UPON_OUTCOME LIKE 'Spayed%' OR SEX_UPON_OUTCOME LIKE 'Neutered%') AS O
ON I.ANIMAL_ID = O.ANIMAL_ID

```





# String, Date

문자열과 날짜 형식에 대한 여러가지 조건이나 함수가 있는데, 다 기억하는 건 무리겠지만 아래의 문제에 나오는 것들 정도는 알아두면 도움이 많이 될 것 같습니다!



## 1. 루시와 엘라 찾기

> 이런 튜플 형식으로 IN 조건을 쓸 수 있답니다. 

```mysql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS
WHERE NAME IN ('Lucy','Ella','Pickle','Rogan','Sabrina','Mitty')
ORDER BY ANIMAL_ID
```





## 2. 이름에 el 들어가는 동물 찾기

> LIKE 
>
> `%` : 빈 문자열이나 복수의 길이의 문자열이 가능
>
> `_` : 하나의 문자열만 가능!

```mysql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS
WHERE NAME LIKE '%el%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```



## 3. 중성화 동물 파악하기

> 새로운 칼럼을 만들고 싶을 때, 값을 넣어줄 때는 CASE/WHEN/THEN/(ELSE)/END AS '새 칼럼명' 의 형식으로 쓸 수 있습니다!

```mysql
-- 코드를 입력하세요
SELECT ANIMAL_ID, NAME, 
CASE 
    WHEN SEX_UPON_INTAKE LIKE '%Neutered%' THEN 'O'
    WHEN SEX_UPON_INTAKE LIKE '%Spayed%' THEN 'O'
    ELSE 'X'
END AS 중성화
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID
```



## 4. 오랜 기간 보호한 동물(2)

> DATEDIFF, TIMEDIFF function을 알아둡니다!

```mysql 
-- 코드를 입력하세요
SELECT ANIMAL_OUTS.ANIMAL_ID, ANIMAL_OUTS.NAME FROM ANIMAL_OUTS
INNER JOIN ANIMAL_INS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
ORDER BY DATEDIFF(ANIMAL_OUTS.DATETIME, ANIMAL_INS.DATETIME) DESC
LIMIT 2
```





## 5. DATETIME에서 DATE로 형 변환

> 원하는 데이트포맷은 DATE_FORMAT을 이용해서 만들어 줍니다!!! 

```mysql 
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME,'%Y-%m-%d') AS 날짜 FROM ANIMAL_INS 
ORDER BY ANIMAL_ID
```

