FORMAT: 1A
HOST: https://api.growthpush.com/2

# Growth Push API v2
[Markdown](http://daringfireball.net/projects/markdown/syntax) **formatted** description.

## Description
Growth Push API v2 のドキュメントです.

::: warning
## Deprecation Notice
こちらのAPIは **廃止予定** ですので、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】2016年6月現在ご利用のSDKはサポート対象外となります](https://sirok-growthbeat.amebaownd.com/posts/970274) ブログをご覧ください.
:::

# Group Notifications

## Notifications [/notifications]
プッシュ通知の送信.

### Create New Note [POST]
Create a new note using a title and an optional content body.

+ Request with body (application/json)

    + Body

            {
                "applicationId"=1
                "secret"="YOUR_CREDENTIAL_ID"
                "query="{\"type\":\"tag\",\"tagId\":1,\"operator\":\"equal\",\"value\":1}"
                "text"="NOTIFICATION_MESSAGE"
                "sound"=1
                "badge"=1
                "extra"=""
                "attachNotificationId"=0
                "duration"=2
            }

+ Response 200 (application/json)

    + Body
            {
                "jobId":"6cfdc958-5bb7-4b5d-b263-9fca79e24847"
            }