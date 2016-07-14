FORMAT: 1A
HOST: https://api.growthpush.com/3

# Growth Push API v3

::: note
Growth Push API v3 のドキュメントです 最新版は[Growth Push API v4](../v4)を御覧ください
:::

**Common Params**

 Name | Type | Notes
 :---- | ------ | -----------
 growthbeatApplicationId  | string | [Grwothbeat アプリケーションID](http://faq.growthbeat.com/article/130-growthbeat-id)
 credentialId  | string| [Grwothbeat クレデンシャルID(API)](http://faq.growthbeat.com/article/130-growthbeat-id)

# Group Clients

**Clients Object**

 Name | Type | Notes
 :---- | ------ | -----------
 growthbeatClientId  | string| Growthbeat クライアントID
 id  | int | クライアントID
 token  | string | デバイストークン
 os  | enum | デバイスOS ( ios/android )
 status  | enum | デバイスステータス ( unknown/validating/active/inactive/invalid )
 environment  | enum | デバイス環境 ( development/production )
 applicationId  | string | Growth Push アプリケーションID
 code  | string | Growth Push Code
 created  | string | 作成日 ( yyyy-MM-dd HH:mm:ss )

## Get Client [GET /clients{?applicationId}{&credentialId}{&token}]
クライアント取得

+ Parameters

    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + token: (required, string) - Device Token

+ Response 200 (application/json)

    + Attributes (GrowthbeatClient)

## Create New Client [POST /clients]
新規クライアント作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: TOKEN (string) - デバイストークン
        + os: ios (required, enum[string]) - デバイスOS
            + ios
            + android
        + environment: development (required, enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Update a Client [PUT /clients/{clientId}]
クライアント更新

+ Parameters
     + clientId: (required, string) - Growthbeat クライアントID

+ Request (application/json)
    + Headers
    + Attributes
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャル
        + token: TOKEN (required, string) - デバイストークン
        + environment: development (enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

# Group Events

**Event Object**

Name|Type|Note
:---|---|---
goalId|int|イベントID
timestamp|long|作成日時
clientId|long|クライアントID
value|string|イベント値

## Get Events [GET /events{?goalId}{&credentialId}{&exclusiveTimestamp}{&limit}{&order}]
イベント取得

+ Parameters
    + goalId: (required, number) - イベントID
    + credentialId: (required, string) - Growthbeat クレデンシャル
    + exclusiveTimestamp: (number) - デバイストークン
    + limit: (number) - リミット
    + order: (number) - ascending / descending

+ Response 200 (application/json)
    + Attributes (array[Goal])

## Create New Event [POST /events]
新規イベント作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: NAME (required, string) - イベント名
        + value: VALUE (string) - イベント値

+ Response 200 (application/json)
    + Attributes (Goal)

# Group Tags

**Tag Object**

Name|Type|Note
:---|---|---
id|int|タグID
applicationId|int|アプリケーションID
type|enum|タグタイプ ( custom/message )
name|string|タグ名
value|string|タグ値
invisible|boolean|削除フラグ
created|string|作成日 ( yyyy-MM-dd HH:mm:ss )

## Get Tag Client [GET /tags{?applicationId}{&credentialId}{&name}]
タグ取得

+ Parameters
    + applicationId: (required, number) - アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + name: (required, string) - タグ名

+ Response 200 (application/json)
    + Attributes (Tag)

## Create New Tag [POST /tags]
新規タグ作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: TOKEN (required, string) - デバイストークン
        + name: NAME (required, string) - タグ名
        + value: VALUE (string) - タグ値

+ Response 200 (application/json)
    + Attributes (Tag)

# Group Tag Clients

::: note
タグに紐づくクライアントを抽出するAPIです
:::

**Tag Client Object**

Name|Type|Note
:---|---|---
tagId|int|タグID
clientId|long|クライアントID
value|string|タグ値


## Get Tag [GET /tags{?tagId}{&credentialId}{&exclusiveClientId}{&limit}{&order}]
タグクライアントリスト取得

+ Parameters
    + tagId: (required, number) - タグID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + exclusiveClientId: (number) - クライアントID
    + limit: (number) - リミット
    + order: (number) - ascending / descending

+ Response 200 (application/json)
    + Attributes (array[TagClient])


## Get Tag Client [GET /tags{?clientId}{&credentialId}]
タグクライアントの取得

+ Parameters
    + clientId: (required, number) - クライアントID
    + credentialId: (required, string) - Growthbeat クレデンシャルID

+ Response 200 (application/json)
    + Attributes (TagClient)

## Create New Tags [POST /tags]
タグクライアントの作成

::: note
* Growthbeat クライアントID と、Growthbeat クレデンシャルID を使用します。
* このAPIは、指定のデバイスにまとめてタグ付けをします。既にタグが登録されている場合は、そのvalueを更新します。
* リクエスト数は指定したタグの件数分カウントされます。また、Notificationタグは更新する事はできません。
* 大量のタグを更新することを想定しているため **反映までに時間がかかる場合があります(数時間以上かかる場合があります)**  。即時性が必要な場合は、1件ずつのタグ付けを利用してください。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + {"clientId":"GROWTHBEAT_CLIENT_ID","credentialId":"GROWTHBEAT_CREDENTIAL_ID","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]} (required, string) - JSON

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Create New Tag [POST /tags]
タグクライアントの作成

::: note
このAPIは、Growthbeat クライアントID と、Growthbeat クレデンシャルID の認証によってタグを生成します。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: NAME (required, string) - タグ名
        + value: VALUE (string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

## Create New Tag [POST /tags]
タグクライアントの作成

::: note
このAPIは、Growthbeat クレデンシャルID と、デバイストークンを元にタグを紐付けます。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: TOKEN (required, string) - デバイストークン
        + name: NAME (required, string) - タグ名
        + value: VALUE (string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

# Group Segments

**Segment Object**

Name|Type|Note
:---|---|---
id|int|セグメントID
name|string|セグメント名
query|string|**JSON形式** のセグメント。詳細は [Notification API クエリ指定方法](http://faq.growthbeat.com/article/96-notification-api) 参照。
size|int|セグメント対象人数 詳細は [セグメントの概算人数とは？いつ更新されるのか？](http://faq.growthbeat.com/article/166-article) 参照。
invisible|boolean|削除フラグ
modified|string|作成日時 ( yyyy-MM-dd HH:mm:ss )
created|string|作成日時 ( yyyy-MM-dd HH:mm:ss )

## Get Segments [GET /segments{?applicationId}{&credentialId}{&page}{&limit}]
セグメント一覧取得

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + page: (number) - ページ数
    + limit: (number) - リミット

+ Response 200 (application/json)
    + Attributes (array[Segment])

## Get Segment Size [GET /segments/size{?applicationId}{&credentialId}{&condition}]
セグメントサイズ取得

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + condition: (string) - query（**JSON形式** のセグメント）

+ Response 200 (application/json)
    + Attributes (number)

## Create New Segment [POST /segments]
新規セグメント作成

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + name: (required, string) - セグメント名
    + query: (required, string) - **JSON形式** のセグメント

+ Response 200 (application/json)
    + Attributes (Segment)

## Update a Segment [PUT /segments/{segmentId}]
セグメント更新

+ Parameters
     + segmentId: (required, string) - セグメントID

+ Request (application/json)
    + Headers
    + Attributes
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: NAME (string) - セグメント名
        + query: QUERY (string) - **JSON形式** のセグメント

+ Response 200 (application/json)
    + Attributes (Segment)

# Group Notifications

**Notification Object**

Name|Type|Note
:---|---|---
id|int|Notification ID
applicationId|int|Growth Push アプリケーションID
status|enum|送信状態 ( waiting/success/failure ) 
created|string|作成日時 ( yyyy-MM-dd HH:mm:ss )
attachNotificationId|boolean|指定するとペイロードに "growthpush":{"notificationId":xxxxx} という形式で、通知IDが含まれます。
duration|Long|配信時刻からこの時間以上の時間が経過したpushは配信されずに破棄されます。ミリ秒で指定してください。
trial|array[Trial]|
segment|Segment|

**Trial Object**

Name|Type|Note
:---|---|---
id|int|トライアルID
notifiationId|int|Notification ID
automationId|int| 自動配信ID
text|string|配信文言
sound|boolean|通知音
badge|boolean|通知バッジ
extra|boolean|ペイロードに任意のパラメータを **JSON形式** で指定します。ex. {"url":"https://growthpush.com"}
scheduled|string|配信予定時刻 ( yyyy-MM-dd HH:mm:ss )
status|enum|送信状態 ( standby/creating/waiting/pending/sending/completed )

## Get Notifications [GET /notifications{?applicationId}{&credentialId}{&page}{&limit}]
配信一覧所得

+ Parameters
    + applicationId: (required, number) - アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + page: (number) - ページ数
    + limit: (number) - リミット

+ Response 200 (application/json)
    + Attributes (array[Notification])

## Create New Notification [POST /notifications]
新規配信作成

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + query: {} (string) - セグメントクエリ
        + text: text (string) - テキスト
        + sound: true (boolean) - 通知音
        + badge: true (boolean) - 通知バッジ
        + extra: {} (string) - 任意のパラメータを追加
        + attachNotificationId: true (boolean) - 通知IDの追加
        + duration: (number) - ミリ秒で指定

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
+ applicationId: APPLICATION_ID (number)
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
+ applicationId: APPLICATION_ID (number)
+ Include Timestamp

## Notification (object)
+ id: NOTIFICATION_ID (number)
+ applicationId: APPLICATION_ID (number)
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