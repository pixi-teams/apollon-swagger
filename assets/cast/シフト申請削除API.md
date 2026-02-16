# シフト申請削除 API

## エンドポイント

```
DELETE /casts/{castId}/shifts/{shiftId}
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
| shiftId | string | シフトID（削除対象） |

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
| deleted_shift_id | string | 削除されたシフトID | ✕ |

### レスポンス例

```json
{
  "success": true,
  "message": "シフトが正常に削除されました。",
  "deleted_shift_id": "shift_12345"
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 401 | Unauthorized: 認証エラー |
| 403 | Forbidden: 権限なし（他のキャストのシフト削除は不可） |
| 404 | Not Found: シフトが存在しない |
| 409 | Conflict: 既に予約が入っているシフトは削除不可 |
| 500 | Internal Server Error: サーバーエラー |

### 例

```json
{
  "error": {
    "code": "404",
    "message": "指定されたシフトが見つかりません。"
  }
}
```

---
