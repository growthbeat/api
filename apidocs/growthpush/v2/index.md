FORMAT: 1A
HOST: https://api.growthpush.com/2

# Growth Push API v2
Growth Push API v2 のドキュメントです.

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **2016年12月21日** をもって **廃止** となりました。APIをご利用の場合は、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】12/6再掲：一部のAPI廃止について](https://sirok-growthbeat.amebaownd.com/posts/1402007) ブログをご覧ください.
:::

**Common Params**

 Name | Type | Notes
 :---- | ------ | -----------
 applicationId  | number | Growth Push アプリケーションID
 secret  | string| [Growth Push シークレットキー](http://faq.growthbeat.com/article/50-notification-api)

# Group Notifications

**Notification Object**

Name|Type|Note
:---|---|---
id|number|Notification ID
applicationId|number|Growth Push アプリケーションID
status|enum|送信状態 ( waiting/success/failure ) 
created|string|作成日時 ( yyyy-MM-dd HH:mm:ss )
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
scheduled|string|配信予定時刻 ( yyyy-MM-dd HH:mm:ss )
status|enum|送信状態 ( standby/creating/waiting/pending/sending/completed )

**Segment Object**

Name|Type|Note
:---|---|---
id|number|セグメントID
name|string|セグメント名
query|string|**JSON形式** のセグメント。セグメントには、タグ・イベント・セグメントをかけ合わせて組み合わせることができます。
size|number|セグメント対象人数 詳細は [セグメントの概算人数とは？いつ更新されるのか？](http://faq.growthbeat.com/article/166-article) 参照。
invisible|boolean|削除フラグ
modified|string|作成日時 ( YYYY-MM-DD HH:mm:ss )
created|string|作成日時 ( YYYY-MM-DD HH:mm:ss )

## Create New Notification [POST /notifications]
::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
このAPIは **2016年12月21日** をもって **廃止** となりました。Notification APIをご利用の場合は、 [Growth Push Notification API v3](../v3) をご利用ください.
:::

:::note
このAPIは、Growth Push の認証キー(secret)を使用します。配信作成時に生成したUUIDをreturnします。 
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