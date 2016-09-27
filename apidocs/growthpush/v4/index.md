FORMAT: 1A
HOST: https://api.growthpush.com/4

# Growth Push API v4

# Group API Overview
<!--
## Rate Limits

APIの呼び出しにはリクエスト制限が設けられております。リクエスト制限を超えた場合は、 429 (Too Many Requests) のHTTPエラーが返却されます。以下がそれぞれのAPIに設けられている制限となります。

:::note
Clients API : 2リクエスト / 秒
:::
-->

## Error Codes & Responses

400系リクエストにはそれぞれ4桁のエラーコードを設けております。

:::note
10xx : 共通系

11xx : Clients 系

12xx : Tags 系　

13xx : Events 系

14xx : TagClients 系

99xx : Growthbeat 共通系
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
1102 | Client not found. | 指定のクライアントが存在しません
1103 | The OS is currently not supported. | サポート外のOSです
1104 | Invalid device token length. | 不正なデバイストークンです
1105 | Duplicate token. | トークンが重複しています
1100 | Unexpected error has occured. | 予期しないエラーが発生しました

**Tags API**

Code | Text | Description
:---- | ------ | -----------
1201 | Tag not found. | 指定のタグが存在しません
1202 | Duplicate tag. | タグが重複しています
1203 | Tag name cannot be longer than 64 characters. | タグ名は64文字以内に設定してください
1204 | Invalid tag. | 不正なタグです

**TagClients API**

Code | Text | Description
:---- | ------ | -----------
1401 | TagClient not found. | 指定のタグクライアントが存在しません
1102 | Client not found. | 指定のクライアントが存在しません
1201 | Tag not found. | 指定のタグが存在しません

**Events API**

Code | Text | Description
:---- | ------ | -----------
1301 | Event not found. | 指定のイベントが存在しません
1302 | Duplicate event. | イベントが重複しています

**EventClients API**

Code | Text | Description
:---- | ------ | -----------
1102 | Client not found. | 指定のクライアントが存在しません
1301 | Tag not found. | 指定のイベントが存在しません

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

エラーメッセージを JSONフォーマット で返却します。 `sutatus` は HTTPレスポンス、 `message` はエラーの詳細、 `code` はエラーコードを表しています。

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
クライアント一覧取得（降順取得固定）

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartId: (optional, string) - 指定の `clientId` の次の値を取得します

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

## Update a Client token [PUT /clients/{id}/token]
デバイストークンの更新

::: warning
* SDK と併用する場合、データの上書きが発生するため、SDK での更新が無効になる場合がございます。
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
* SDK と併用する場合、データの上書きが発生するため、SDK での更新が無効になる場合がございます。
:::

::: note
**ステータスの種類**

Status | Description
:---- | ------
active | ステータスを強制的に `active` に変更します。同一端末に紐づく複数デバイストークンを `active` にした場合、 **重複配信** される可能性があります。
validating | プッシュ通知可能な端末か検証した後、通知可能な端末は `active` に、通知を拒否している端末、またはアンインストール済みの端末は `inactive` に変化します。
unknown | ステータスを `unknown` に変更します。この更新を行うと端末に通知が届かなくなります。
inactive | ステータスを `inactive` に変更します。この更新を行うと端末に通知が届かなくなります。
invalid | ステータスを `invalid` に変更します。この更新を行うと端末に通知が届かなくなります。
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

# Group Tags

**Tag Object**

 Name | Type | Notes
 :---- | ------ | -----------
 id  | number| タグID
 applicationId  | string | [Growthbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 type | enum | タグのタイプ ( custom \| notification \| automation )
 name  | string | タグ名
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Tag [GET /tags/{id}{?applicationId}{&credentialId}]
タグ取得

+ Parameters
    + id: (required, number) - タグID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (Tag)

## Get Tags [GET /tags{?applicationId}{&credentialId}]
タグ一覧取得（降順取得固定）

::: warning
# メモ
* type はデフォルト custom にする。他の type の指定は可能。
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartId: (optional, number) - 指定の `tagId` の次の値を取得します
    + type: (optional, enum[string]) - タグタイプ
        + Default: custom

+ Response 200 (application/json)
    + Attributes (array[Tag])

## Create New Tag [POST /tags]
新規タグ作成

::: note
* 同じ `name` のタグは作成できません
:::

::: warning
# メモ
* 同じ `name` のタグは作成できない
* type は custom 固定
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: TAG_NAME (required, string) - タグ名

+ Response 200 (application/json)
    + Attributes (Tag)

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

## Get TagClients by tag [GET /tag_clients/tag/{tagId}{?applicationId}{&credentialId}]
タグに紐づくクライアントを取得（降順取得固定）

::: warning
# メモ
* パスは tagId, clientId 両方を指定するパターンはないので `/tag_clients/tag/{tagId}` にしようと思う
* 指定した tagId の type は custom 以外でも取得可能
:::

+ Parameters
    + tagId: (required, number) - タグID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartClientId: (optional, string) - 指定の `clientId` の次の値を取得します

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Get TagClients by client [GET /tag_clients/client/{clientId}{?applicationId}{&credentialId}]
クライアントに紐づくタグを取得

::: warning
# メモ
* デフォルト 100 件の取得でページングは設けない
:::

+ Parameters
    + clientId: (required, string) - Growthbeat クライアントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Create New TagClient [POST /tag_clients]
新規タグクライアント作成

:::note
* 既にタグクライアントが登録されている場合は、 value を更新します。
:::

::: warning
# メモ
* 指定した tagId の type が custom の場合のみしか作成許可しない
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

## Create New TagClients [POST /tag_clients]
タグクライアントの作成

::: warning
# メモ
* 既存の仕様と統一するか、レスポンスどうするか
* 優先度低めで must ではない
:::

::: note
* この API は、指定のデバイスにまとめてタグ付けをします。既にタグクライアントが登録されている場合は、その value を更新します。
* リクエスト数は指定したタグの件数分カウントされます。また、 Notification タグは更新する事はできません。
* 大量のタグを更新することを想定しているため **反映までに時間がかかる場合があります(数時間以上かかる場合があります)**  。即時性が必要な場合は、1件ずつのタグ付けを利用してください。

* `body` には下記の `json` を指定してください
```
{
  "clientId": "GROWTHBEAT_CLIENT_ID",
  "credentialId": "GROWTHBEAT_CREDENTIAL_ID",
  "tagIdValues": [
    {
      "tagId": 1,
      "value": "hoge"
    },
    {
      "tagId": 2,
      "value": "fuga"
    },
    ……
  ]
}
```

* curl 例
```
curl -X POST \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"clientId":"GROWTHBEAT_CLIENT_ID","credentialId":"GROWTHBEAT_CREDENTIAL_ID","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]}' \
    https://api.growthpush.com/4/tag_clients
```
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + {"clientId":"GROWTHBEAT_CLIENT_ID","credentialId":"GROWTHBEAT_CREDENTIAL_ID","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]} (required, string) - JSON

+ Response 200 (application/json)
    + Attributes (Job)

# Group Events

**Event Object**

::: warning
# メモ
* 内部的な goalId , eventId の扱いは変更しなくてよいかと
  * Response @JsonProperty("id") で返却
  * converter で変換させる
:::

 Name | Type | Notes
 :---- | ------ | -----------
 id  | number| イベントID
 applicationId  | string | [Growthbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 type | enum | イベントのタイプ ( custom \| message )
 name  | string | イベント名
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Event [GET /events/{id}{?applicationId}{&credentialId}]
イベント取得

+ Parameters
    + id: (required, number) - イベントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (Event)

## Get Events [GET /events{?applicationId}{&credentialId}]
イベント一覧取得（降順取得固定）

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartId: (optional, number) - 指定の `eventId` の次の値を取得します
    + type: (optional, enum[string]) - タグタイプ
        + Default: custom

+ Response 200 (application/json)
    + Attributes (array[Event])

## Create New Event [POST /events]
新規イベント作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: EVENT_NAME (required, string) - イベント名

+ Response 200 (application/json)
    + Attributes (Event)

# Group EventClients

**EventClient Object**

 Name | Type | Notes
 :---- | ------ | -----------
 eventId | number | イベントID
 clientId | string | Growthbeat クライアントID
 value | string | 任意の値
 timestamp  | number | 作成日

<!-- 開発検討
## Get EventClients by event [GET /event_clients/in_events/{eventId}{?applicationId}{&credentialId}]
イベントに紐づくクライアントを取得
::: warning
* DynamoからexclusiveStartTIdでページングで取得できのか確認
  * DynamoからclientIdでソートして取得しているので可能
  * DynamoからのexclusiveIdを返してもらうこともできるかも
:::

+ Parameters
    + eventId: (required, number) - イベントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartClientId: (optional, string) - 指定値より小さい ClientId を `limit` 分取得

+ Response 200 (application/json)
    + Attributes (array[EventClient])

## Get TagClients by client [GET /event_clients/to_client/{clientId}{?applicationId}{&credentialId}]
クライアントに紐づくイベントを取得
::: warning
* timestamp を Date 型で送信してもらうほうがわかりやすい？どっちがいいのか？
:::

+ Parameters
    + clientId: (required, string) - Growthbeat クライアントID
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + limit: (number, optional) - max: 100 min: 1
        + Default: 100
    + exclusiveStartTimestamp: (optional, string) - 指定値より小さい timestamp を `limit` 分取得

+ Response 200 (application/json)
    + Attributes (array[TagClient])
-->

::: warning
# メモ
* v1,v3 からの移行を優先して、イベントに紐づくクライアント一覧取得 / クライアントに紐づくイベント一覧取得 は追加開発項目にする
* タイムスタンプは現在時刻を入れる仕様。
:::

## Create New EventClient [POST /event_clients]
新規イベントクライアント作成

::: warning
# メモ
* 指定した eventId の type が custom の場合のみしか作成許可しない
* タイムスタンプは作成日時にする
:::

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
    + Attributes (EventClient)

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

## Event (object)
+ id: EVENT_ID (number)
+ applicationId: GRWOTHBEAT_APPLICATION_ID (string)
+ type: (enum[string])
    + custom
    + message
+ name: EVENT_NAME (string)
+ created: `2015-02-03 12:34:56` (string)

## EventClient (object)
+ eventId: EVENT_ID (number)
+ clientId: GROWTHBEA_CLIENT_ID (string)
+ value: VALUE (string)
+ timestamp: TIEMSTAMP (number)

## Tag (object)
+ id: TAG_ID (number)
+ applicationId: GRWOTHBEAT_APPLICATION_ID (string)
+ type: (enum[string])
    + custom
    + notification
    + automation
+ name: TAG_NAME (string)
+ created: `2015-02-03 12:34:56` (string)

## TagClient (object)
+ tagId: TAG_ID (number)
+ clientId: GROWTHBEA_CLIENT_ID (string)
+ value: VALUE (string)

## Job (object)
+ jobId: JOB_ID (string)

## 400 (object)
+ status: 400 (number) - ステータスコード
+ message: Growthbeat Client id cannot be longer than 16 characters. (string) - 不正な値の説明
+ code: 1101 (number) - エラーコード

## 401 (object)
+ status: 401 (number)
+ message: Bad credentials. (string)
+ code: 1103 (number) - エラーコード

## 404 (object)
+ status: 404 (number)
+ message: Client does not exist. (string)
+ code: 1105 (number) - エラーコード

## 429 (object)
+ status: 429 (number)
+ message: Too many requests. (string)
+ code: 1100 (number) - エラーコード

## 500 (object)
+ status: 500 (number)
+ message: Internal Server Error. (string)
+ code: 1100 (number) - エラーコード

## 503 (object)
+ status: 503 (number)
+ message: Service Unavailable (string)
+ code: 1100 (number) - エラーコード