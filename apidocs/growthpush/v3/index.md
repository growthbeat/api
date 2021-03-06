FORMAT: 1A
HOST: https://api.growthpush.com/3

# Growth Push API v3

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
NotificationAPI 以外のAPIは **廃止予定** です。APIをご利用の場合は、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】12/6再掲：一部のAPI廃止について](https://sirok-growthbeat.amebaownd.com/posts/1402007) ブログをご覧ください.
:::

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

## Get Notifications [GET /notifications{?applicationId}{&credentialId}{&page}{&limit}]
配信一覧所得

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + page: (optional, number) - ページ数
        + Default: 1
    + limit: (optional, number) - リミット
        + Default: 100

+ Response 200 (application/json)
    + Attributes (array[Notification])

## Create New Notification [POST /notifications]
新規配信作成

::: warning
配信端末が100件に満たない場合は、配信一覧画面に表示されません。予めご了承下さい。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + query: {} (string) - セグメントクエリ
        + text: text (string) - テキスト
        + sound: true (boolean) - 通知音
        + badge: true (boolean) - 通知バッジ
        + extra: {} (string) - 任意のパラメータを追加
        + attachNotificationId: true (boolean) - 通知IDの追加
        + duration: (number) - ミリ秒で指定

+ Response 200 (application/json)
    + Attributes (Job)

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
 created  | string | 作成日 ( YYYY-MM-DD HH:mm:ss )

## Get Client By Device Token [GET /clients{?applicationId}{&credentialId}{&token}]
クライアント取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + token: (required, string) - デバイストークン

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Create New Client [POST /clients]
新規クライアント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: DEVICE_TOKEN (string) - デバイストークン
        + os: ios (required, enum[string]) - OS
            + ios
            + android
        + environment: development (required, enum[string]) - デバイス環境
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

## Update a Client [PUT /clients/{clientId}]
クライアント更新
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + clientId: (string) - Growthbeat クライアントID

+ Request (application/x-www-form-urlencoded)
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

## Create New Event [POST /events]
新規イベント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
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

## Segment Query
query パラメーターに指定する JSON形式 のセグメント作成方法

**タグセグメント**
Parameter|Value|Note
:---|---|---
type|tag|タグ指定は必ず `tag` と入力してください。
tagId|任意のタグID|管理画面から tagId を確認してください
operator|equal(=) / begin_witch(前方一致) / less_equal(>=) / less_than(<) / greater_qeual(>=) / greater_than(>) / exist(存在) / in(含む)| 指定する value に合わせて変更してください
value|任意の値|抽出するタグの値

:::note
1. 男性ユーザー
```
{"type":"tag","tagId":2,"operator":"equal","value":"male"}
```

2. 指定のタグが紐づいているユーザー
```
{"type":"tag","tagId":3,"operator":"exist"}
```

3. 指定のタグに xxxx,YYYY,ZZzzAA…… の value が紐づいているユーザー (valueの値は **10件以上** 指定する必要がございます。)
```
{"type":"tag","tagId":4,"operator":"in","value":"xxxx,YYYY,ZZzzAA……"}
```
:::

**イベントセグメント**
Parameter|Value|Note
:---|---|---
type|event|イベント指定は必ず event と入力してください
goalId|任意のイベントID|管理画面から eventId(goalId) を確認してください
range|relative(相対時間) / absolute(絶対時間) |相対時間もしくは絶対時間を指定できます
begin|relative(現在時刻からの指定時間までのミリ秒) / absolute(YYYY-MM-DD HH:mm:ss) | 期限の開始時間を設定します
end|relative(現在時刻からの指定時間までのミリ秒) / absolute(YYYY-MM-DD HH:mm:ss) | 期限の終了時間を設定します
aggregation|count(発生回数) / summation(値の合計) / maximu(値の合計) / minimum(値の最小) | イベントの計算豊富お
operator|equal(=) / begin_witch(前方一致) / less_equal(>=) / less_than(<) / greater_qeual(>=)| 指定する value に合わせて変更してください
value|任意の値|aggregation に対する指定値

:::note
1. 72時間で1回以上起動しているユーザー
```
{"type":"event","goalId":1,"range":"relative","aggregation":"count","operator":"greater_equal","value":1.0,"begin":259200000,"end":0}
```
:::

**セグメント掛けあわせ**
Parameter|Value|Note
:---|---|---
type|and / or| and 演算子、 or 演算子で計算します
conditions | タグ・イベントの掛けあわせ| JSON形式

:::note
1. 男性ユーザー、かつ72時間で1回以上起動しているユーザー
```
{"type":"and","conditions":[{"type":"tag","tagId":2,"operator":"equal","value":"male"},{"type":"event","goalId":1,"range":"relative","aggregation":"count","operator":"greater_equal","value":1.0,"begin":259200000,"end":0}]}
```

2. 東京のユーザー、または神奈川のユーザー
```
{"type":"or","conditions":[{"type":"tag","tagId":4,"operator":"equal","value":"Tokyo"},{"type":"tag","tagId":4,"operator":"equal","value":"Kanagawa"}]}
```

3. ユーザーID複数して、24時間前の課金合計が5000以上のユーザー
```
{"type":"and","conditions":[{"type":"tag","tagId":25,"operator":"in","value":"111,23,90,150,72,65,55,1320"},{"type":"event","goalId":5,"range":"relative","aggregation":"summation","operator":"greater_equal","value":50000,"begin":3600000,"end":0}]}
```
:::

**否定セグメント**
Parameter|Value|Note
:---|---|---
type|not| 論理否定
conditions | タグ・イベントの掛けあわせ| JSON形式

:::note
1. 72時間で1回も起動していないユーザー
```
{"type":"not","condition":{"type":"event","goalId":1,"range":"relative","aggregation":"count","operator":"greater_equal","value":1.0,"begin":259200000,"end":0}}
```
:::

**既存セグメント**
Parameter|Value|Note
:---|---|---
type|segment| セグメント指定の場合は必ず segment と入力してください
segmentId | 任意のセグメントID| 既に作成済みのセグメントIDを指定してください

:::note
1. セグメントを指定かつ、OSがiOSのユーザー
```
{"type":"and","conditions":[{"type":"segment","segmentId":10},{"type":"tag","tagId":4,"operator":"begin_with","value":"iOS"}]}
```
:::

## Get Segments [GET /segments{?applicationId}{&credentialId}{&page}{&limit}]
セグメント一覧取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + page: (optional, number) - ページ数
        + Default: 1
    + limit: (optional, number) - リミット
        + Default: 100

+ Response 200 (application/json)
    + Attributes (array[Segment])

## Get Segment Size [GET /segments/size{?applicationId}{&credentialId}{&condition}]
セグメントサイズ取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + condition: (required, string) - query（**JSON形式** のセグメント）

+ Response 200 (application/json)
    + Attributes (number)

## Create New Segment [POST /segments]
新規セグメント作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: NAME (required, string) - セグメント名
        + query: QUERY (required, string) - **JSON形式** のセグメント

+ Response 200 (application/json)
    + Attributes (Segment)

## Update a Segment [PUT /segments/{segmentId}]
セグメント更新
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + segmentId: (required, string) - セグメントID

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
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
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + applicationId: (required, string) - Growthbeat アプリケーションID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
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


## Get Tag Clients By Tag Id [GET /tags{?tagId}{&credentialId}{&exclusiveClientId}{&limit}{&order}]
タグクライアントリスト取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + tagId: (required, number) - タグID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + exclusiveClientId: (optional, number) - Growth Push クライアントID
    + limit: (optional, number) - リミット
        + Default: 100
    + order: (optional, string) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (array[TagClient])


## Get Tag Clients By Client Id [GET /tags{?clientId}{&credentialId}{&type}{&exclusiveTagId}{&limit}{&order}]
タグクライアントの取得
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

+ Parameters
    + clientId: (required, string) - Growthbeat クライアントID
    + credentialId: (required, string) - Growthbeat クレデンシャルID
    + type: (optional, string) - タグタイプ
        + Members
            + `custom`
            + `notification`
            + `automation`
            + `message`
    + exclusiveTagId: (optional, number) - タグID
    + limit: (optional, number) - リミット
        + Default: 100
    + order: (optional, string) - ソート
        + Default: `descoding`
        + Members
            + `ascending`
            + `descending`

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Create New Tags [POST /tags]
タグクライアントの作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

::: note
* Growthbeat クライアントID と、Growthbeat クレデンシャルID を使用します。
* このAPIは、指定のデバイスにまとめてタグ付けをします。既にタグが登録されている場合は、そのvalueを更新します。
* リクエスト数は指定したタグの件数分カウントされます。また、Notificationタグは更新する事はできません。
* 大量のタグを更新することを想定しているため **反映までに時間がかかる場合があります(数時間以上かかる場合があります)**  。即時性が必要な場合は、1件ずつのタグ付けを利用してください。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + {"clientId":"GROWTHBEAT_CLIENT_ID","credentialId":"GROWTHBEAT_CREDENTIAL_ID","tagIdValues":[{"tagId":1,"value":"hoge"},{"tagId":2,"value":"fuga"}]} (required, string) - JSON

+ Response 200 (application/json)
    + Attributes (array[TagClient])

## Create New Tag By Client Id [POST /tags]
タグクライアントの作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

::: note
このAPIは、Growthbeat クレデンシャルID と、Growthbeat クライアントID を元にタグを紐付けます。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat クライアントID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + name: NAME (required, string) - タグ名
        + value: VALUE (string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

## Create New Tag By Device Token [POST /tags]
タグクライアントの作成
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **廃止予定** です。
:::

::: note
このAPIは、Growthbeat クレデンシャルID と、デバイストークン を元にタグを紐付けます。
:::

+ Parameters

+ Request (application/x-www-form-urlencoded)
    + Headers
    + Attributes
        + applicationId: GROWTHBEAT_APPLICATION_ID (required, string) - Growthbeat アプリケーションID
        + credentialId: GROWTHBEAT_CREDENTIAL_ID (required, string) - Growthbeat クレデンシャルID
        + token: DEVICE_TOKEN (required, string) - デバイストークン
        + name: NAME (required, string) - タグ名
        + value: VALUE (string) - タグ値

+ Response 200 (application/json)
    + Attributes (TagClient)

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