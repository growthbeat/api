400系リクエストにはそれぞれ4桁のエラーコードを設けております。

:::note
10xx : 共通系

11xx : Clients 系

12xx : Tags 系　

13xx : Events 系

14xx : TagClients 系

15xx : EventClients 系

16xx : Segment 系

17xx : Task 系

18xx : Intent 系

99xx : Growthbeat 共通系
:::


**API 共通**

Code | Text | Description
:---- | ------ | -----------
1001 | Invaid credential. | 不正な認証キー
1002 | Application not found. | 指定のアプリケーションが見つかりません
1003 | Unauthorized. | 認証が必要です
1004 | Permission denied. | 権限がありません
1005 | Not found. | 指定のページが見つかりません
1000 | Unexpected error has occured. | 予期しないエラーが発生しました
<!--1006 | Too Many Requests. | 使用制限超過-->

**Clients API**

Code | Text | Description
:---- | ------ | -----------
1101 | Growthbeat client not found. | Growthbeat クライアント が存在しません
1102 | Client not found. | 指定のクライアントが存在しません
1103 | The OS is currently not supported. | サポート外のOSです
1104 | Invalid device token length. | 不正なデバイストークンです
1105 | Duplicate token. | トークンが重複しています
1100 | Unexpected error has occured. | 予期しないエラーが発生しました

**Tags API**

Code | Text | Description
:---- | ------ | -----------
1201 | Tag not found. | 指定のタグが存在しません
1202 | Duplicate tag. | タグが重複しています
1203 | Invalid tag type. | 不正なタグのタイプです

**TagClients API**

Code | Text | Description
:---- | ------ | -----------
1401 | TagClient not found. | 指定のタグクライアントが存在しません
1402 | Failed to create tagClient. | タグクライアントの作成に失敗しました
1102 | Client not found. | 指定のクライアントが存在しません
1201 | Tag not found. | 指定のタグが存在しません
1203 | Invalid tag type. | 不正なタグのタイプです

**Events API**

Code | Text | Description
:---- | ------ | -----------
1301 | Event not found. | 指定のイベントが存在しません
1302 | Duplicate event. | イベントが重複しています
1303 | Invalid event type. | 不正なイベントのタイプです

**EventClients API**

Code | Text | Description
:---- | ------ | -----------
1501 | EventClients not found. | 指定のイベントクライアントが存在しません
1502 | Failed to create eventClient. | イベントクライアントの作成に失敗しました
1102 | Client not found. | 指定のクライアントが存在しません
1301 | Event not found. | 指定のイベントが存在しません
1303 | Invalid event type. | 不正なイベントのタイプです

**Receive API**
Code | Text | Description
:---- | ------ | -----------
1611 | Segment not found. | セグメントが存在しません。
1701 | Task not found. | 配信メッセージが存在しません。
1702 | Task was stopped. | 配信メッセージは停止中です。
1703 | Task is inactive. | 配信メッセージは、配信期間外です。
1801 | Intent not found. | アクションが存在しません。

**Growthbeat 共通基盤**

Code | Text | Description
:---- | ------ | -----------
9901 | Bad Request. | 不正なリクエストです
9902 | Unauthorized. | 認証キーが異なります
9903 | Payment Required. | 使用するには支払いが必要な機能です
9904 | Forbidden. | アクセス権限がありません
9905 | Not Found. | 指定のページが見つかりません
9906 | Not Acceptable. | 受け入れ受け入れられない値があります
9907 | Unsupprted Media Type. | 予期していない値が入力されました
9908 | Unexpected error has occured. | 予期しないエラーが発生しました
9909 | Service Unavailable. | サービスを使用できません

**Error Responses**

エラーメッセージをJSONフォーマットで返却します。 `sutatus` はHTTPレスポンス、 `message` はエラーの詳細、 `code` はエラーコードを表しています。

```
{
    "status": 400,
    "message": "Invaid credential.",
    "code": 1001
}
```
