# Authorization Hands-on

These are the list of URLs used on Authorization Hands-on.

Details are available on the following pages of [Developer Center](https://ads-developers.yahoo.co.jp/developercenter/en/startup-guide/) 

- [Add Application](https://ads-developers.yahoo.co.jp/developercenter/en/startup-guide/app-registration.html)
- [API Call](https://ads-developers.yahoo.co.jp/developercenter/en/startup-guide/api-call.html)

## 1. Add a new Application

[Add a new Application screen](https://connect-business.yahoo.co.jp/client/list)

## 2. Authorization

### Auth Request

Access to the following [Auth Request URL](https://biz-oauth.yahoo.co.jp/oauth/v1/authorize?response_type=code&client_id=YOUR_CLIENT_ID&redirect_uri=oob&scope=yahooads) via your web browser.

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/authorize
?response_type=code
&client_id=YOUR_CLIENT_ID
&redirect_uri=oob
&scope=yahooads
```

### Issue Token Request

Access to the following [Issue Token Requet URL](https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=authorization_code&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&redirect_uri=oob&code=AUTH_CODE) with `curl` or [Postman](https://www.getpostman.com/).
It can be accessed via your browser, however please note that the client secret and other information will remain on the browsing history.

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/token
?grant_type=authorization_code
&client_id=YOUR_CLIENT_ID
&client_secret=YOUR_CLIENT_SECRET
&redirect_uri=oob
&code=AUTH_CODE
```

For `curl`, use the following command:

```sh
curl -i "https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=authorization_code&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&redirect_uri=oob&code=AUTH_CODE"
```

### Re-issue Access Token

Access to the following [Re-issue Access Token URL](https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=refresh_token&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&refresh_token=REFRESH_TOKEN) with `curl` or [Postman](https://www.getpostman.com/).
It can be accessed via your browser, however please note that the client secret and other information will remain on the browsing history.

```http
https://biz-oauth.yahoo.co.jp/oauth/v1/token
?grant_type=refresh_token
&client_id=YOUR_CLIENT_ID
&client_secret=YOUR_CLIENT_SECRET
&refresh_token=REFRESH_TOKEN
```

For `curl`, use the following command:

```sh
curl -i "https://biz-oauth.yahoo.co.jp/oauth/v1/token?grant_type=refresh_token&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&refresh_token=REFRESH_TOKEN"
```

# API Call Trial Hands-on on API Reference

## 1. Add Redirect URI of API Reference

Redirect URI of API Reference can be added on "Added applications" screen.


[Added applications screen](https://connect-business.yahoo.co.jp/client/list)

During this Hands-on, we use [Yahoo! JAPAN Ads Display Ads API Reference](https://ads-developers.yahoo.co.jp/reference/ads-display-api/ ?lang=en). 

## 2. Authorization

For authorization, use the `Authorize` button on [Yahoo! JAPAN Ads Display Ads API Reference](https://ads-developers.yahoo.co.jp/reference/ads-display-api/ ?lang=en).

## 3. API Call on API Reference

For API Call, use the `Try it out` button on [Yahoo! JAPAN Ads Display Ads API Reference](https://ads-developers.yahoo.co.jp/reference/ads-display-api/ ).
During this Hands-on, let’s use `AccountService/get`.

Request body:

```json
{
  "includeTestAccount": "ALL"
}
```

# Yahoo! JAPAN Ads API Hands-on
Access with `curl` or [Postman](https://www.getpostman.com/).  
When using Postman, import `https---ads-display.yahooapis.jp.postman_collection.json` in this repository, and rewrite its Variables.  

## Get Account
For VideoService/upload (for updating) described later, get an ad account with `"authType": "UPDATABLE"`.
### AccountService/get
```Shell
ACCESS_TOKEN=XXX # Rewrite with your Access Token
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
For `Windows Command prompt`, the command as follows.
```Batchfile
set ACCESS_TOKEN=XXX
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/AccountService/get -d ^
"^
{^
  \"authType\": \"UPDATABLE\",^
  \"includeTestAccount\": \"ONLY_TEST\"^
}^
"
```
## Download Report
  
For downloading reports, create report (ReportDefinitionService/add) first, then download.  

** Downloading reports on Yahoo! JAPAN Promotional Ads API (former API) is done with the Response URL of ReportDefinitionService/get,
https://github.com/yahoojp-marketing/ydn-api-documents/blob/master/docs/en/api_reference/services/ReportDefinitionService.md#get  
however on Yahoo! JAPAN Ads API, specify reportJobId of ReportDefinitionService/add on the request of ReportDefinitionService/download for downloading reports. 

### ReportDefinitionService/add
```Shell
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/add -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  \"operand\": [
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
For `Windows Command prompt`, the command as follows.
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
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
REPORT_JOB_ID=XXX # Rewrite with reportJobId on the response of ReportDefinitionService/add
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/ReportDefinitionService/download -d ^
'
{
  "accountId": '${ACCOUNT_ID}',
  "reportJobId": '${REPORT_JOB_ID}'
}
'
```
For `Windows Command prompt`, the command as follows.
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
## Upload Videos
For uploading videos, grant a request parameter on URL in QueryString format, and submit the video file itself with multipart/form-data.  
Use `sample.mp4` in this repository as a video for uploading.  

** Uploading videos on Yahoo! JAPAN Promotional Ads API (former API) is done with the Response URL of VideoService/getUploadUrl,  
https://github.com/yahoojp-marketing/ydn-api-documents/blob/master/docs/en/api_reference/services/VideoService.md#getuploadurl  
however on Yahoo! JAPAN Ads API, you can upload directly by using VideoService/upload.
### VideoService/upload
```Shell
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
VIDEO_FILE=/path/to/sample.mp4 # Rewrite with the file path of videos to be uploaded
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
"https://ads-display.yahooapis.jp/api/v0/VideoService/upload?accountId=${ACCOUNT_ID}&videoName=video_name.mp4&videoTitle=video_title&userStatus=ACTIVE" \
-F file=@${VIDEO_FILE}
```
For `Windows Command prompt`, the command as follows.
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
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/VideoService/get -d ^
'
{
  "accountId": '${ACCOUNT_ID}'
}
'
```
For `Windows Command prompt`, the command as follows.
```Batchfile
set ACCESS_TOKEN=XXX
set ACCOUNT_ID=XXX
curl -H "Authorization: Bearer %ACCESS_TOKEN%" ^
-H "Content-Type: application/json" ^
https://ads-display.yahooapis.jp/api/v0/VideoService/get -d ^
"^
{^
  \"accountId\": %ACCOUNT_ID%,^
}^
"
```

### VideoService/download
```Shell
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
MEDIA_ID=XXX     # Rewrite with mediaId of the response on VideoService/upload
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
For `Windows Command prompt`, the command as follows.
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
ACCESS_TOKEN=XXX # Rewrite with your Access Token
ACCOUNT_ID=XXX   # Rewrite with your Account ID
MEDIA_ID=XXX     # Rewrite with mediaId of the response on VideoService/upload
curl -H "Authorization: Bearer ${ACCESS_TOKEN}" \
-H 'Content-Type: application/json' \
https://ads-display.yahooapis.jp/api/v0/VideoService/remove -d \
'
{
  "accountId": '${ACCOUNT_ID}',
  \"operand\": [^
    {
      "accountId": '${ACCOUNT_ID}',
      "mediaId": '${MEDIA_ID}'
    }
  ]
}
'
```
For `Windows Command prompt`, the command as follows.
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
      \"mediaId\": %MEDIA_ID%,^
    }^
  ]^
}^
"
```

