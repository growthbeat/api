FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API v4

# Group Clients

::: note
* リクエスト上限
* 
:::


**Clients Object**

 Name | Type | Notes
 :---- | ------ | -----------
 growthbeatApplicationId  | string | [Grwothbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 growthbeatClientId  | string| Growthbeat クライアントID
 id  | number | Growth Push クライアントID
 token  | string | デバイストークン
 os  | enum | OS ( ios/android )
 status  | enum | プッシュ通知ステータス ( unknown/validating/active/inactive/invalid )
 environment  | enum | デバイス環境 ( development/production )
 applicationId  | number | Growth Push アプリケーションID
 code  | string | Growth Push Code
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Client By Token [GET /clients{?applicationId}{&credentialId}{&token}]
クライアント取得

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + token: (required, string) - デバイストークン

+ Response 200 (application/json)
    + Attributes (ClientV4Response)

+ Response 400 (application/json)
    + Attributes (400)

+ Response 401 (application/json)
    + Attributes (401)

+ Response 404 (application/json)
    + Attributes (404)

+ Response 429 (application/json)
    + Attributes (429)

+ Response 500 (application/json)
    + Attributes (500)

+ Response 503 (application/json)
    + Attributes (503)

## Create a Client [POST /clients]
新規クライアント作成

::: warning
## <i class="fa fa-warning"></i> WARNING!
* `token` が登録済みのクライアントは登録されません
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: DEVICE_TOKEN (optional, string) - デバイストークン
        + os: ios (required, enum[string]) - OS
            + ios
            + android
        + environment: development (required, enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (ClientV4Response)

+ Response 400 (application/json)
    + Attributes (400)

+ Response 401 (application/json)
    + Attributes (401)

+ Response 404 (application/json)
    + Attributes (404)

+ Response 429 (application/json)
    + Attributes (429)

+ Response 500 (application/json)
    + Attributes (500)

+ Response 503 (application/json)
    + Attributes (503)

## Update a Client [PUT /clients/{clientId}]
クライアント更新

+ Parameters
    + clientId: (string) - Growthbeat クライアントID

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャル
        + token: DEVICE_TOKEN (required, string) - デバイストークン
        + environment: development (optional, enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (ClientV4Response)

+ Response 400 (application/json)
    + Attributes (400)

+ Response 401 (application/json)
    + Attributes (401)

+ Response 404 (application/json)
    + Attributes (404)

+ Response 429 (application/json)
    + Attributes (429)

+ Response 500 (application/json)
    + Attributes (500)

+ Response 503 (application/json)
    + Attributes (503)

# Data Structures

## Result (object)
+ result: true (boolean)

## Timestamp (object)
+ created: `2015-02-03 12:34:56` (string)

## Growthbeat (object)
+ growthbeatClientId: GROWTHBEAT_CLIENT_ID (string)
+ growthbeatApplicationId: GRWOTHBEAT_APPLICATION_ID (string)

## ClientV4Response (object)
+ id: GROWTHBEAT_CLIENT_ID (string)
+ applicationId: GROWTHBEAT_APPLICATION_ID (string)
+ token: DEVICE_TOKEN (string)
+ status: DEVICE_STATUS (enum[string])
    + unknown
    + validating
    + active
    + inactive
    + invalid
+ os: OS (enum[string])
    + ios
    + android
+ environment: DEVICE_ENVIRONMENT (enum[string])
    + production
    + development
+ created: `2015-02-03 12:34:56` (string)

## GrowthbeatClient (object)
+ id: CLIENT_ID (number)
+ code: DEVICE_CODE (string)
+ token: DEVICE_TOKEN (string)
+ Include Growthbeat
+ os: OS (enum[string])
    + ios
    + android
+ status: DEVICE_STATUS (enum[string])
    + unknown
    + validating
    + active
    + inactive
    + invalid
+ environment: DEVICE_ENVIRONMENT (enum[string])
    + production
    + development
+ applicationId: GROWTH_PUSH_APPLICATION_ID (number)
+ Include Timestamp

## Client (object)
+ id: CLIENT_ID (number)
+ code: DEVICE_CODE (string)
+ token: DEVICE_TOKEN (string)
+ Include Growthbeat
+ os: OS (enum[string])
    + ios
    + android
+ status: DEVICE_STATUS (enum[string])
    + unknown
    + validating
    + active
    + inactive
    + invalid
+ environment: DEVICE_ENVIRONMENT (enum[string])
    + production
    + development
+ applicationId: GROWTH_PUSH_APPLICATION_ID (number)
+ Include Timestamp

## Notification (object)
+ id: NOTIFICATION_ID (number)
+ applicationId: GROWTH_PUSH_APPLICATION_ID (number)
+ segmentId: SEGMENT_ID (number)
+ tagId: TAG_ID (number)
+ automationId: AUTOMATION_ID (number)
+ status: (enum[string])
    + waiting
    + success
    + failure
+ Include Timestamp
+ trial: (array[Trial])
+ segment: (Segment)

## Trial (object)
+ id: TRIALS_ID (number)
+ notificationId: NOTIFICATION_ID (number)
+ automationTrialId: AUTOMATION_TRIAL_ID (number)
+ text: TEXT (string)
+ sound: true (boolean)
+ badge: true (boolean)
+ extra: EXTRA (string)
+ scheduled: `2015-02-03 12:34:56` (string)
+ status: (enum[string])
   + standby
   + creating
   + waiting
   + pending
   + sending
   + completed

## Segment (object)
+ id: SEGMENT_ID (number)
+ name: NAME (string)
+ query: QUERY (string)
+ size: SIZE (number)
+ invisible: true (boolean)
+ modified: `2015-02-03 12:34:56` (string)
+ Include Timestamp

## Goal (object)
+ goalId: GOAL_ID (number)
+ timestamp: TIEMSTAMP (number)
+ clientId: CLIENT_ID (number)
+ value: VALUE (string)

## Tag (object)
+ id: TAG_ID (number)
+ applicationId: APPLICATION_ID (number)
+ type: (enum[string])
    + custom
    + message
+ invisible: true (boolean)
+ created: `2015-02-03 12:34:56` (string)

## TagClient (object)
+ tagId: TAG_ID (number)
+ clientId: CLIENT_ID (number)
+ value: VALUE (string)

## Job (object)
+ jobId: JOB_ID (string)

## 400 (object)
+ status: 400 (number) - ステータスコード
+ message: Invalid Request. (string)
+ description: Growthbeat Client id cannot be longer than 16 characters. (string) - 不正な値の説明

## 401 (object)
+ status: 401 (number)
+ message: Unauthorized. (string)
+ description: Bad credentials. (string)

## 404 (object)
+ status: 404 (number)
+ message: Not Found. (string)
+ description: Client does not exist. (string)

## 429 (object)
+ status: 429 (number)
+ message: Too many requests. (string)
+ description: Number of allowed requests has been exceeded for this API. please try again soon. (string)

## 500 (object)
+ status: 500 (number)
+ message: Internal Server Error. (string)

## 503 (object)
+ status: 503 (number)
+ message: Service Unavailable (string)
+ description: XXX/XXX is under maintenance (string)
