
# 멋쟁이 사자처럼 - 백엔드 Django 세미나 (❁´◡`❁)

## 📌 3. Form을 이용하여 사용자 입력을 받을 수 있다.


## 🧙‍♂️ 3-2. Django form 을 통해 model 객체 생성하기

html form 외에도 Django form 을 활용해 사용자로 부터 입력받은 데이터를 데이터베이스에 저장할 수 있음

---

## 1️⃣ index.html 

"django form을 이용한 새 글 작성"이라는 버튼을 누르면
"(기본주소)/new" 라는 url 로 넘어가도록 하자.

![](https://velog.velcdn.com/images/msung99/post/40829167-47d8-42eb-b7cb-bc69627d8deb/image.png)

---

## 2️⃣ urls.py

- "formcreate/" 라는 url 을 등록해준다. 

- name, 즉 namespace 를 'formcreate' 로 지정해줬으므로 이동해야 하는 url 도 index.html 에서 보듯이 formcreate 로 지정해줌

![](https://velog.velcdn.com/images/msung99/post/b8b56fac-2cf6-4288-86e3-47e6087a3299/image.png)

---

## 3️⃣ 새로운 파일 forms.py
 
- blogapp 어플에다가 장고 form 을 정의할 수 있는 새로운 파일 forms.py 을 생성해봤다.

- 장고 form 을 이용해 자동으로 form 을 만드는 이 기능은 클래스로 정의가 가능하다. 

~~~
from django import forms   # 장고로부터 forms 를 가져옴
from .models import Blog  # Blog 모델을 기반으로 form 을 만들 것이므로 import 해줌

# django form 을 이용해 자동으로 form 을 만드는 기능을 클래스로 정의

class BlogForm(forms.Form):  # django 로부터 import 해온 form 을 상속받아서 form 을 만든다.
  # 내가 입력받고자 하는 값들을 아래와 같이 form의 Field 형식에 맞게 적어줌
  title = forms.CharField() # form 으로부터 Char 터입으로 title 값을 입력받음
  body = forms.CharField(widget = forms.Textarea) # form 으로부터 긴 문자열 타입으로 body 값을 입력받음
  
~~~

---

## 4️⃣ views.py

> 핵심1 : 장고는 사용자로부터 입력값을 받을 수 있는 어떤 url 이 있다면, 한 url 에서 GET 요청과 POST 요청을 둘 다 처리할 수 있게끔 만들 수 있다.

> 핵심2: render의 3번째 인자 => views.py 내의 데이터를 html 에 넘겨줄 수 있다. 단, 딕셔너리 자료형으로 넘겨줘야 한다.

- forms.py 파일을 기반으로 form 을 만든다.
    - 이를 위해 forms.py 파일에서 정의한 BlogForm 을 import 한다.
    

- "(기본주소)/formcreate" URL 을 입력받으면 views.py 파일의 
formcreate 함수가 실행되는데, django forms 를 이용해서 입력값을 받는 이 함수는 GET, POST 요청 모두 동시에 처리가 가능하다.  
 
 
 
 - GET 요청 = 입력값을 받을 수 있는 html 을 갖다 줘야함
   
   -  ex) index.html 에서 'django form을 이용한 새 글 작성' 버튼을 누르면 입력을 받을 수 있는 html, 즉 form 으로 이루어진 html 을 갖다줘 라는 요청이 formcreate 함수로 전달된다.
   
 - POST 요청 = 입력한 내용을 데이터베이스에 저장하는 기능. 즉, form 에서 입력한 내용을 처리
 
~~~
def formcreate(request):
  if request.method == 'POST'  # 이 함수를 요청한 타입이(=메소드가) POST 인 경우
    # 입력 내용을 DB 에 저장
      form = BlogForm(request.POST)  # form 을 만들어서 request의 POST 를 담고있는 BlogForm 을 만듦
       if form.is_valid(): # 지정해 놓은 필드 데이터 타입과 일치하는 데이터를 입력했는지 유효성을 검사 
         post = Blog()
         post.title = form.cleaned_data['title'] # form 으로 입력된 값 중에서 검증을 거친 깨끗한 데이터중에서 title 에 해당하는 데이터를 post 객체의 title 에 할당 
         post.body = form.cleaned_data['body']
         post.save()
         return redirect('home')  # POST 요청이 끝나면 딱히 html 을 보여줄 것은 없으니까 redirect 하고 끝
      
  else:      # 이 함수를 요청한 타입이(=메소드가) GET 인 경우
    # 입력을 받을 수 있는 html 을 갖다주기
    form = BlogForm()  # 만들어준 form 으로 생성된 객체인 form 을 렌더링하려는 파일인 form_create.html 에다 views.py 파일 내부에 있는 정보를 찍어서 보내준다. 
    return render(request, 'form_create.html', {'form':form} )
~~~

---

## 5️⃣ form_create.html 

> 핵심 : views.py 에서 render() 의 세번쨰 인자로 넘겨준 데이터는 html 내에서 { { } } 에 감싸진 채로 table 형태로 표현될 수 있다.



- views.py 파일의 formcreate 함수에서 렌러딩을 할때 form_create.html 파일에 views.py 파일 안의 내용을 찍어보낼 수 있다.

   - 2번쨰 인자로 준 form_create.html 파일에다 views.py 파일 안에 있는 데이터를 3번쨰 인자인 딕셔너리 형태로써 찍어보내줄 수 있다.
 
   - 딕셔너리 형태로 내보낸 데이터는 중괄호 2개로 감싼 형태로 html 에서 table 형식으로 찍어줄 수 있다.
   
> django form 을 이용해서 만들어진 form은 html 에서 form.as_table 으로만 써주면 자동으로 < table > 태그가 감싸진 것 처럼 표현될 수 있다.

>- form.as_table : table 태그가 감싸진 것처럼 보여짐
- form.as_p : p 태그가 감싸진 것처럼 보여짐
- form.as_ul : ul 태그가 감싸진 것처럼 보여짐

~~~
<h1>django form을 이용한 새 글 작성 페이지</h1>
<form action = "" method = "POST">
   {% csrf_token %}
   <table>
   {{form.as_table}} // views.py 파일의 내용을 보여줄때 table 형태로 보여줌
   </table>
   <input type = "submit" value = "새 글 생성하기">
</form>
~~~

아래처럼, '(기본주소)/formcreate' 에 GET 요청이 보내지면 views.py 의 BlogForm 을 상속한 form 이 자동으로 html form 을 만들어준다.

즉, 우리가 입력할 수 있는 form 을 우리 브라우저에 갖다줘! 라고 요청하는 것

### 👉 html form 으로 form.as_ul 를 적용했을 때
![](https://velog.velcdn.com/images/msung99/post/3f179fa4-4791-49cb-98d0-2f933956fd34/image.png)


### 👉 html form 으로 form.as_tale 을 적용했을 때

![](https://velog.velcdn.com/images/msung99/post/20a68cce-52e9-4247-8a89-0b220b3f4a58/image.png)


---

## 🎁 요점정리(실행화면 위주로)

![](https://velog.velcdn.com/images/msung99/post/bdefc01c-f5f1-40e7-a705-c5af71f462ef/image.png)

- 여기서 'django form을 이용한 새 글 작성' 을 누르면 '(기본주소)/formcreate' URL 한테 GET 요청이 보내지고,

![](https://velog.velcdn.com/images/msung99/post/076e4930-ca2a-4878-8658-d43faff64535/image.png)

- 위와 같이 글을 입력후 '새 글 생성하기' 를 누르면 
'(기본주소)/formcreate' URL 에 대해서 POST 요청이 간다.

- 그러면 우리가 입력한 내용을 바탕으로 우리가 forms.py 에서 정의한 BlogForm 내용에 알맞게 form 객체에 담길 것이다.

- 그리고 우리가 입력한 데이터에 대한 유효성을 검증후 models.py 안에 있는 Blog 형식에 알맞게 저장한다. 


