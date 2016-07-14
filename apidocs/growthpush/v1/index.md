FORMAT: 1A
HOST: https://api.growthpush.com/1

# Growth Push API v1
[Markdown](http://daringfireball.net/projects/markdown/syntax) **formatted** description.

## Description
Growth Push API v1 のドキュメントです. 

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
こちらのAPIは **廃止予定** ですので、最新版 [Growth Push API v4](../v4) をご利用ください.

詳細は [【重要】2016年6月現在ご利用のSDKはサポート対象外となります](https://sirok-growthbeat.amebaownd.com/posts/970274) ブログをご覧ください.
:::

# Group Clients

## Client List [/clients]
登録済みクライアントのリストの取得・更新.

### Get Clients [GET]

::: warning
## <i class="fa fa-warning"></i> Deprecation Notice
If the value for `title` or `body` is `null` or `undefined`, then the corresponding value is not modified on the server. However, if you send an empty string instead then it will **permanently overwrite** the original value.
:::

+ Response 200 (application/json)

    + Headers

            X-Request-ID: f72fc914
            X-Response-Time: 4ms

    + Attributes (NoteList)


### Create New Note [POST]
Create a new note using a title and an optional content body.

+ Request with body (application/json)

    + Body

            {
                "title": "My new note",
                "body": "This is the body"
            }

+ Response 201

+ Response 400 (application/json)

    + Body

            {
                "error": "Invalid title"
            }

+ Request without body (application/json)

    + Body

            {
                "title": "My new note"
            }

+ Response 201

+ Response 400 (application/json)

    + Body

            {
                "error": "Invalid title"
            }

## Client [/clients{?id}{&body}]
登録済みクライアントの取得・更新・登録.

+ Parameters

    + id: `68a5sdf67` (required, string) - The note ID

### Get Clients [GET]
クライアントの取得.

+ Parameters

    + body: `false` (boolean) - Set to `false` to exclude note body content.

+ Response 200 (application/json)

    + Headers

            X-Request-ID: f72fc914
            X-Response-Time: 4ms

    + Attributes (NoteData)

+ Response 404 (application/json)

    + Headers

            X-Request-ID: f72fc914
            X-Response-Time: 4ms

    + Body

            {
                "error": "Note not found"
            }