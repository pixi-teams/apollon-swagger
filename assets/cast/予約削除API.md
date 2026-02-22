# 予約削除 API

## エンドポイント

```
DELETE /casts/{castId}/reservations/{reservationId}
```

## HTTPメソッド

```
DELETE
```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 |
| --- | --- | --- |
| castId | string | キャストID（ログイン中のキャスト） |
| reservationId | string | 予約ID（削除対象） |

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
| message | string | 処理結果メッセージ | ✕ |
| deleted_reservation_id | string | 削除された予約ID | ✕ |
| deleted_at | string (date-time) | 削除日時（ISO8601形式） | ✕ |

### レスポンス例

```json
{
  "success": true,
  "message": "予約が正常に削除されました。",
  "deleted_reservation_id": "res_98231",
  "deleted_at": "2026-02-22T10:00:00+09:00"
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 401 | Unauthorized: 認証エラー |
| 403 | Forbidden: 権限なし（他のキャストの予約削除は不可） |
| 404 | Not Found: 予約が存在しない |
| 409 | Conflict: ステータス制限により削除不可 |
| 500 | Internal Server Error: サーバーエラー |

### 例

```json
{
  "error": {
    "code": "404",
    "message": "指定された予約が見つかりません。"
  }
}
```

---
