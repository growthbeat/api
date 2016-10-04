FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API for SDK

# Group API Overview

## Error Codes & Responses

400系リクエストにはそれぞれ4桁のエラーコードを設けております。

:::note
10xx : API 共通

11xx : Clients API

99xx : Growthbeat 共通基盤
:::

**API 共通**

Code | Text | Description
:---- | ------ | -----------
1001 | Invaid credential. | 不正な認証キー
1002 | Application not found. | 指定のアプリケーションが見つかりません
1003 | Unauthorized. | 認証が必要です
1004 | Permission denied. | 権限がありません
1005 | Not found. | 指定のページが見つかりません
1000 | Unexpected error has occured. | 予期しないエラーが発生しました
<!--1006 | Too Many Requests. | 使用制限超過-->

**Clients API**

Code | Text | Description
:---- | ------ | -----------
1101 | Growthbeat client not found. | Growthbeat クライアント が存在しません
1102 | Client not found. | クライアントが存在しません
1103 | The OS is currently not supported. | サポート外のOSです
1104 | Invalid device token length. | 不正なデバイストークンです
1105 | Duplicate token. | トークンが重複しています
1100 | Unexpected error has occured. | 予期しないエラーが発生しました

**Growthbeat 共通基盤**

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
 applicationId  | string | [Growthbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 token  | string | デバイストークン
 os  | enum | OS ( ios \| android )
 status  | enum | プッシュ通知ステータス ( unknown \| validating \| active \| inactive \| invalid )
 environment  | enum | デバイス環境 ( development \| production )
 updated  | string | 更新日 ( YYYY-MM-DD HH:mm:ss )
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Attach Client [PUT /clients/{id}/attach]
新規クライアント作成

+ Parameters
    + id: (required, string) - Growthbeat クライアントID


+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: DEVICE_TOKEN (required, string) - デバイストークン
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

# Group TagClients

:::note
* デバイスに紐づくタグの情報を取得できます。
:::

**TagClient Object**

 Name | Type | Notes
 :---- | ------ | -----------
 tagId | number | タグID
 clientId | string | Growthbeat クライアントID
 value | string | 任意の値

## Create New TagClient [POST /tag_clients]
新規タグクライアント作成

:::note
* 既にタグクライアントが登録されている場合は、 value を更新します。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + tagId: TAG_ID (required, number) - タグID
        + value: value (optional, string) - 任意の値

+ Response 200 (application/json)
    + Attributes (TagClient)

# Group Event

**Event Object**

 Name | Type | Notes
 :---- | ------ | -----------
 eventId | number | イベントID
 clientId | string | Growthbeat クライアントID
 value | string | 任意の値
 timestamp  | number | 作成日

## Create New Event [POST /events]
新規イベントクライアント作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + eventId: EVENT_ID (required, number) - イベントID
        + value: VALUE (optional, string) - 任意の値

+ Response 200 (application/json)
    + Attributes (Event)

# Group Task

Name | Type | Notes
:---- | ------ | -----------
id | string | タスクID
applicationId | string | Growthbeat アプリケーションID
goalId | number | ゴールID
segmentId | number | セグメントID
orientation | enum | (vartical/horizontal)
goal | object | ゴール
segment | object | セグメント
messages | array | メッセージ一覧
begin | string | メッセージ開始時間
end | string | メッセージ終了時間
capacity | number | 配信上限数
stopped | string | 配信停止時間
updated | string | データ更新時間
created | string | データ作成時間

## Get list tasks [GET /tasks]

+ Parameters
    + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
    + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID


+ Response 200 (application/json)
    + Attributes (array[Task])

# Group Message

## Receive message [GET /receive]

+ Parameters
    + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
    + taskId: TASK_ID (required, string) - タスクID
    + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
    + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (Message)

## Count up show message [POST /receive/count]

+ Request (application/json)
    + Headers
    + Attributes
      + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
      + taskId: TASK_ID (required, string) - タスクID
      + messageId: MESSAGE_ID (required, string) - メッセージID
      + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
      + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID

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

## TagClient (object)
+ tagId: TAG_ID (number)
+ clientId: GROWTHBEA_CLIENT_ID (string)
+ value: VALUE (string)

## Event (object)
+ goalId: EVENT_ID (number)
+ clientId: GROWTHBEA_CLIENT_ID (string)
+ value: VALUE (string)
+ timestamp: TIEMSTAMP (number)

## Task (object)
+ id: TASK_ID (number)
+ applicationId: APPLICATION_ID (number)
+ goalId: GOAL_ID (number)
+ segmentId: SEGMENT_ID (number)
+ orientation: ORIENTATION (enum[string])
      + vartical
      + horizontal
+ goal: GOAL (object)
+ segment: SEGMENT (object)
+ messages: LIST MESSAGE (array[Message])
+ begin: `2016-10-01 12:00:00` (string)
+ end: `2016-10-30 12:00:00` (string)
+ capacity: 5 (number)
+ stopped: (string)
+ updated: `2016-10-01 12:00:00` (string)
+ created: `2016-10-01 12:00:00` (string)

## Message (object)
+ id: MESSAGE_ID (number)
+ type: MESSAGE_TYPE (enum[string])
    + plain
    + card
    + swipe
+ task: TASK (Task)
+ buttons: BUTTON (array[object])
+ background: BACKGROUND (object)
+ created: CREATED (string)

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
