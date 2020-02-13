---
title: 이미지 업로드 / 썸네일 보여주기
date: 2020-02-13
categories:
    - Django
---

# 업로드한 이미지 보기

* 이미지 필드를 등록하고 추가하면 어드민 페이지에서 추가는 되지만 이미지를 보려고 하면 사진이 보이지 않는다.  
장고 어드민에서 이미지 보여주기  

1. 장고 세팅파일에서 MEDIA_URL ="media/"을 정해주면 이미지url 경로가 MEDIA_URL로 변경된다  
  근데 "media/" 상대경로 식으로 적어주면 localhost:8000/~~/~~/media 이런식으로 앞에 필요하지 않은 url로 된다.  
  이를 "/media/" 절대경로로 적어주면 localhost:8000/media/ 경로로 이미지 url이 변경된다  

2. MEDIA_ROOT = 'os.path.join(BASEDIR, "저장될 위치") 이렇게 설정하면 사진이 업로드 되면 해당 위치에 사진이 저장된다.  
 

3. ImageField(upload_to='TEST') 이미지 필드에서 upload_to 를 설정하면 사진이 업로드되면 MEDIA_ROOT/TEST 로 저장된다.  
   단, 개발환경에서만 이렇게 사용한다. 서버에 이미지를 저장하는 방법은 사용자가 많아지고 파일이 많아질 때 문제가 있다.  
   배포할때는 AWS S3같은 별도의 서버에 파일을 저장하는 것이 좋다.  


4. 마지막으로 urls 파일에서 settings와 static 을 import해준 뒤  
   urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) 를 추가해주면 잘 작동한다.


5. settings에서 세팅해준 변수들을 urls파일에서 사용하기 위해 import해야하는데  
   from . import settings 와 같이 import 해주는 것은 좋은 방법이 아니다.  
   왜냐하면 파일 위치가 달라진다면 해당 코드는 실행이 되지 않기 때문이다  
   따라서 from django.conf import settings 로 장고가 settings파일을 찾을 수 있도록 작성해준다.  
 

   DEBUG=True일때는 개발환경이고 배포할때는 False로 설정하는데 개발환경일때만 이렇게 사용 할 것이기 때문에
   아래와 같이 작성하면 더 좋은 코드가 된다.
   
```python
from django.conf import settings
from django.conf.urls.static import static
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## 어드민 페이지에서 image보여주기

아래와 같이 작성하면 어드민 페이지에서 썸네일 사진을 볼 수 있다.  
```python
from django.utils.html import mark_safe
@admin.register(models.Photo)
class PhotoAdmin(admin.ModelAdmin):
    list_display = ('__str__', 'get_thumnail')

    def get_thumnail(self, obj):
        return mark_safe(f'<img width="100px" src={obj.file.url} />')  
```  

get_thumnail 함수를 분석해보면 self는 PhotoAdmin 클래스이고 obj는 모델의 Photo클래스이다.  
장고는 해커가 script코드를 작성하여 공격하는 것을 막기위해 코드내에 script 작성을 막고있다.  
mark_safe는 장고에게 이 부분은 괜찮다고 알려주는 것이다.  
obj.file.url이란 코드를 사용하고 있는데 그 전에 파이썬에 vars, dir이라는 내장 함수가 있다  
python에서 vars([객체])는 모듈, 클래스, 인스턴스, __dict__속성이 있는 다른 객체의 __dict__속성을 리턴해준다.  
즉 해당 객체의 속성과 관련된 것들을 리턴해준다.  
dir([객체]) 객체가 형 또는 클래스 객체면 속성과 base attribute 이름들을 리턴해 준다.  
dir(obj.file)를 찍어보면 여러 값들이 있는데 그중 url은 file의 url을 변경해준다. 그 외에 save, size, width 등의 옵션이 있다.  
