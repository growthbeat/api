FORMAT: 1A
HOST: https://api.growthpush.com/1

# Growth Push API v1

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** ですので、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】2016年6月現在ご利用のSDKはサポート対象外となります](https://sirok-growthbeat.amebaownd.com/posts/970274) ブログをご覧ください.
:::

# Group Clients

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

## Get Clients [GET /clients{?applicationId}{&secret}{&token}]
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, number) - Growth Push アプリケーションID
    + secret: (required, string) - Growth Push シークレットキー
    + token: (required, string) - デバイストークン

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Create New Client [POST /clients]
新規クライアント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: APPLICATION_ID (required, number) - Growth Push アプリケーションID
        + secret: SECRET (required, string) - Growth Push シークレットキー
        + token: DEVICE_TOKEN (required, string) - デバイストークン
        + os: ios (required, enum[string]) - OS
            + ios
            + android
        + environment: development (required, enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Update a Client [PUT /clients/{growthPushClientId}]
クライアント更新
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + growthPushClientId: (number) - Growth Push クライアントID

+ Request (application/json)
    + Headers
    + Attributes
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャル
        + token: DEVICE_TOKEN (string) - デバイストークン
        + environment: development (enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

# Group Events

**Event Object**

Name|Type|Note
:---|---|---
goalId|number|イベントID
timestamp|number|作成日時
clientId|number|Growth Push クライアントID
value|string|イベント値

## Get Events [GET /events{?goalId}{&credentialId}{&exclusiveTimestamp}{&limit}{&order}]
イベント取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + goalId: (number) - イベントID
    + secret: (string) - Growth Push シークレットキー
    + exclusiveTimestamp: (optional, number) - タイムスタンプ
    + limit: (number, optional) - リミット
        + Default: 100
    + order: (string, optional) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (array[Goal])

## Create New Event By Client Id [POST /events]
新規イベント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTH_PUSH_CLIENT_ID (required, number) - Growth Push クライアントID
        + code: CODE (required, string) - Growth Push Code
        + name: NAME (required, string) - イベント名
        + value: VALUE (optional, string) - イベント値

+ Response 200 (application/json)
    + Attributes (Goal)

## Create New Event By Token [POST /events]
新規イベント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTH_PUSH_APPLICATION_ID (required, number) - Growth Push アプリケーションID
        + secret: (string) - Growth Push シークレットキー
        + token: DEVICE_TOKEN (string) - デバイストークン
        + name: NAME (required, string) - イベント名
        + value: VALUE (optional, string) - イベント値

+ Response 200 (application/json)
    + Attributes (Goal)

# Group Segments

**Segment Object**

Name|Type|Note
:---|---|---
id|number|セグメントID
name|string|セグメント名
query|string|**JSON形式** のセグメント。セグメントには、タグ・イベント・セグメントをかけ合わせて組み合わせることができます。
size|number|セグメント対象人数 詳細は [セグメントの概算人数とは？いつ更新されるのか？](http://faq.growthbeat.com/article/166-article) 参照。
invisible|boolean|削除フラグ
modified|string|更新日時 ( YYYY-MM-DD HH:mm:ss )
created|string|作成日時 ( YYYY-MM-DD HH:mm:ss )

## Get Segments [GET /segments{?applicationId}{&secret}{&page}{&limit}]
セグメント一覧取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, number) - Growth Push アプリケーションID
    + secret: (required, string) - Growth Push シークレットキー
    + page: (optional, number) - ページ数
        + Default: 1
    + limit: (optional, number) - リミット
        + Default: 100

+ Response 200 (application/json)
    + Attributes (array[Segment])

## Get Segment Size [GET /segments/size{?applicationId}{&secret}{&condition}]
セグメントサイズ取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, number) - Growth Push アプリケーションID
    + secret: (required, string) - Growth Push シークレットキー
    + condition: (required, string) - query（**JSON形式** のセグメント）

+ Response 200 (application/json)
    + Attributes (number)

## Create New Segment [POST /segments]
新規セグメント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: (required, number) - Growth Push アプリケーションID
        + secret: SECRET (required, string) - Growth Push シークレットキー
        + name: NAME (required, string) - セグメント名
        + query: QUERY (required, string) - **JSON形式** のセグメント

+ Response 200 (application/json)
    + Attributes (Segment)

## Update a Segment [PUT /segments/{segmentId}]
セグメント更新
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + segmentId: (required, string) - セグメントID

+ Request (application/json)
    + Headers
    + Attributes
        + secret: SECRET (required, string) - Growth Push シークレットキー
        + name: NAME (string) - セグメント名
        + query: QUERY (string) - **JSON形式** のセグメント

+ Response 200 (application/json)
    + Attributes (Segment)

# Group Tags

**Tag Object**

Name|Type|Note
:---|---|---
id|number|タグID
applicationId|number|Growth Push アプリケーションID
type|enum|タグタイプ ( custom/message )
name|string|タグ名
value|string|タグ値
invisible|boolean|削除フラグ
created|string|作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Tag [GET /tags{?applicationId}{&credentialId}{&name}]
タグ取得
新規セグメント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, number) - Grwoth Push アプリケーションID
    + secret: (required, string) - Growth Push シークレットキー
    + name: (required, string) - タグ名

+ Response 200 (application/json)
    + Attributes (Tag)

# Group Tag Clients

::: note
タグに紐づくクライアントを抽出するAPIです
:::

**Tag Client Object**

Name|Type|Note
:---|---|---
tagId|number|タグID
clientId|number|Growth Push クライアントID
value|string|タグ値

## Create New Tags [POST /tags]
タグクライアントの作成
新規セグメント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

::: note
* このAPIは、指定のデバイスにまとめてタグ付けをします。既にタグが登録されている場合は、そのvalueを更新します。
* リクエスト数は指定したタグの件数分カウントされます。また、Notificationタグは更新する事はできません。
* 大量のタグを更新することを想定しているため **反映までに時間がかかる場合があります(数時間以上かかる場合があります)**  。即時性が必要な場合は、1件ずつのタグ付けを利用してください。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + {"clientId":"GROWTH_PUSH_CLIENT_ID","code":"GROWTH_PUSH_CODE","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]} (required, string) - JSON

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Create New Tag By Client Id [POST /tags]
タグクライアントの作成
新規セグメント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTH_PUSH_CLIENT_ID (required, number) - Growth Push クライアントID
        + code: CODE (required, string) - Growth Push Code
        + name: NAME (required, string) - タグ名
        + value: VALUE (optional, string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

## Create New Tag By Device Token [POST /tags]
タグクライアントの作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + secret: SECRET (required, number) - Growth Push シークレットキー
        + token: DEVICE_TOKEN (required, string) - デバイストークン
        + name: NAME (required, string) - タグ名
        + value: VALUE (optional, string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

# Group Notifications

**Notification Object**

Name|Type|Note
:---|---|---
id|number|Notification ID
applicationId|number|Growth Push アプリケーションID
status|enum|送信状態 ( waiting/success/failure ) 
created|string|作成日時 ( YYYY-MM-DD HH:mm:ss )
attachNotificationId|boolean|指定するとペイロードに "growthpush":{"notificationId":xxxxx} という形式で、通知IDが含まれます。
duration|number|配信時刻からこの時間以上の時間が経過したpushは配信されずに破棄されます。ミリ秒で指定してください。
trial|array[Trial]|
segment|Segment|

**Trial Object**

Name|Type|Note
:---|---|---
id|number|トライアルID
notifiationId|number|Notification ID
automationId|number| 自動配信ID
text|string|配信文言
sound|boolean|通知音
badge|boolean|通知バッジ
extra|boolean|ペイロードに任意のパラメータを **JSON形式** で指定します。ex. {"url":"https://growthpush.com"}
scheduled|string|配信予定時刻 ( YYYY-MM-DD HH:mm:ss )
status|enum|送信状態 ( standby/creating/waiting/pending/sending/completed )

## Get Notifications [GET /notifications{?applicationId}{&secret}{&page}{&limit}]
配信一覧所得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, number) - Growth Push アプリケーションID
    + secret: (required, string) - Growth Push シークレットキー
    + page: (optional, number) - ページ数
        + Default: 1
    + limit: (optional, number) - リミット
        + Default: 100

+ Response 200 (application/json)
    + Attributes (array[Notification])

## Create New Notification [POST /notifications]
新規配信作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTH_PUSH_APPLICATION_ID (required, number) - Growth Push アプリケーションID
        + secret: SECRET (required, string) - Growth Push シークレットキー
        + query: {} (string) - セグメントクエリ
        + text: text (string) - テキスト
        + sound: true (boolean) - 通知音
        + badge: true (boolean) - 通知バッジ
        + extra: {} (string) - 任意のパラメータを追加
        + attachNotificationId: true (boolean) - 通知IDの追加

+ Response 200 (application/json)
    + Attributes (Job)

# Data Structures

## Result (object)
+ result: true (boolean)

## Timestamp (object)
+ created: `2015-02-03 12:34:56` (string)

## Growthbeat (object)
+ growthbeatClientId: GROWTHBEAT_CLIENT_ID (string)
+ growthbeatApplicationId: GRWOTHBEAT_APPLICATION_ID (string)

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
+ status: 400 (number)
+ message: Bad Request. (string)

## 401 (object)
+ status: 401 (number)
+ message: Unauthorized. (string)