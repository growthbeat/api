FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API v4

# Group API Overview
## Rate Limits

APIの呼び出しにはリクエスト制限が設けられております。リクエスト制限を超えた場合は、 429 (Too Many Requests) のHTTPエラーが返却されます。以下がそれぞれのAPIに設けられている制限となります。

:::note
Clients API : 2リクエスト / 秒

All Other Resources API : 2リクエスト / 秒
:::

## Error Codes & Responses

400系リクエストにはそれぞれ4桁のエラーコードを設けております。

:::note
10xx : API 共通

11xx : Clients API

99xx : Growthbeat 共通基盤
:::

**Commons**

Code | Text | Description
:---- | ------ | -----------
1001 | Invaid credential. | 不正な認証キー
1002 | Application not found. | 指定のアプリケーションが見つかりません
1003 | Unauthorized. | 認証が必要です
1004 | Permission denied. | 権限がありません
1005 | Too Many Requests. | 使用制限超過
1000 | Unexpected error has occured. | 予期しないエラーが発生しました

**Clients**

Code | Text | Description
:---- | ------ | -----------
1101 | Growthbeat client not found. | Growthbeat クライアント が存在しません
1102 | Client not found. | クライアントが存在しません
1103 | The OS is currently not supported. | サポート外のOSです
1104 | Invalid device token length. | 不正なデバイストークンです
1105 | Duplicate token. | トークンが重複しています
1100 | Unexpected error has occured. | 予期しないエラーが発生しました


**Growthbeat**

Code | Text | Description
:---- | ------ | -----------
9901 | Bad Request. | 不正なリクエストです
9902 | Unauthorized. | 認証キーが異なります
9903 | Payment Required. | 使用するには支払いが必要な機能です
9904 | Forbidden. | アクセス権限がありません
9905 | Not Found. | 指定のページが見つかりません
9906 | Not Acceptable. | 受け入れ受け入れられない値があります
9907 | Unsupprted Media Type. | 予期していない値が入力されました
9908 | Unexpected error has occured. | 予期しないエラーが発生しました
9909 | Service Unavailable. | サービスを使用できません

**Error Responses**

エラーメッセージをJSONフォーマットで返却します。 `sutatus` はHTTPレスポンス、 `message` はエラーの詳細、 `code` はエラーコードを表しています。

```
{
    "status": 400,
    "message": "Invaid credential.",
    "code": 1001
}
```

# Group Clients

**Client Object**

 Name | Type | Notes
 :---- | ------ | -----------
 id  | string| Growthbeat クライアントID
 applicationId  | string | [Grwothbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 token  | string | デバイストークン
 os  | enum | OS ( ios \| android )
 status  | enum | プッシュ通知ステータス ( unknown \| validating \| active \| inactive \| invalid )
 environment  | enum | デバイス環境 ( development \| production )
 updated  | string | 更新日 ( YYYY-MM-DD HH:mm:ss )
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Client [GET /clients/{id}{?applicationId}{&credentialId}]
クライアント取得

+ Parameters
    + id: (required, string) - Growthbeat クライアントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Get Client by token [GET /clients/token/{token}{?applicationId}{&credentialId}]
クライアント取得

+ Parameters
    + token: (requeired, string) - クライアントトークン
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Get Clients [GET /clients{?applicationId}{&credentialId}{&limit}{&exclusiveStartId}]
クライアントリスト取得

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartId: (optional, string) - 指定値より大きい ClientId を `limit` 分取得

+ Response 200 (application/json)
    + Attributes (array[GrowthbeatClient])

## Create New Client [POST /clients]
新規クライアント作成

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
    + Attributes (GrowthbeatClient)

+ Response 400 (application/json)
    + Attributes (400)

## Update a Client token [PUT /clients/{id}/token]
デバイストークンの更新

::: warning
* SDKと併用する場合、データの上書きが発生するため、SDKでの更新が無効になる場合がございます。
:::

+ Parameters
    + id: (required, string) - Growthbeat クライアントID

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: DEVICE_TOKEN (optional, string) - デバイストークン

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Update a Client status [PUT /clients/{id}/status]
クライアントのステータス環境更新

::: warning
* SDKと併用する場合、データの上書きが発生するため、SDKでの更新が無効になる場合がございます。
:::

+ Parameters
    + id: (string) - Growthbeat クライアントID

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャル
        + status: DEVICE_STATUS (enum[string])
            + unknown
            + validating
            + active
            + inactive
            + invalid

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

# Data Structures

## Result (object)
+ result: true (boolean)

## Timestamp (object)
+ created: `2015-02-03 12:34:56` (string)

## Growthbeat (object)
+ growthbeatClientId: GROWTHBEAT_CLIENT_ID (string)
+ growthbeatApplicationId: GRWOTHBEAT_APPLICATION_ID (string)

## GrowthbeatClient (object)
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
+ updated: `2015-02-03 12:34:56` (string)
+ created: `2015-02-03 12:34:56` (string)

## ClientV4Response (object)
+ client: (GrowthbeatClient)
+ rateLimit: RATE_LIMIT (number)

## ClientListV4Response (object)
+ clients: (array[GrowthbeatClient])
+ rateLimit: RATE_LIMIT (number)
+ nextClientId: NEXT_CLIENT_ID (string)

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
+ message: Growthbeat Client id cannot be longer than 16 characters. (string) - 不正な値の説明

## 401 (object)
+ status: 401 (number)
+ message: Bad credentials. (string)

## 404 (object)
+ status: 404 (number)
+ message: Client does not exist. (string)

## 429 (object)
+ status: 429 (number)
+ message: Too many requests. (string)

## 500 (object)
+ status: 500 (number)
+ message: Internal Server Error. (string)

## 503 (object)
+ status: 503 (number)
+ message: Service Unavailable (string)