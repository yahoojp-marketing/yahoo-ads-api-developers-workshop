# 認可ハンズオン

認可ハンズオンで使用する URL 一覧です。

[Developer Center](https://ads-developers.yahoo.co.jp/developercenter/ja/startup-guide/) の以下のページにさらに詳細な説明があります。

- [アプリケーションの登録](https://ads-developers.yahoo.co.jp/developercenter/ja/startup-guide/app-registration.html)
- [APIコールを実施する](https://ads-developers.yahoo.co.jp/developercenter/ja/startup-guide/api-call.html)

## 1. アプリケーション登録

[登録アプリケーション画面](https://connect-business.yahoo.co.jp/client/list)

## 2. 認可

### 認可リクエスト

以下の[認可リクエスト URL](https://biz-oauth.yahoo.co.jp/oauth/v1/authorize?response_type=code&client_id=YOUR_CLIENT_ID&redirect_uri=oob&scope=yahooads) にブラウザでアクセスしてください。

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/authorize
?response_type=code
&client_id=YOUR_CLIENT_ID
&redirect_uri=oob
&scope=yahooads
```

### トークン発行リクエスト

以下の[トークン発行リクエスト URL](https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=authorization_code&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&redirect_uri=oob&code=AUTH_CODE) に `curl` や [Postman](https://www.getpostman.com/) などでアクセスしてください。
ブラウザでもアクセスできますが、クライアントシークレット等がブラウザの履歴に残りますので注意してください。

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/token
?grant_type=authorization_code
&client_id=YOUR_CLIENT_ID
&client_secret=YOUR_CLIENT_SECRET
&redirect_uri=oob
&code=AUTH_CODE
```

`curl` の場合は以下のコマンドになります。

```sh
curl -i "https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=authorization_code&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&redirect_uri=oob&code=AUTH_CODE"
```

### アクセストークン再発行

以下の[アクセストークン再発行 URL](https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=refresh_token&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&refresh_token=REFRESH_TOKEN) に `curl` や [Postman](https://www.getpostman.com/) などでアクセスしてください。
ブラウザでもアクセスできますが、クライアントシークレット等がブラウザの履歴に残りますので注意してください。

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/token
?grant_type=refresh_token
&client_id=YOUR_CLIENT_ID
&client_secret=YOUR_CLIENT_SECRET
&refresh_token=REFRESH_TOKEN
```

`curl` の場合は以下のコマンドになります。

```sh
curl -i "https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=refresh_token&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&refresh_token=REFRESH_TOKEN"
```

# APIリファレンス上でのAPIコール試用ハンズオン

## 1. APIリファレンスのリダイレクトURI追加

APIリファレンスのリダイレクトURIは、登録アプリケーション画面から追加します。

[登録アプリケーション画面](https://connect-business.yahoo.co.jp/client/list)

ハンズオンでは [Yahoo!広告 ディスプレイ広告 API リファレンス](https://yahoojp-marketing.github.io/ads-display-api-documents/) を使用します。
リダイレクト URI: `https://yahoojp-marketing.github.io/ads-display-api-documents/oauth2-redirect.html`

なお、[Yahoo!広告 検索広告 API リファレンス](https://yahoojp-marketing.github.io/ads-search-api-documents/)と[Yahoo!広告 ディスプレイ広告 API リファレンス](https://yahoojp-marketing.github.io/ads-display-api-documents/)で**登録するリダイレクトURIが異なります**ので注意してください。

## 2. 認可

[Yahoo!広告 ディスプレイ広告 API リファレンス](https://yahoojp-marketing.github.io/ads-display-api-documents/) の `Authorize` ボタンから認可します。

## 3. APIリファレンス上でAPIコール

[Yahoo!広告 ディスプレイ広告 API リファレンス](https://yahoojp-marketing.github.io/ads-display-api-documents/) の `Try it out` ボタンからAPIコールします。
ハンズオンでは `AccountService/get` を使用してみます。

リクエストボディ:

```json
{
  "includeTestAccount": "ALL"
}
```

# ヤフー広告API ハンズオン
`curl` や [Postman](https://www.getpostman.com/) などでアクセスしてください。  
Postmanを使う場合はこのリポジトリにある`https---ads-display.yahooapis.jp.postman_collection.json`をImportしてVariablesを書き換えて使用してください。  

##  アカウントの取得
後述のVideoService/upload(更新系操作)をするために`"authType": "UPDATABLE"`な広告アカウントを取得します。
### AccountService/get
```Shell
ACCESS_TOKEN=XXX # アクセストークンに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/AccountService/get -d \
'
{
  "authType": "UPDATABLE",
  "includeTestAccount": "ONLY_TEST"
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/AccountService/get -d ^
"^
{^
  \"authType\": \"UPDATABLE\",^
  \"includeTestAccount\": \"ONLY_TEST\"^
}^
"
```
## レポートのダウンロード
レポートをダウンロードするためには、事前にレポートを作成(ReportDefinitionService/add)してから、  
ダウンロードします。  

※
Yahoo!プロモーション広告API(旧API)ではレポートのダウンロードは、ReportDefinitionService/getのレスポンスのURLから行っていましたが  
https://github.com/yahoojp-marketing/ydn-api-documents/blob/master/docs/ja/api_reference/services/ReportDefinitionService.md#get  
Yahoo!広告 APIではReportDefinitionService/addのレスポンスのreportJobIdを  
ReportDefinitionService/downloadのリクエストに指定してダウンロードします。  

### ReportDefinitionService/add
```Shell
ACCESS_TOKEN=XXX # アクセストークンに置き換えてください
ACCOUNT_ID=XXX   # アカウントIDに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/add -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  "operand": [
    {
      "dateRangeType": "LAST_7_DAYS",
      "fields": [
        "ACCOUNT_ID",
        "ACCOUNT_NAME"
      ]
    }
  ]
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/add -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%,^
  \"operand\": [^
    {^
      \"downloadEncode\": \"SJIS\",^
      \"dateRangeType\": \"LAST_7_DAYS\",^
      \"fields\": [^
        \"ACCOUNT_ID\",^
        \"ACCOUNT_NAME\"^
      ]^
    }^
  ]^
}^
"
```
### ReportDefinitionService/download
```Shell
ACCESS_TOKEN=XXX  # アクセストークンに置き換えてください
ACCOUNT_ID=XXX    # アカウントIDに置き換えてください
REPORT_JOB_ID=XXX # ReportDefinitionService/addのレスポンスのreportJobIdに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/download -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  "reportJobId": '${REPORT_JOB_ID}'
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
set REPORT_JOB_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/download -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%,^
  \"reportJobId\": %REPORT_JOB_ID%^
}^
"
```
## ビデオのアップロード
ビデオのアップロードはリクエストパラメータはQueryString形式でURLに付与し、  
ビデオファイル自体はmultipart/form-dataで送信します。  
アップロードするビデオはこのリポジトリにある`sample.mp4`をご利用ください  

※
Yahoo!プロモーション広告API(旧API)ではビデオのアップロードは、VideoService/getUploadUrlのレスポンスのURLから行っていましたが  
https://github.com/yahoojp-marketing/ydn-api-documents/blob/master/docs/ja/api_reference/services/VideoService.md#getuploadurl  
Yahoo!広告 APIではVideoService/uploadで直接行えます。
### VideoService/upload
```Shell
ACCESS_TOKEN=XXX               # アクセストークンに置き換えてください
ACCOUNT_ID=XXX                 # アカウントIDに置き換えてください
VIDEO_FILE=/path/to/sample.mp4 # アップロードするvideoファイルのパスに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
"https://ads-display.yahooapis.jp/api/v0/VideoService/upload?accountId=${ACCOUNT_ID}&videoName=video_name.mp4&videoTitle=video_title&userStatus=ACTIVE" \
-F file=@${VIDEO_FILE}
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
set VIDEO_FILE=C:\path\to\sample.mp4
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
https://ads-display.yahooapis.jp/api/v0/VideoService/upload?accountId=%ACCOUNT_ID%^&videoName=video_name.mp4^&videoTitle=video_title^&userStatus=ACTIVE ^
-F file=@%VIDEO_FILE%
```
## appendix
### VideoService/get
```Shell
ACCESS_TOKEN=XXX # アクセストークンに置き換えてください
ACCOUNT_ID=XXX   # アカウントIDに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/VideoService/get -d \
'
{
  "accountId": '${ACCOUNT_ID}'
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/VideoService/get -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%^
}^
"
```

### VideoService/download
```Shell
ACCESS_TOKEN=XXX # アクセストークンに置き換えてください
ACCOUNT_ID=XXX   # アカウントIDに置き換えてください
MEDIA_ID=XXX     # VideoService/uploadのレスポンスのmediaIdに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
-o downloadedVideo.mp4 \
https://ads-display.yahooapis.jp/api/v0/VideoService/download -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  "mediaId": '${MEDIA_ID}',
  "qualityType": "ORIGINAL"
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
set MEDIA_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
-o downloadedVideo.mp4 ^
https://ads-display.yahooapis.jp/api/v0/VideoService/download -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%,^
  \"mediaId\": %MEDIA_ID%,^
  \"qualityType\": \"ORIGINAL\"^
}^
"
```

### VideoService/remove
```Shell
ACCESS_TOKEN=XXX # アクセストークンに置き換えてください
ACCOUNT_ID=XXX   # アカウントIDに置き換えてください
MEDIA_ID=XXX     # VideoService/uploadのレスポンスのmediaIdに置き換えてください
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/VideoService/remove -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  "operand": [
    {
      "accountId": '${ACCOUNT_ID}',
      "mediaId": '${MEDIA_ID}'
    }
  ]
}
'
```
`Windowsのコマンドプロンプト` の場合は以下のようなコマンドになります。
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
set MEDIA_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/VideoService/remove -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%,^
  \"operand\": [^
    {^
      \"accountId\": %ACCOUNT_ID%,^
      \"mediaId\": %MEDIA_ID%^
    }^
  ]^
}^
"
```
