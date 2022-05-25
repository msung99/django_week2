# 멋쟁이 사자처럼 - 백엔드 Django 세미나 (❁´◡`❁)

## 📌 2. Model을 구성할 수 있다.

## - model 생성하기 

> 데이터베이스에 반영될 테이블의 형식을 클래스로 정의한다.


- 우리가 생성한 어플리케이션  "blogapp" 안의 models.py 안에 테이블, 즉 model, 또 다시말해서 클래스를 정의함.

- 클래스의 인자로 "models.Model" 을 넣어줘야 함.
이렇게 인자로 넣어주면 해당 클래스를 models.Model 를 상속받고, 이미 구현되어 있는 장고의 모델 기능을 사용할 수 있음.

- 모델(클래스)안의 각 요소(각각의 열) 이 어떤 데이터 타입으로 구성되어 있는지를 명시할 것
   - 명시방법 : 요소이름 =  models.필드메소드()
   - 필드 메소드 종류
        - models.CharField : 문자열을 저장하는 필드임을 명시
        - models.TextField : 대용량 문자열을 저장하는 필드임을 명시
        - models.DataTimeField() : 날짜 형식 필드(밑의 코드에서 auto_now_add 란 자동으로 현재 시간을 추가하겠다는 의미)
   
![](https://velog.velcdn.com/images/msung99/post/b9343b28-dfe8-4421-969b-5d643eb57e4b/image.png)

---

## - model 생성 후 변경사항 반영하기

- 변경사항이 담긴 파일을 생성.
$ python manage.py makemigrations

![](https://velog.velcdn.com/images/msung99/post/af7f51f6-757d-410c-8d79-3c37c5b7cbc6/image.png)

- 변경사항이 담긴 파일을 생성후, 이제 실제 DB 에다 변경사항을 반영.
$ python manage.py migrate

![](https://velog.velcdn.com/images/msung99/post/913b34ed-9882-458f-b10f-dd886177bb28/image.png)

- 이제부터 위 Blog 클래스와 같은 형식을 가진 데이터들을 생성 가능함.


---

## - model 객체 직접 확인하기

- admin 사이트에서 우리가 만든 model(클래스)의 객체를 직접 눈으로 확인할 수 있음.

- admin.py 파일에서 아래와 같이 Blog 모델를 등록하면, 관리자 페이지에서 Blog 객체를 확인 가능.
![](https://velog.velcdn.com/images/msung99/post/e2fc763a-b7a9-4635-8118-498a9a94848c/image.png)

- createsuperuser 명령어로 관리자 계정을 생성후, runserver 로 '기본주소'/admin 서버에 접속하면 Blogs 라는 model 을 확인할 수 있음(이때 객체 뒤에 자동으로 's' 가 붙음. 그래서 'Blogs' 임).


![](https://velog.velcdn.com/images/msung99/post/8aab3cfc-d13e-4bac-9827-70a161a16f87/image.png)

![](https://velog.velcdn.com/images/msung99/post/281750bc-d7dc-431e-aa36-5173ae62cf68/image.png)


![](https://velog.velcdn.com/images/msung99/post/2a3aaa60-e6a9-4bc0-962b-17384b2dbbb1/image.png)

- add 버튼을 눌러서 title, body 에다 우리가 직접 인풋값을 넣어서 생성한 테이블 객체를 DB에 반영할 수도 있음.

![](https://velog.velcdn.com/images/msung99/post/06b77d5d-5f48-42c5-be5d-537dc4403366/image.png)

- save 버튼을 누르면 해당 클래스(model) 에 대한 객체가 생성되었음을 볼 수 있음.


---
### 🎁요점정리
- 장고를 이용해 DB 에 반영할 수 있는 객체를 만들고, admin 사이트에서 객체 안에 있는 그 테이블의 형식에 맞게끔 데이터들을 추가하고 삭제할 수 있다.
