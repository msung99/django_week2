## 🧙‍♂️ 마지막으로, Django ModelForm 으로 model 객체를 생성하는 방법을 알아봅시다!


### urls.py


urls.py 에 아래처럼 url 을 등록한다.

![](https://velog.velcdn.com/images/msung99/post/bc38f5f2-5dd6-46de-863d-5c2d72aa517f/image.png)


---


### forms.py

- models.py 파일안에 있는 Blog 를 기반으로 form 을 만들 것이므로,
 from .models import Blog

~~~
# 모델 Blog 를 기반으로 자동으로 필드, 즉 입력할 수 있는 공간을 만들 수 있게 된다.

class BlogModelForm(forms.ModelForm): # forms.ModelForm 을 상속받음
  class Meta:
    model = Blog   # 어떤 모델을 기반으로 자동으로 입력값, 즉 form 을 생성할 것인지 명시
    # fields =  '__all__' # 어떤 필드를 입력받을지를 명시. //  __all__ => Blog 클래스 안에 있는 모든 열들을 다 입력받는다.
    fields = ['title', 'body']    # 특정 열의 필드 데이터만 입력받고 싶다면 리스트 활용
~~~

- 앞서 만든 BlogForm 은 어떤 모델을 기반으로 form 을 만들지 딱히 정해진 것이 없었다.
 그냥 우리가 입력받고자 하는 것들을 form에다 그냥 쓴다음에, views.py 에다 실시간으로 객체를 생성해서 views.py 의 formcreate 함수에서 실시간으로 객체를 생성해서, 객체 안에다 입력된 값을 하나하나 담아 줬었다.
 

- 지금 만든 modelForm 은 애초에 모델을 기반으로 만들어졌으므로 더 편하게 설계가 가능

---

### index.html

![](https://velog.velcdn.com/images/msung99/post/01f52e27-7ca9-4f10-9fae-58d93a66c9d1/image.png)


---

### views.py

views.py 에서 modelformcreate 함수를 정의한다.


![](https://velog.velcdn.com/images/msung99/post/52e911bf-05a6-45b2-92e5-facaea9602f3/image.png)

- GET 요청이 들어온 경우
    - 모델을 기반으로 forms.py 에서 정의한 입력공간, 즉 BlogModelForm 을 그대로 찍어서 보내준다.


- POST 요청이 들어온 경우
     - form_create.html 에서 입력공간을 통해 만들어진 어떤 입력공간이 POST 요청으로 request 가 들어왔다면,  form 의 유효성을 검사 ( 올바른 입력값인지 검사) 를 한후, form 에서 입력한 값을 save(), 즉 저장한다.
     
     









