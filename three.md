# 멋쟁이 사자처럼 - 백엔드 Django 세미나 (❁´◡`❁)

## 📌 3. Form을 이용하여 사용자 입력을 받을 수 있다.


### 💡 장고를 이용해 사용자 입력을 받는 방법


> Django 에서 사용자 입력을 받는 방법
#### 1. HTML Form 을 이용하기 
   - HTML Form 을 이용해서 model 객체를 추가하는 방법. 즉, 데이터베이스에 반영하는 방법

#### 2. Django Form 을 이용하기
- 장고가 기본적으로 제공하는 form 을 이용해서 model 객체를 추가하는 방법

#### 3. Django modelForm 을 이용하기
- 우리가 만든 model 을 기반으로 만들어진 장고가 기본적으로 제공하고 있는 modelForm 을 이용해서 model 객체를 추가하는 방법


---

## 🧙‍♂️ 3-1. HTML Form 을 이용한 사용자 입력받기에 대해 알아봅시다.

## 💻 html form

- HTML폼은 사용자와 웹사이트 또는 어플리케이션이 서로 상호 작용하는 것 중 중요한 기술, 폼은 사용자가 웹사이트에 데이터를 전송하는 것을 허용
- 장고는 model 객체, 즉 데이터베이스에 반영할 html form 을 아주 잘 지원해줌<br>
-> html 에다 글을 직접 작성하고, 수정하고, 추가하는 행위를 html form 으로 구성 가능함


---

## 💻 예시

- index.html 에서 "새 글 작성" 이라는 버튼을 누르면
새로운 html(new.html) 로 이동해서 그 html 에서 글을 작성한후, 
"글 생성" 버튼을 누르면 실제로 블로그 글이 만들어지는 프로그램을 만들어보자!

- (index.html)

![](https://velog.velcdn.com/images/msung99/post/919b4c4f-6e1a-485b-bbff-3d0144dc4d02/image.png)

- (new.html)

![](https://velog.velcdn.com/images/msung99/post/5f679afc-20e6-43c3-be19-b3ea94169074/image.png)


---

## 1️⃣ models.py 의 Blog 클래스

![](https://velog.velcdn.com/images/msung99/post/77ee5797-716a-4f3c-af7c-1b4b96773595/image.png)


---

## 2️⃣ urls.py 내용 

![](https://velog.velcdn.com/images/msung99/post/2ed4b994-f58d-4d5b-a609-b1db7d70fabd/image.png)

---

## 3️⃣ index.html 내용

- index.html 에서 new.html 로 이동할 수 있도록 url 을 만든다

![](https://velog.velcdn.com/images/msung99/post/c9e6b982-52f2-43d0-9de0-1e6f3df1dfae/image.png)

---


## 4️⃣ new.html 내용

> 핵심기능 : 사용자가 데이터들을 작성후, form 태그를 이용해서 POST 요청을 보냄


- 사용자가 글을 작성후에, form 태그에서 "글 생성하기" 누르면 views.py 파일의 create 함수가 실행
  - 즉, url 인 "(기본경로)/create" 로 POST 요청을 보낸다.
  
   - create 함수 내용 : model 객체(블로그 객체) 를 생성. 제목, 본문 등의 내용등의 사용자가 작성한 내용을 테이블(블로그 객체)에 저장하게 된다.

- 블로그 객체(model 객체)를 만드는 행위는 POST 요청이다.
    - 이 데이터를 처리해주라는 것이 POST 요청이므로!
    - method 에 'POST' 를 추가
   
- {% csrf_token %} : 보안문제를 위해 작성 
    - 해킹 공격 기법중 csrf 라는 것을 방지


![](https://velog.velcdn.com/images/msung99/post/4910adf6-2f3c-45d2-a4b3-028a38005034/image.png)

~~~
<form action = "{% url 'create' %}" method = "POST">
  {% csrf_token %}
  <div>
    <label for = "title">제목</label><br/>
    <input type = "text" name = "title">
  </div>
  <div>
    <label for = "body">본문</label><br/>
    <textarea name = "body" id = "body" cols = "30" rows = "10"></textarea>
  </div>
  <input type = "submit" value = "글 생성하기">
</form>
~~~



---

## 5️⃣ views.py 내용

> 핵심기능 : redirect


![](https://velog.velcdn.com/images/msung99/post/55201d8a-0cd3-4d25-beac-fd9e8d96fc89/image.png)


### from .models import Blog
    
 - create 함수는 Blog 객체 (model 객체) 를 만들어주는 함수 이므로, Blog 클래스를 import

### from django.utils import timezone


### new 함수
   - 블로그 글 작성 html form 을 보여주는 함수

### create 함수
   - 블로그 글을 저장해주는 함수
   - new.html 에서 POST 요청을 보낸 녀석에 의해서 create 함수가 실행된다.
   - redirect 
      - 해당 함수 내용이 다 실행된 후 특정 경로(html) 로  다시 돌아가도로록 하는 것
   
      - 렌더링을 하는 (html 을 보여주는) 함수가 아니므로, 이 함수의 실행문이 다 끝나면 특정 html, 즉 어떤 특정 경로로 다시 이동하도록 (redirect) 구현 가능 
   
   
   
 
~~~
def create(request):
  if(request.method == 'POST')  # request 의 method 가 POST 인 경우 (즉, POST 요청을 받은 경우)
    posts = Blog()   # table 객체, 즉 model 객체(Blog 객체)를 생성
    post.title = request.POST['title'] # request 안에있는 POST 요청 안에있는 데이터 중에서 title 에 해당하는 데이터를  Blog 객체의 title 에다 할당한다.
    post.body = request.POST['body']
    post.date = timezone.now()
    post.save()   # model객체.save() 를 통해 모델 객체를 데이터베이스에 저장할 수 있다.
  return redirect('home')  # 이 create 함수가 잘 끝났으면 home, 즉 index.html 로 다시 돌아가라는 뜻
~~~


---


##  🎁 요점정리(코드 작성 흐름 위주로)


- 1. views.py 파일의 함수 하나를 실행시켜서 GET 요청으로 index.html 으로 html을 화면에 띄운다.

- 2. index.html 내용의 "새 글 작성" 버튼을 누르면 GET 요청으로 new.html 을 화면에 띄운다.
    - 즉, 버튼을 누르면 index.html 에서 new.html 으로 이동해서 블로그 글 작성 html form 을 보여준다.

- 3. 사용자가 글을 작성후, html form 안에 있는 "글 생성하기" 버튼을 누르면 view.py 파일의 create 함수가 실행되면서 url 인 '(기본경로)/create' 으로 POST 요청을 보낸다.

- 4. POST 요청을 통해 Blog 객체(model 객체), 즉 테이블 객체가 생성되고, redirect 를 통해 다시 index.html 을 화면에 띄운다.


- 5. 생성된 Blog 객체는 관리자 url (주소/admin) 로 접속하면 확인가능하다.







