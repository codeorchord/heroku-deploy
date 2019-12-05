# heroku-deploy
django 프로젝트를 heroku에 배포하는 테스트

## Heroku 배포하기

### IaaS / PaaS

- IaaS (Infrastructure As A Service)

  대표적으로 `AWS EC2`가 있다. 배포시에 인력적인 코스트가 많이 드는(일일이 다 해 주거야 하기때문에) 단점이 있다.

- PaaS (Platform As A Service)

  `Heroku`, `AWS EB` 등이 있다. 배포 환경이 이미 갖추어져있다.

### 배포 전 준비사항

- 배포하려는 소스 올리기

- decouple 설치

  ```bash
  $ pip install python-decouple
  ```

- 중요 정보 숨기기(.env)

  ```python
  from decouple import config
  # SECURITY WARNING: keep the secret key used in production secret!
  SECRET_KEY = config('SECRET_KEY')
  
  # SECURITY WARNING: don't run with debug turned on in production!
  DEBUG = config('DEBUG')
  ```

- django-heroku 설치

  :point_right:https://github.com/heroku/django-heroku

  ```bash
  $ pip install django-heroku
  ```

  ***setting.py***

  ```python
  # 최하단에
  import django_heroku
  django_heroku.setting(locals())
  ```

### 배포를 위한 설정

- **Procfile** 작성

  > 앱을 시작할 때 필요한 커맨드를 기록하는 파일. 프로세스 실행과 관련된 다양한 설정값을 처리 해준다.

  ```
  web: gunicorn config.wsgi --log-file -
  ```

- **gunicorn** 설치 (uwsgi 대신 사용, heroku 추천)

  ㅇㄹㅇㄹ

  ```bash
  $ pip install gunicorn
  ```

- **runtime.txt** 작성

  ```
  python-3.7.5
  
  ```

- ***pip freeze***

  ```bash
  $ pip freeze > requirements.txt
  ```

### Heroku 설치, 로그인 및 설정

heroku dev center > search > heroku cli 검색 > windows 64-bit 다운로드 > 설치

```bash
$ heroku
...
  spaces          manage heroku private spaces
  status          status of the Heroku platform
  teams           manage teams
  update          update the Heroku CLI
  webhooks        list webhooks on an app
  
$ heroku login
heroku: Press any key to open up the browser to login or q to exit: 
Opening browser to https://cli-auth.heroku.com/auth/browser/16c4b062-6336-4f91-8398-4102eac3d6ca
Logging in... done
Logged in as codeorchord@gmail.com

$ heroku create mulcamp-deploy-jh
Creating ⬢ mulcamp-deploy-jh... done
https://mulcamp-deploy-jh.herokuapp.com/ | https://git.heroku.com/mulcamp-deploy-jh.git
(venv)

student@M1504 MINGW64 /c/git@jaehyun/heroku-deploy (master)
$ git remote -v
heroku  https://git.heroku.com/mulcamp-deploy-jh.git (fetch)
heroku  https://git.heroku.com/mulcamp-deploy-jh.git (push)
origin  https://github.com/codeorchord/heroku-deploy.git (fetch)
origin  https://github.com/codeorchord/heroku-deploy.git (push)


```

#### 환경변수 설정

- command 로 환경 변수 등록

  ```bash
  $ heroku config:set DEBUG=True
  Setting DEBUG and restarting ⬢ mulcamp-deploy-jh... done, v3
  DEBUG: True
  (venv) 
  ```

- Dash Board 를 이용한 환경변수 등록

  Heroku 접속 > 앱 이름 > setting > Reveal Config Vars 

  이곳에 SECRET_KEY를 등록한다.

#### 배포

```bash
$ git add .
$ git commit -m 'heroku deploy'
$ git push heroku master

```





