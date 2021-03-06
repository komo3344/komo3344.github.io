---
title: "Django Heroku로 배포하기"
date: 2019-11-06
---
[튜토리얼 참조]하여 작성


# 배포하기 위해 필요한 것들

- runtime.txt: 프로그래밍 언어와 사용 버전.
- requirements.txt: Django를 포함한 파이썬 관련 라이브러리 의존성.
- Procfile: 웹 어플리케이션을 구동하기 위해 실행되어야 하는 프로세스의 목록. 장고의 예를 들자면, Gunicorn 웹 어플리케이션 서버( .wsgi 스크립트와 함께) 가 될것이다.  
- wsgi.py: Heroku 환경에서 Django 어플리케이션을 호출 하기 위한 WSGI 설정.

자세한 내용은 [Heroku 문서] 참조
  
  # 배포시작
**settings.py**


```
# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = 'cg#p$g+j9tax!#a3cup@1$8obt2_+&k3q+pmu)5%asj6yjpkag'
import os
**SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'cg#p$g+j9tax!#a3cup@1$8obt2_+&k3q+pmu)5%asj6yjpkag')**

# SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
**DEBUG = bool( os.environ.get('DJANGO_DEBUG', True) )**
```
  # Heroku에 맞춰 App 수정하기
  
  (manage.py 있는 위치에 파일 추가)
  
  **Procfile 작성**
  
  (파일명 대소문자 확인!!)
  어플리케이션의 프로세스 타입과 엔트리 포인트를 선언하기 위해  GiHub 저장소의 root 폴더에  Procfile 파일을 (확장자 없이 ) 생성
  
  아래 코드 기입
  
  
  ```
  web: gunicorn 프로젝트명.wsgi --log-file -
  ```
  
  
  **Gunicorn 설치하기**
  
  Django와 함께 사용되는 용도로 Heroku에서 추천되는 HTTP server 이다 
  
  ```
  $ pip install gunicorn
  ```
  

**dj-database-url (환경 변수를 통한 Django 데이터베이스 설정 ) 설치하기**


Heroku에서 원격 서버에 설치하기 위한  요구조건의 일부가 되었으니, dj-database-url 를 로컬에 설치한다
```
$ pip3 install dj-database-url
```

**settings.py 수정하기**


settings.py 를 열고 아래 설정코드를 파일의 맨 밑에 복사해 넣는다


```
# Heroku: Update database configuration from $DATABASE_URL.
import dj_database_url
db_from_env = dj_database_url.config(conn_max_age=500)
DATABASES['default'].update(db_from_env)
```

- `DATABASE_URL` 환경변수가 개발용 컴퓨터에는 없을것이므로 개발단계에서는 계속 SQLite를 사용한다.
- `conn_max_age=500` 설정을 하면 연결상태가 지속될 수 있는데, 매번 요청 주기에 연결을 새로 하는것보다 이렇게 하는 것이 훨신 효율적이다. 하지만 이것은 옵션이며 불 필요하면 제거해도 된다.


**psycopg2 (Python Postgres 데이터베이스 지원용) 설치하기**


Django에서 Postgres 데이터베이스로 작업하기 위해서는 psycopg2 가 필요하므로 Heroku에 원격 서버를 생성하기 위해서는( 아래 요구조건 섹션에 논의된 바와 같이) requirements.txt 파일에 이 항목을 추가할 필요가 있다.


```
pip3 install psycopg2
```
[다른 플렛폼 설치방법]


하지만, Heroku에 사이트를 적용하기 위한 ( requirements.txt (아래에 나옴)에서) 요구조건 으로서만 맞추려고 한다면, 굳이 이렇게 PostGreSQL을 로컬 컴퓨터에 설치할 필요까지는 없다.


**운영환경에서 정적 파일(static file) 지원하기**


- `STATIC_URL`: 이것은 베이스 URL 위치인데 이곳에서 정적 파일들이 지원된다. 예를 들면 CDN과 같은곳이다. 베이스 템플릿에서 접근하는 정적 템플릿 변수에 사용된다. 
- `STATIC_ROOT`: 이것은 Django의 "collectstatic" 도구로 템플릿에서 참조하는 모든 정적 파일을 모집하는 디렉토리로 가는 절대 경로이다. 일단 수집되면, 이것들은 파일이 어떤곳에서 호스팅되든지 단체로 업로드 될 수 있다.
- `STATICFILES_DIRS`: 이것은 Django의 colletstatic 도구가 정적 파일을 탐색할 추가적인 디렉토리를 나열한다.

**settings.py 수정하기**


```
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.0/howto/static-files/

# The absolute path to the directory where collectstatic will collect static files for deployment.
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

# The URL to use when referring to static files (where they will be served from)
STATIC_URL = '/static/'
```

**Whitenoise 적용하기**


운영환경에서 정적 파일을 관리하기 위한 수많은 방법이 있다 (바로 앞 섹션에서 관련된 Django 설정을 봤다). 
Heroku는 WhiteNoise 프로젝트를 이용하여 운영환경의 Gunicorn상에서 직접 정적 자원을 관리하는 것을 추천한다.

아래 명령어로 로컬에 설치
```
pip3 install whitenoise
```

**settings.py 수정하기**

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    **'whitenoise.middleware.WhiteNoiseMiddleware',**
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

선택 사항으로, 파일이 서빙될 때 정적 파일의 크기를 줄일 수 있다. (이 방식이 좀더 효율적이다)

```
# Simplified static file serving.
# https://warehouse.python.org/project/whitenoise/
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```


**파이썬 관련 라이브러리 (Requirements) 설치하기**


웹 어플리케이션의 Python 관련 라이브러리들은 저장소의 루트에 위치한 requirements.txt 라는 파일에 저장되어야 한다. 그러면 Heroku는 환경을 재구성할 때 이 패키지들을 자동적으로 설치할 것이다. 커맨드 라인에서 pip 명령을 이용해 이 파일을 생성할 수 있다
```
pip freeze > requirements.txt
```

requirements.txt 목록에 psycopg2 줄이 있다는 것을 주의해야 한다! 
로컬환경에서 이 라이브러리를 설치한 적이 없더라도 이 줄은 requirements.txt 파일에 추가해야 한다.

**Runtime 파일 추가하기**


`runtime.txt` 파일이 존재한다면 Heroku에게 웹사이트에서 사용할 프로그래밍 언어를 알려준다. 
아래 문구를 추가하여 저장소의 루트에 runtime.txt 파일을 생성한다
```
python-3.7.4
```
Heroku가 지원하는 [Python 실행버전]


# Github에 변경사항을 저장하고 테스트 다시하기


다음으로 모든 변경사항을 Github에 저장하자. 아래 명령을 ( 저장소 범위내 위치에서) 터미널에 입력

```
git add -A
git commit -m "Added files and changes required for deployment to heroku"
git push origin master
```

계속 진행하기전에, 로컬에서 사이트를 다시 테스트 해서 위의 변경사항에 의해 영향받은부분이 없는지 확인한다. 
지금까지 해온 것 처럼 개발용 웹 서버를 실행하고 브라우저 상에서 여전히 기대한 대로 동작하는지 체크
```
python manage.py runserver
```

# 웹 사이트 생성하고 업로드


앱을 생성하기 위해 "create" 명령을 저장소의 루트 디렉토리에서 실행한다. 
이명령은 로컬 컴퓨터의 git 환경에 heroku라는 이름의 git의 원격 저장소(remote) ("원격 저장소의 지정자(pointer)")를 생성한다.

```
heroku create
```
원한다면 원격 저장소(remote)에 이름을 붙일 수 있는데 . "create" 다음에 값을 추가하면 된다. 
아무것도 붙이지 않으면 랜덤으로 생성된 이름을 가진다. 이 이름은 기본 URL로 사용된다.
  
그다음에 아래와 같은 명령으로 앱을 Heroku 저장소에 등록(push)할  수 있다. 
이 명령은 앱을 업로드하고, 다이노안에 앱을 포장(pakage)하며, colletstatic을 실행하고 사이트가 시작 되도록 한다.

```
git push heroku master
```

운이 좋다면, 앱은 이제 사이트상에서 "실행(running)" 상태에 있게되겠지만, 
어플리케이션을 위한 데이터베이스 테이블을 구성하지 않았기 때문에 제대로 동작하지 않을 것이다. 
이 작업을 위해 heroku run 명령을 사용해 migrate 작업을 수행할 "one off 다이노" 를 시작시켜야 한다. 
아래 명령을 터미널에 입력하라

```
heroku run python manage.py migrate
heroku run python manage.py createsuperuser
```
  
  [heroku 문서]: https://devcenter.heroku.com/articles/getting-started-with-python
  [튜토리얼 참조]: https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Deployment
  [다른 플렛폼 설치방법]: http://initd.org/psycopg/docs/install.html
  [Python 실행버전]: https://devcenter.heroku.com/articles/python-support#supported-python-runtimes
