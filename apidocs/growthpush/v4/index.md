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

<!-- include(../error.md) -->

# Group Clients

**Client Object**

 Name | Type | Notes
 :---- | ------ | -----------
 id  | string| Growthbeat クライアントID
 applicationId  | string | [Growthbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 token  | string | デバイストークン
 os  | enum | OS ( ios \| android )
 status  | enum | [プッシュ通知ステータス](http://support.growthbeat.com/manual/growthpush/#デバイス) ( unknown \| validating \| active \| inactive \| invalid )
 environment  | enum | デバイス環境 ( development \| production )
 updated  | string | 更新日 ( YYYY-MM-DD HH:mm:ss ) ※ UTC 時間表記となります
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss ) ※ UTC 時間表記となります

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
    + token: (required, string) - デバイストークン
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Get Client by v1 client id [GET /clients/{?v1ClientId}{&applicationId}{&credentialId}]
クライアント取得 (V1 Client APIのクライアントIDを元に)

+ Parameters
    + v1ClientId: (required, number) - v1 API ClientID
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

::: warning
* SDK と併用する場合、データの上書きが発生するため、SDK での更新が無効になる場合がございます。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
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

+ Request (application/x-www-form-urlencoded)
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
**[ステータスの種類](http://support.growthbeat.com/manual/growthpush/#デバイス)**

Status | Description
:---- | ------
active | ステータスを強制的に `active` に変更します。同一デバイスに紐づく複数デバイストークンを `active` にした場合、 **重複配信** される可能性があります。
validating | プッシュ通知可能なデバイスか検証した後、通知可能なデバイスは `active` に、通知を拒否しているデバイス、またはアンインストール済みのデバイスは `inactive` に変化します。
unknown | ステータスを `unknown` に変更します。この更新を行うとデバイスに通知が届かなくなります。
inactive | ステータスを `inactive` に変更します。この更新を行うとデバイスに通知が届かなくなります。
invalid | ステータスを `invalid` に変更します。この更新を行うとデバイスに通知が届かなくなります。
:::

+ Parameters
    + id: (string) - Growthbeat クライアントID

+ Request (application/x-www-form-urlencoded)
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
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss ) ※ UTC 時間表記となります

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
* 作成済タグ（削除済であっても）と同じ `name` のタグは作成できません
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
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

:::warning
* 最新の登録タグ100件の中から合致するものを抽出いたします。101件以上ある場合、検索結果が安定いたしません、ご注意下さい。
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

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + tagId: TAG_ID (required, number) - タグID
        + value: value (optional, string) - 任意の値

+ Response 200 (application/json)
    + Attributes (TagClient)

<!-- ## Create New TagClients [POST /tag_clients]
タグクライアントの作成

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

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + {"clientId":"GROWTHBEAT_CLIENT_ID","credentialId":"GROWTHBEAT_CREDENTIAL_ID","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]} (required, string) - JSON

+ Response 200 (application/json)
    + Attributes (Job)
-->

# Group Events

**Event Object**

 Name | Type | Notes
 :---- | ------ | -----------
 id  | number| イベントID
 applicationId  | string | [Growthbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 type | enum | イベントのタイプ ( custom \| message )
 name  | string | イベント名
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss ) ※ UTC 時間表記となります

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

::: note
* 作成済イベント（削除済であっても）と同じ `name` のイベントは作成できません
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
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
 timestamp  | number | 作成日 (ナノ秒単位)

## Create New EventClient [POST /event_clients]
新規イベントクライアント作成

+ Parameters

+ Request (application/x-www-form-urlencoded)
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

