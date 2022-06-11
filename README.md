# spring-security-openam-oauth2


## Run OpenAM

```
docker run -h openam-01.example.com -p 8080:8080 --name openam-01 openidentityplatform/openam:14.6.5
```

1. ブラウザで `http://localhost:9000/private` にアクセスする
2. OpenAMのログインページにリダイレクトする
   `http://localhost:8080/openam/oauth2/authorize?response_type=code&client_id=test&scope=email&state=Sz61Zw0rWrT_GQZsRQqvg-HL_Z8tAzKuCXU2TTkSGX0%3D&redirect_uri=http://localhost:9000/login/oauth2/code/openam`
3. OpenAMにログインする
3. アプリケーションにリダイレクトする
   `http://localhost:9000/login/oauth2/code/openam?code=bf5079d8-6ee3-4ee3-8c50-085ce6ab0e01&scope=email&iss=http%3A%2F%2Flocalhost%3A8080%2Fopenam%2Foauth2&state=Sz61Zw0rWrT_GQZsRQqvg-HL_Z8tAzKuCXU2TTkSGX0%3D&client_id=test`
4. OpenAMのAPIを呼び出してアクセストークンを取得する
   ```
   ===========================request begin================================================
   URI         : http://localhost:8080/openam/oauth2/access_token
   Method      : POST
   Headers     : [Accept:"application/json;charset=UTF-8", Content-Type:"application/x-www-form-urlencoded;charset=UTF-8", Authorization:"Basic dGVzdDpwYXNzd29yZA==", Content-Length:"147"]
   Request body: grant_type=authorization_code&code=bf5079d8-6ee3-4ee3-8c50-085ce6ab0e01&redirect_uri=http%3A%2F%2Flocalhost%3A9000%2Flogin%2Foauth2%2Fcode%2Fopenam
   ==========================request end================================================
   ============================response begin==========================================
   Status code  : 200 OK
   Status text  :
   Headers      : [X-Frame-Options:"SAMEORIGIN", Cache-Control:"no-store", Date:"Sat, 11 Jun 2022 07:16:32 GMT", Accept-Ranges:"bytes", Server:"Restlet-Framework/2.4.0", Vary:"Accept-Charset, Accept-Encoding, Accept-Language, Accept", Pragma:"no-cache", Content-Type:"application/json", Transfer-Encoding:"chunked", Keep-Alive:"timeout=20", Connection:"keep-alive"]
   Response body: {"access_token":"7b653046-cc64-48bf-aab1-151de16936a4","scope":"email","token_type":"Bearer","expires_in":3599}
   =======================response end=================================================
   ```
5. OpenAMのAPIを呼び出してユーザー情報を取得する
   ```
   ===========================request begin================================================
   URI         : http://localhost:8080/openam/oauth2/userinfo
   Method      : GET
   Headers     : [Accept:"application/json", Authorization:"Bearer 7b653046-cc64-48bf-aab1-151de16936a4", Content-Length:"0"]
   Request body:
   ==========================request end================================================
   ============================response begin==========================================
   Status code  : 200 OK
   Status text  :
   Headers      : [X-Frame-Options:"SAMEORIGIN", Date:"Sat, 11 Jun 2022 07:16:32 GMT", Accept-Ranges:"bytes", Server:"Restlet-Framework/2.4.0", Vary:"Accept-Charset, Accept-Encoding, Accept-Language, Accept", Content-Type:"application/json;charset=UTF-8", Transfer-Encoding:"chunked", Keep-Alive:"timeout=20", Connection:"keep-alive"]
   Response body: {"sub":"Taro","email":"taro@example.com"}
   =======================response end=================================================
   ```
6. もともとアクセスしようとしていた `http://localhost:9000/private` にリダイレクトする