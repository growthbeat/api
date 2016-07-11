FORMAT: 1A
HOST: https://api.growthpush.com/3

# Growth Push API v3

::: note
Growth Push API v3 のドキュメントです 最新版は[Growth Push API v4](../v4)を御覧ください
:::

**Common Params**

 Name | Type | Notes
 :---- | ------ | -----------
 growthbeatApplicationId  | String | [Grwothbeat Application Id](http://faq.growthbeat.com/article/130-growthbeat-id)
 credentialId  | String| [Grwothbeat Credential Key (API Key)](http://faq.growthbeat.com/article/130-growthbeat-id)

# Group Client

**Client Object**

 Name | Type | Notes
 :---- | ------ | -----------
 growthbeatClientId  | String| Growthbeat Client Id
 id  | int | Client Id
 token  | String | Device Token
 os  | enum [ios/android] | Device Os
 status  | enum [unknown/validating/active/inactive/invalid] | Device Status
 environment  | enum [development/production] | Device Environment
 applicationId  | String | Growth Push Application Id
 code  | String | Growth Push Code
 created  | String [yyyy-MM-dd HH:mm:ss] | Created Date

## Client List [/clients]

### Get Clients [GET /clients{?applicationId}{&credentialId}{&token}]
Get a list of client.

+ Parameters

    + applicationId: (required, string) - Growthbeat Application Id
    + credentialId: (required, string) - Growthbeat Credential
    + token: (required, string) - Device Token

+ Response 200 (application/json)

    + Attributes (GrowthbeatClient)

+ Response 400 (application/json)

    + Attributes (400)

+ Response 401 (application/json)

    + Attributes (401)

### Create New Client [POST /clients]
Create a new client.

+ Parameters

+ Request (application/json)
    + Headers
    + Attributes
        + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat Client Id
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat Credential
        + token: TOKEN (string) - Device Token
        + os: ios (required, enum[string]) - Device Os
            + ios
            + android
        + environment: development (required, enum[string]) - Device Environment
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

+ Response 400 (application/json)
    + Attributes (400)

+ Response 401 (application/json)
    + Attributes (401)

## Client [/clients/{clientId}]

### Update a Client [PUT /clients/{clientId}]
Update a single client by clientId.

+ Parameters
     + clientId: GROWTHBEAT_CLIENT_ID (required, string) - Growthbeat Client Id

+ Request (application/json)
    + Headers
    + Attributes
        + credentialId: CREDENTIAL_ID (required, string) - Growthbeat Credential
        + token: TOKEN (required, string) - Device Token
        + environment: development (enum[string]) - Device Environment
            + production
            + development

+ Response 200 (application/json)
    + Attributes (GrowthbeatClient)

+ Response 400 (application/json)
    + Attributes (400)

+ Response 401 (application/json)
    + Attributes (401)

# Group Events

## Event [/clients/{?goalId}{&applicationId}{&credentialId}{&token}]
登録済みイベントの取得・更新・登録.

### Get Events [GET]
イベントの取得.

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

## 400 (object)
+ status: 400 (number)
+ message: Bad Request. (string)

## 401 (object)
+ status: 401 (number)
+ message: Unauthorized. (string)