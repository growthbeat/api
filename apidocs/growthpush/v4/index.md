FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API v4

# Group API Overview
## Rate Limits

APIの呼び出しにはリクエスト制限が設けられております。リクエスト制限を超えた場合は、 429 (Too Many Requests) のエラーコードが返却されます。以下がそれぞれのAPIに設けられている制限となります。

Clients API : 10 requests in 10 seconds.

Notifications API : 10 requests in 30 seconds.

Tags / Events API : 10 requests in 30 seconds.

All Other Resources API : 10 requests in 10 seconds.

例えば…

## Error Codes & Responses
:::note
エラーコード作ったほうが良い？
::::

**HTTP Status Codes**

 Code | Text | Description
 :---- | ------ | -----------
 200  | OK | 
 400  | Bad Request | パラメーターに誤りがあります
 401  | Unauthorized | 認証が失敗しました
 403  | Forbidden | 権限がありません
 404  | Not Found | URIが見つかりません
 409  | Conflict | 競合するデータがあるためリクエストが受け付けられません
 429  | Too Many Requests | APIのリクエスト制限に達しています
 500  | Internal Server Error | 不具合が発生しております.時間を置いても解消されないようでしたらお問い合わせください
 502  | Bad Gateway | システムがダウンしています
 503  | Service Unavailable | メンテナンス中です  
 504  | Gateway timeout | 負荷が集中しています.時間を置いてお試しください


**Error Responses**

エラーメッセージをJSONフォーマットで返却します。

案1
```
{
  "status": 400,
  "message": "Parameter limit cannot be larger than 1000."
}
```

案2
```
{
  "status": 400,
  "message": "Bad Request",
  "description": "Parameter limit cannot be larger than 1000."
}
```

# Group Clients

**Client Object**

::: note
* code は今後無くなる可能性があるので Client Objectから削除している
* Growth Push clientId, applicationId はレスポンスに含めない（Growthbeat を使ってもらう）
:::

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
::: note
* URLは単体とリストの見分けがつきやすいよう /clients/{id} にする
* 秒間リクエスト制限
* 制限超えた場合は、HTTP 429 “Too Many Requests”
:::

+ Parameters
    + id: (required, string) - Growthbeat クライアントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Get Client by token [GET /clients/token/{token}{?applicationId}{&credentialId}]
クライアント取得
::: note
* v3,v1 API利用者用のtokenベースのクライアント取得API
* 
:::

+ Parameters
    + token: (requeired, string) - クライアントトークン
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Get Clients [GET /clients{?applicationId}{&credentialId}{&limit}{&lessThan}{&greaterThan}]
クライアントリスト取得
::: note
* Growthbeat クライアントID の降順取得
## parameter
* lessThan: 指定値より小さい ClientId を `limit` 分取得
* greaterThan: 指定値より大きい ClientId を `limit` 分取得
* lessThan と greaterThan の両方を指定した場合はその間の値の limit 分を取得します
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 1000 min: 1
        + Default: 100
    + lessThan: (optional, string) - 指定値より小さい ClientId を `limit` 分取得
    + greaterThan: (optional, string) - 指定値より大きい ClientId を `limit` 分取得

+ Response 200 (application/json)
    + Attributes (array[GrowthbeatClient])

## Create New Client [POST /clients]
新規クライアント作成

::: note
## メモ
* `token` が登録済みのクライアントは登録されない

* iOS8 インストール/アンインストールしてもtokenが変わらないため、一度アインインストールされるとinactiveのまま
* iOS9 新規tokenとしてインストールされる。古いトークンは次期配信でinactiveに変更される
* Android iOS9の挙動と同様

## 実装設計
* growthbeatApplicationIdからgrowthPushApplicationを検索
* token が登録済みかチェック > 登録済みだったら何もしない
* token が null だったら invalid、null じゃなかったら validating に
* 登録したら activatioin なげる
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
    + Attributes (GrowthbeatClient)

+ Response 400 (application/json)
    + Attributes (400)

## Update a Client token [PUT /clients/{id}/token]
デバイストークンの更新

::: note
## 実装設計
* clientId から client を検索
* growthbeatApplicationIdからgrowthPushApplicationを検索
* token null じゃない and 変更がある
  * validating にして、activate
:::

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

::: note
## 実装設計
* clientId から client を検索
* growthbeatApplicationIdからgrowthPushApplicationを検索
* status の更新がある && active
  * validating にして activate
* active 以外
:::

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