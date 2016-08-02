FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API v4

# Group Clients

**Clients Object**

::: note
* code は今後無くなる可能性があるので Client Objectから削除している
* Growth Push clientId, applicationId はレスポンスに含めない（Growthbeat を使ってもらう）
:::

 Name | Type | Notes
 :---- | ------ | -----------
 id  | string| Growthbeat クライアントID
 applicationId  | string | [Grwothbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 token  | string | デバイストークン
 os  | enum | OS ( ios/android )
 status  | enum | プッシュ通知ステータス ( unknown/validating/active/inactive/invalid )
 environment  | enum | デバイス環境 ( development/production )
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Client [GET /clients/{growthbeatClientId}{?applicationId}{&credentialId}]
クライアント取得
::: note
* URLは単体とリストの見分けがつきやすいよう /clients/{growthbeatClientId} にする
:::

+ Parameters
    + growthbeatClientId: (required, string) - Growthbeat クライアントID   
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (ClientV4Response)


## Get Client by token [GET /clients{?token}{&applicationId}{&credentialId}]
クライアント取得
::: note
* v3,v1 API利用者用のtokenベースのクライアント取得API
* tokenはparameterにした
:::

+ Parameters
    + token: (requeired, string) - クライアントトークン
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (ClientV4Response)

## Get Clients 案1 [GET /clients{?applicationId}{&credentialId}{&limit}{&page}{&order}]
クライアントリスト取得
::: note
* limit と page で取得
* 懸念
  * asc にした時に更新が大量にあるとページングが機能しないかも
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 1000 min: 1
        + Default: 100
    + page: (optional, number) - ページ数
        + Default: 1
    + order: (string, optional) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (array[ClientV4Response])

## Get Clients 案2 [GET /clients{?applicationId}{&credentialId}{&limit}{&page}{&order}]
クライアントリスト取得
::: note
* limit と page で取得して、レスポンスを clientList object に変更
* n回リクエストしたら制限かけるみたいな感じで、リクエスト制限かけやすい？
* 懸念
  * asc にした時に更新が大量にあるとページングが機能しないかも
  * 単体取得とレスポンス形式が異なる
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 1000 min: 1
        + Default: 100
    + page: (optional, number) - ページ数
        + Default: 1
    + order: (string, optional) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (ClientListV4Response)

## Get Clients 案3 [GET /clients{?applicationId}{&credentialId}{&limit}{&exclusiveClientId}{&order}]
クライアントリスト取得
::: note
* limit と exclusiveClientId を使用
* 懸念
  * 一度 clientId を取得する必要がある
  * 単体取得とレスポンス形式が異なる
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 1000 min: 1
        + Default: 100
    + exclusiveClientId: (optional, string) - 前のページの最後のクライアントID
    + order: (string, optional) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (array[ClientV4Response])

## Get Clients 案4 [GET /clients{?applicationId}{&credentialId}{&limit}{&exclusiveClientId}{&order}]
クライアントリスト取得
::: note
* limit と exclusiveClientId を使用してレスポンスを clientList object に変更
* n回リクエストしたら制限かけるみたいな感じで、リクエスト制限かけやすい？
* 懸念
  * 一度 clientId を取得する必要がある
  * 単体取得とレスポンス形式が異なる
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 1000 min: 1
        + Default: 100
    + exclusiveClientId: (optional, string) - 前のページの最後のクライアントID
    + order: (string, optional) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (ClientListV4Response2)

## Create New Client [POST /clients]
新規クライアント作成

::: note
* `token` が登録済みのクライアントは登録されない
  * -> 設計段階で詳細に詰める
* Growthbeat クライアントID と、Growthbeat クレデンシャルID を使用
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

## Update a Client [PUT /clients/{clientId}{?applicationId}{&credentialId}]
クライアントのデバイス環境更新

:::note
* 旧tokenをnullにupdateさせる / environmentを帰る等で使えるように全体のupdateにしている
:::

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

## ClientListV4Response (object)
+ client_list: (array[ClientV4Response])
+ page: PAGE (number)
+ limit: LIMIT (number)

## ClientListV4Response2 (object)
+ client_list: (array[ClientV4Response])
+ exclusiveClientId: EXCLUCICE_CLIENT_ID (string)
+ limit: LIMIT (number)

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
