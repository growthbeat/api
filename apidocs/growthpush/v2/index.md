FORMAT: 1A
HOST: https://api.growthpush.com/2

# Growth Push API v2
Growth Push API v2 のドキュメントです.

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** ですので、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】2016年6月現在ご利用のSDKはサポート対象外となります](https://sirok-growthbeat.amebaownd.com/posts/970274) ブログをご覧ください.
:::

**Common Params**

 Name | Type | Notes
 :---- | ------ | -----------
 applicationId  | int | Growth Push アプリケーションID
 secret  | string| [Growth Push シークレットキー](http://faq.growthbeat.com/article/50-notification-api)

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

## Create New Notification [POST /notifications]
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** ですので、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】2016年6月現在ご利用のSDKはサポート対象外となります](https://sirok-growthbeat.amebaownd.com/posts/970274) ブログをご覧ください.
:::

:::note
このAPIは、Growth Push の認証キー(secret)を使用します。
配信作成時に生成したUUIDをreturnします。
:::

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + applicationId: APPLICATION_ID (required, number) - Growth Push アプリケーションID
        + secret: SECRET (required, string) - Grwoth Push シークレットキー
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

## Job (object)
+ jobId: JOB_ID (string)