FORMAT: 1A
HOST: https://api.growthpush.com/3

# Growth Push API v3
[Markdown](http://daringfireball.net/projects/markdown/syntax) **formatted** description.

## Description
Growth Push API v3 のドキュメントです 最新版は[Growth Push API v4](../v4)を御覧ください

# Group Clients

## Client [/clients/{clientId}{?applicationId}{&credentialId}{&token}]
登録済みクライアントの取得・更新・登録.

### Get Clients [GET]
クライアントの取得.

+ Parameters

    + applicationId: `PIaD6TaVt7wvKwao` (required, string) - Growthbeat アプリケーションID
    + credentialId: `7gpJ51ZQPJhNzrk0p0ThnNwh9jVcDlRS` (required, string) - Growthbeat クレデンシャルキー
    + token: `62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c` (required, string) - デバイストークン

+ Response 200 (application/json)

    + Body

            {
                "growthbeatApplicationId": "PIaD6TaVt7wvKwao",
                "code": "u0bAExbsVIz1Z1Pt",
                "os": "ios",
                "applicationId": 7700,
                "growthbeatClientId": "PbdeyiRjTNcmxMHE",
                "created": "2016-02-03 08:40:48",
                "id": 265728187,
                "environment": "development",
                "status": "active",
                "token": "62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c"
            }

+ Response 404 (application/json)

    + Body

            {
                "error": "Note not found"
            }

### Create New Clients [POST]
クライアントの作成.


+ Request with body (application/json)

    + Body

            {
                "clientId":"2KTYrVuTZOyJbEZp",
                "credentialId":"7gpJ51ZQPJhNzrk0p0ThnNwh9jVcDlRS",
                "token":"62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c",
                "growthbeatClientId":"2KTYrVuTZOyJbEZp",
                "token":"5a7e69734d159c8b3798acd3f34d8fc3b691e975fb813f67bff0f25be1d270f4",
                "os":"ios",
                "environment":"development"
            }

+ Response 200 (application/json)

    + Body
            {
                "id":265728187,
                "applicationId":7700,
                "code":"1KWVpSLCud3ba736",
                "growthbeatClientId":"2KTYrVuTZOyJbEZp",
                "token":"62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c",
                "os":"ios",
                "environment":"development",
                "status":"validating",
                "created":"2013-07-01 16:52:42"
            }

### Update Clients [PUT]

+ Parameters

    + clientId: `PIaD6TaVt7wvKwao` (required, string) - Growthbeat クライアントID

+ Request (application/json)

    + Headers

    + Body

            {
                "clientId": "2KTYrVuTZOyJbEZp",
                "credentialId":"7gpJ51ZQPJhNzrk0p0ThnNwh9jVcDlRS",
                "token":"62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c",
                "os":"ios",
                "environment":"development"
            }

+ Response

    + Body

            {
                "id":265728187,
                "applicationId":7700,
                "code":"1KWVpSLCud3ba736",
                "growthbeatClientId":"2KTYrVuTZOyJbEZp",
                "token":"62fb1363ac2336a9f188ff1cc6e54aeaf59a85c149e3f049643cb6f78357565c",
                "os":"ios",
                "environment":"development",
                "status":"validating",
                "created":"2013-07-01 16:52:42"
            }
