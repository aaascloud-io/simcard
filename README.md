# iotpf
```
※各ツールバージョン：
maven version:3.6.3
jdk version:1.8
angular version:7.2
```
## 初めてのコンパイル時
①leshanフレームワークをコンパイルしてパッケージ化
```
cd leshan-2.0.x
mvn install 
※なぜかテスト失敗する人 -> mvn install -Dmaven.test.skip=true
```
②commons-v1.0→model-v1.0→services-v1.0→bs-server-v1.0→coap-server-v1.0→web-server-v1.0の順でンパイルしてパッケージ化
```
cd commons-v1.0
mvn install 
cd ..
cd model-v1.0
mvn install 
cd ..
cd services-v1.0
mvn install 
cd ..
cd bs-server-v1.0
mvn install 
cd ..
cd coap-server-v1.0
mvn install 
cd ..
cd web-server-v1.0
mvn install 
```
③管理員コンソールウェブページビルド
```
cd web-page
npm clean-install
ng build <- 不要？
```
## 動かすには

#### 起動前準備
⓪プロパティファイル<br>
起動パスと同じパスに格納して、名前はServer.propertiesにしてください。<br>
<br>
①mongoDBを起動し、接続情報をプロパティファイルに記入<br>
MONGO_CONNECTION_URL=xxxxxxxxxx<br>
<br>
②redisを起動し、接続情報をプロパティファイルに記入<br>
REDIS_CONNECTION_PASSWORD=xxx #パスワード使用してない場合、空白可<br>
REDIS_ADDRESS_HOST=xxxxxx<br>
REDIS_ADDRESS_PORT=xxxx<br>

### 起動

#### 1. PFデータ受信サーバーを起動
①使用ポート番号をプロパティファイルに記入<br>
COAP_UNSECURE_PORT=xxxx #DTLS通信ポート<br>
COAP_SECURE_PORT=xxxx #明文通信ポート<br>
<br>
②コマンドで起動
```
cd coap-server-v1.0/target
java -jar coap-server-1.0.0-SNAPSHOT-shaded.jar #mvn install後生成したjarファイル
```
#### 2. PFサーバー分配のブートストラップサーバーを起動
①使用ポート番号をプロパティファイルに記入<br>
BSSERVER_COAP_UNSECURE_PORT=xxxx #DTLS通信ポート<br>
BSSERVER_COAP_SECURE_PORT=xxxx #明文通信ポート<br>
<br>
②コマンドで起動
```
cd bs-server-v1.0/target
java -jar bs-server-1.0.0-SNAPSHOT-shaded.jar #mvn install後生成したjarファイル
```
#### 3. PFウェブAPIを起動
①使用ポート番号をプロパティファイルに記入<br>
WEB_PORT=xxxx #Http通信ポート<br>
<br>
②認証サーバーkeycloakを起動<br>
<br>
③認証サーバー接続情報をプロパティファイルに記入<br>
AUTH_REALM_NAME=xxxx<br>
AUTH_CLIENT_NAME=xxxx<br>
AUTH_SERVER_URL=xxxx<br>
AUTH_CORS_ALLOWED_HEADERS=*<br>
AUTH_CORS_ALLOWED_METHODS=*<br>
AUTH_CORS_MAX_AGE=86400<br>
AUTH_CORS_EXPOSED_HEADERS=*<br>
AUTH_ADMIN_CLIENT_NAME=xxxx<br>
AUTH_ADMIN_USER_USERNAME=xxxx<br>
AUTH_ADMIN_USER_PASSWORD=xxxx<br>
<br>
④コマンドで起動
```
cd web-server-v1.0/target
java -jar web-server-1.0.0-SNAPSHOT-shaded.jar #mvn install後生成したjarファイル
```

#### 4. 管理員コンソールウェブページ起動
angular-cli(開発環境)を使用する場合
```
cd web-page
ng serve
```
サーバーにdeployする場合
```
cd web-page
ng build --base-href /pfWeb/
distディレクトリ内のファイルをHTTPサーバーに配置する
```
