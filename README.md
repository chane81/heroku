# heroku

## 기타 명령어

- heroku 버전
  - heroku --version

- 3rd-party buildpacks
  > heroku plugins:install heroku-repo

***
## 배포

- 유념
  > 배포시에 `.next` 가 포함되지 않았다라고 경고가 나올 수 있는데 아래와 같이 package.json 의 script 부분을 수정해서 빌드하고 배포하면 된다.
  ```json
  	"dev": "next",
		"build": "next build",
		"start": "next start -p $PORT",
		"heroku-postbuild": "next build"
  ```



  > push 하기 전에 먼저 빌드부터 하고 서버에 배포해야한다.

- login
  > heroku login -i

- 로그 실시간 보기
  > heroku logs -t

- git 릴리즈 보기
  > heroku releases

- 해당버전으로 롤백
  > heroku rollback v12

- 배포
  ```
  heroku login
  heroku git:remote -a thawing-ravine-21547
  git add .
  git commit -m "수정내역"
  git push -f heroku master
  ```

***

## 인증서와 SSL

- 참조 url
  - 인증서 발급과정
    - https://devcenter.heroku.com/articles/ssl-certificate-self
  - 인증서 적용 과정
    - https://devcenter.heroku.com/articles/ssl

- 개인키 생성
  - openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
  - openssl rsa -passin pass:x -in server.pass.key -out server.key
  - del server.pass.key

- 개인키로 요청서 생성
  - openssl req -new -key server.key -out server.csr -config ../openssl.cnf

- 인증서 발급
  - openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt

## 인증서 add
- 인증서들 보기
  - heroku certs --app socket-server-node

- 해당 git remote 이동
  - heroku git:remote -a thawing-inlet-61413

- 인증서 add
  - heroku certs:add server.crt server.key --type endpoint