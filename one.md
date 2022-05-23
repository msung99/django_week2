# 멋쟁이 사자처럼 - 백엔드 Django 세미나 (❁´◡`❁)


학습 목표 1. RDB(Relational DataBase)를 알 수 있다.

### CRUD

- Create, Read, Update, Delete
 
ex) 인스타그램
Create : 사용자들이 새로운 글을 생성(저장)가능
Read : 저장된 글을 읽어들일 수 있음
Update : 이미 작성된 글 ( DB 로 저장된 글 ) 을 수정가능
Delete : 삭제 가능

온라인 쇼핑몰
- 계정 생성 회원가입 => 쇼핑몰 db의 본인의 데이터를 저장
- 나의 개인정보 : db 에 저장된 정보를 읽어들이는 것
- 회원정보 수정 
- 회원 탈퇴 : 계정정보를 삭제 

> CRUD : 데이터베이스 안에 어떠한 대상을 저장하고, 저장한 대상을 가공하는 행동


- 데이터베이스와의 상호작용을 많이 하게된다!

---

### DB 최소지식

- DB, 장고는 독립적인 대상. 
장고가 DB 를 활용하기 위해 DB에 접근, 즉 연결을 하고 상호작용 해야함

- DB : 데이터를 저장하는 커다란 통
   
- DBMS: DB 안에 데이터를 관리해주고, 다른 SW가 이 DB 에 접근할 수 있도록 하게 해주는 SW. 즉 DB 관리 시스템
(Data Base Management System)
  - 종류 : MySQL, ORACLE, POST 머시기, Sqlite
    
- RDBMS (관계형 데이터베이스)
    - DB 안에 있는 데이터를 테이블로 관리하는 DB

![](https://velog.velcdn.com/images/msung99/post/732116e1-c8cd-4c39-8e5f-4551276c0954/image.png)

> DB 라는 통안에 데이터들을 넣을때 DBMS 를 활용해 데이터들을 관리한다.
   - 관계형 DB : 테이블로 관리하는 DBMS 시스템

> SQL : DBMS 를 활용해 DB 통에 접근 및 조작하게 해주는 언어.
즉, SQL 를 사용해서 DBMS 를 조작하는 것이다.

(몰론 관계형 DB 말고 테이블

- 장고는 sqlite3 라는 기본 DB 를 제공. => BUT, sqlite3 는 큰 프로젝트에 적합하지 않음. MySQl, RDS 등이 더 좋다.

---

#### Primary key(기본 키)

특정 데이터들을 특정지을 수 있는 키

특징
- 반드시 존재(Null 값x)
- 다른 데이터 값과 중복x
![](https://velog.velcdn.com/images/msung99/post/e7daadb6-1da3-4243-83ca-bc2b4a31b639/image.png)

   
각 테이블 간의 연관있는 primary key 들이 있다.
테이블마다 연관있는 데이터들 끼리 관계를 맺어줄 수 있다.

- Foreign  key(외래키) : 참조하려는 데이터 (참조자)
 
   - 다른 테이블의 특정 key 를 참조하는 key
   - 참조 당하는 테이블의 특정 key안의 데이터가 수정 또는 삭제될 경우 변경사항에 맞게 다양한 동작들을 정의할 수 있다.
   
   
ex) 수강과목의 데이터 구성은 개설과목의 데이터들로 구성해야 하므로 참조를 한다.

![](https://velog.velcdn.com/images/msung99/post/50325261-940a-4e8d-817d-4f43a9a9adb4/image.png)


---


### Django Models - ORM & Migration


#### ORM
- 파이썬 객체로 DB 의 테이블과 상호작용을 표현하는 장고의 기능 
- Object Realtional Mapping
- 객체를 이용한 테이블 매핑 (파이썬 객체를 이용해 SQL없이 DB를 조작)
- models.py 안의 클래스로 table 을 표현
  - 클래스로 테이블을 만들때 테이블의 각 필드의 데이터 타입을 명시해줄 것
  
  ![](https://velog.velcdn.com/images/msung99/post/a624d8b2-b03b-435f-a9a6-f5455908e9fe/image.png)

  

#### migration

- 클래스로 표현한 테이블을 migration 해줘야함
    - migration : DB에 변경사항을 반영해주는 것



$ python manage.py migrate  
  - DB를 처음 초기화하거나 변경사항을 반영해주는 것
  
  - migration 을 하지 않은채로 관리자 계정을 생성시 불가
     - 즉, 바로 python manage.py createsuperuser 명령어를 입력시 관리자 생성 안만들어짐
     - 방금전에 만들어진 장고 프로젝트의 model 들이 DB 에 반영되지 않았기 때문

![](https://velog.velcdn.com/images/msung99/post/1a3c42aa-f97f-4a9b-833b-43feb0053106/image.png)

![](https://velog.velcdn.com/images/msung99/post/a877c7af-e1b1-4bef-9636-0f4d2fc5babd/image.png)

$ python manage.py makemigrations

   - DB 변경사항이 내 프로젝트에서 발생시 DB 변경사항을 담은 파일을 토대로 그 변경사항을 DB에 반영
      - DB 변경사항을 담은 파일 : 명령어로 자동 생성할 수 있다
      

ex) 

DB 에 아래와 같은 student 테이블이 있을때

![](https://velog.velcdn.com/images/msung99/post/5024d5f7-bfd6-4223-9ce1-344c5ec3fc31/image.png)

아래처럼 수정하고 변경사항을 DB 에 새롭게 반영하려면

![](https://velog.velcdn.com/images/msung99/post/fe10faff-8825-426b-b65f-af1009c2445a/image.png)

- python manage.py migration 명령어로 변경사항을 담은 파일을 생성
- 그리고 python manage.py migrate 명령어로 DB 테이블에 직접 반영함

![](https://velog.velcdn.com/images/msung99/post/8d5c502d-10f1-4f88-ae30-a2fdccab1235/image.png)


> 정리
- 장고에서는 클래스 객체로 테이블을 정의한다.
---
- 테이블 정의시, 즉 클래스를 정의할 때 각각의 데이터가 어떤 자료형을 가지는지 필드 타입을 명시할 것
---
- 클래스를 정의하고 DB 한테 python manage.py migration 명령어로 변경사항을 알려줄 것
---
- 만일 처음으로 DB 를 초기화 할 경우는 무조건 python manage.py migrate 를 치면 된다.
---
- DB 안의 변경사항이 생기면 python manage.py migration 으로 변경사항을 담은 migration 파일을 만들고 python manage.py migrate 를 수행한다.

---





