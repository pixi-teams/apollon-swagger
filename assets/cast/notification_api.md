# 通知機能 API定義書

本ドキュメントは、Pixi通知機能のAPI仕様を定義します。

**Base URL**: `https://api.pixi.example.com`

**認証**: 全てのエンドポイントでSupabase AuthのJWTトークンが必要です。

```
Authorization: Bearer <supabase_jwt_token>
```

---

# 1. 通知一覧取得

## エンドポイント

```
/api/v1/notifications
```

## HTTPメソッド

```
GET
```

---

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

| パラメータ名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| limit | integer | 取得件数（デフォルト: 20, 最大: 50） | ◯ |
| offset | integer | オフセット（デフォルト: 0） | ◯ |
| is_read | boolean | 未読/既読フィルタ（未指定の場合は全件） | ◯ |

例：

```
/api/v1/notifications?limit=20&offset=0&is_read=false
```

---

### リクエストボディ

なし

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| notifications | array<object> | 通知リスト | ✕ |
| notifications[].id | integer | 通知ID | ✕ |
| notifications[].user_id | string | ユーザーID（UUID） | ✕ |
| notifications[].type | string | 通知種別 | ✕ |
| notifications[].title | string | 通知タイトル | ✕ |
| notifications[].message | string | 通知メッセージ | ✕ |
| notifications[].related_id | integer | 関連エンティティID | ◯ |
| notifications[].related_type | string | 関連エンティティタイプ | ◯ |
| notifications[].is_read | boolean | 既読フラグ | ✕ |
| notifications[].read_at | string | 既読日時（ISO 8601形式） | ◯ |
| notifications[].created_at | string | 作成日時（ISO 8601形式） | ✕ |
| notifications[].updated_at | string | 更新日時（ISO 8601形式） | ✕ |
| notifications[].expires_at | string | 有効期限（ISO 8601形式） | ◯ |
| total | integer | 総件数 | ✕ |
| unread_count | integer | 未読件数 | ✕ |

### 通知種別（type）

| 値 | 説明 |
| --- | --- |
| reservation_created | Web予約受付 |
| reservation_approved | 予約承認 |
| reservation_rejected | 予約却下 |
| reservation_updated | 予約変更 |
| reservation_cancelled | 予約キャンセル |
| shift_approved | シフト承認 |
| salary_confirmed | 給与確定 |

### レスポンス例

```json
{
  "notifications": [
    {
      "id": 123,
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "type": "reservation_created",
      "title": "Web予約が入りました",
      "message": "田中様からフェイシャルコースの予約が入りました",
      "related_id": 456,
      "related_type": "reservation",
      "is_read": false,
      "read_at": null,
      "created_at": "2026-02-06T14:30:00Z",
      "updated_at": "2026-02-06T14:30:00Z",
      "expires_at": "2026-03-08T14:30:00Z"
    },
    {
      "id": 122,
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "type": "shift_approved",
      "title": "シフトが承認されました",
      "message": "2026年2月10日のシフトが承認されました",
      "related_id": 789,
      "related_type": "shift",
      "is_read": true,
      "read_at": "2026-02-06T15:00:00Z",
      "created_at": "2026-02-05T10:15:00Z",
      "updated_at": "2026-02-06T15:00:00Z",
      "expires_at": "2026-03-07T10:15:00Z"
    }
  ],
  "total": 45,
  "unread_count": 5
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 400 | Bad Request: パラメータの形式エラー |
| 401 | Unauthorized: 認証エラー |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Invalid query parameter: limit must be between 1 and 50"
}
```

---

# 2. 未読件数取得

## エンドポイント

```
/api/v1/notifications/unread-count
```

## HTTPメソッド

```
GET
```

---

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

なし

---

### リクエストボディ

なし

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| unread_count | integer | 未読件数 | ✕ |

### レスポンス例

```json
{
  "unread_count": 5
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 401 | Unauthorized: 認証エラー |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Unauthorized"
}
```

---

# 3. 通知既読化

## エンドポイント

```
/api/v1/notifications/:id/read
```

## HTTPメソッド

```
PATCH
```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 |
| --- | --- | --- |
| id | integer | 通知ID |

### クエリパラメータ

なし

---

### リクエストボディ

なし

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| success | boolean | 処理成功フラグ | ✕ |
| notification | object | 更新後の通知情報 | ✕ |
| notification.id | integer | 通知ID | ✕ |
| notification.is_read | boolean | 既読フラグ（必ずtrue） | ✕ |
| notification.read_at | string | 既読日時（ISO 8601形式） | ✕ |

### レスポンス例

```json
{
  "success": true,
  "notification": {
    "id": 123,
    "is_read": true,
    "read_at": "2026-02-06T15:00:00Z"
  }
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 401 | Unauthorized: 認証エラー |
| 404 | Not Found: 通知が存在しない、または自分の通知ではない |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Notification not found"
}
```

---

# 4. 一括既読化

## エンドポイント

```
/api/v1/notifications/read-all
```

## HTTPメソッド

```
PATCH
```

---

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

なし

---

### リクエストボディ

なし

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| success | boolean | 処理成功フラグ | ✕ |
| updated_count | integer | 更新件数 | ✕ |

### レスポンス例

```json
{
  "success": true,
  "updated_count": 5
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 401 | Unauthorized: 認証エラー |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Internal server error"
}
```

---

# 5. Push通知サブスクリプション登録

## エンドポイント

```
/api/v1/push/subscribe
```

## HTTPメソッド

```
POST
```

---

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

なし

---

### リクエストボディ

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| endpoint | string | Push通知エンドポイントURL | ✕ |
| keys | object | 暗号化キー | ✕ |
| keys.p256dh | string | P256DH公開鍵（Base64エンコード） | ✕ |
| keys.auth | string | Auth秘密鍵（Base64エンコード） | ✕ |
| user_agent | string | ユーザーエージェント | ◯ |
| device_type | string | デバイスタイプ（ios / android / desktop） | ◯ |

---

### リクエスト例

```json
{
  "endpoint": "https://fcm.googleapis.com/fcm/send/xxxxxx",
  "keys": {
    "p256dh": "BNcRdreALR...",
    "auth": "tBHItJI5sv..."
  },
  "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 16_0 like Mac OS X) AppleWebKit/605.1.15",
  "device_type": "ios"
}
```

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| success | boolean | 処理成功フラグ | ✕ |
| subscription | object | 登録されたサブスクリプション情報 | ✕ |
| subscription.id | integer | サブスクリプションID | ✕ |
| subscription.user_id | string | ユーザーID（UUID） | ✕ |
| subscription.endpoint | string | Push通知エンドポイント | ✕ |
| subscription.device_type | string | デバイスタイプ | ◯ |
| subscription.is_active | boolean | アクティブフラグ | ✕ |
| subscription.created_at | string | 作成日時（ISO 8601形式） | ✕ |

### レスポンス例

```json
{
  "success": true,
  "subscription": {
    "id": 789,
    "user_id": "550e8400-e29b-41d4-a716-446655440000",
    "endpoint": "https://fcm.googleapis.com/fcm/send/xxxxxx",
    "device_type": "ios",
    "is_active": true,
    "created_at": "2026-02-06T16:00:00Z"
  }
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 400 | Bad Request: 必須項目の未入力、形式エラーなど |
| 401 | Unauthorized: 認証エラー |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Invalid request: endpoint is required"
}
```

---

# 6. Push通知サブスクリプション解除

## エンドポイント

```
/api/v1/push/unsubscribe
```

## HTTPメソッド

```
DELETE
```

---

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

| パラメータ名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| endpoint | string | 解除するエンドポイントURL | ✕ |

例：

```
/api/v1/push/unsubscribe?endpoint=https%3A%2F%2Ffcm.googleapis.com%2Ffcm%2Fsend%2Fxxxxxx
```

---

### リクエストボディ

なし

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| success | boolean | 処理成功フラグ | ✕ |
| message | string | 処理結果メッセージ | ✕ |

### レスポンス例

```json
{
  "success": true,
  "message": "Subscription deleted successfully"
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 400 | Bad Request: endpointパラメータが未指定 |
| 401 | Unauthorized: 認証エラー |
| 404 | Not Found: サブスクリプションが存在しない |
| 500 | Internal Server Error: サーバー内部エラー |

### 例

```json
{
  "error": "Subscription not found"
}
```

---

# 補足情報

## 認証について

全てのAPIエンドポイントは、Supabase AuthのJWTトークンによる認証が必要です。

リクエストヘッダーに以下を含めてください：

```
Authorization: Bearer <access_token>
```

トークンの取得方法については、Supabase Authのドキュメントを参照してください。

---

## Row Level Security（RLS）

通知データはSupabase RLSによって保護されており、ユーザーは自分の通知のみアクセス可能です。

---

## レート制限

現時点では特に設定していませんが、将来的に以下のレート制限を検討しています：

- 通知一覧取得: 100回/分
- 未読件数取得: 200回/分
- 既読化: 50回/分

---

## タイムゾーン

全ての日時はUTC（協定世界時）で返却されます。

クライアント側で適切なタイムゾーンに変換してください。

---

## ページネーション

通知一覧取得APIは、`limit`と`offset`を使用したオフセットベースのページネーションをサポートしています。

次のページを取得する場合は、`offset`に現在の件数を加算してください。

例：
- 1ページ目: `?limit=20&offset=0`
- 2ページ目: `?limit=20&offset=20`
- 3ページ目: `?limit=20&offset=40`

---

## バージョニング

APIのバージョンはURLに含まれています（`/api/v1/...`）。

将来的に破壊的変更が必要な場合は、新しいバージョン（`/api/v2/...`）を作成します。
