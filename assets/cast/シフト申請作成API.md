# シフト申請作成 API

## エンドポイント

```
POST /casts/{castId}/shifts
```

## HTTPメソッド

```
POST
```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 |
| --- | --- | --- |
| castId | string | キャストID（ログイン中のキャスト） |

### クエリパラメータ

なし

---

### リクエストボディ

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| date | string | 日付（ISO8601形式: YYYY-MM-DD） | ✕ |
| start_time | string | 出勤開始日時（ISO8601形式） | ✕ |
| end_time | string | 出勤終了日時（ISO8601形式） | ✕ |
| memo | string | メモ | ◯ |

---

### リクエスト例

```json
{
  "date": "2026-02-20",
  "start_time": "2026-02-20T13:00:00+09:00",
  "end_time": "2026-02-20T21:00:00+09:00",
  "memo": "早めの出勤を希望します"
}
```

---

## 正常系レスポンス（201 Created）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| data | object | 作成結果 | ✕ |
| data.shift_id | string | シフトID | ✕ |
| data.cast_id | string | キャストID | ✕ |
| data.date | string | 日付 | ✕ |
| data.start_time | string | 出勤開始日時 | ✕ |
| data.end_time | string | 出勤終了日時 | ✕ |
| data.memo | string | メモ | ◯ |
| data.created_at | string | 作成日時 | ✕ |

### レスポンス例

```json
{
  "data": {
    "shift_id": "shift_12345",
    "cast_id": "cast_001",
    "date": "2026-02-20",
    "start_time": "2026-02-20T13:00:00+09:00",
    "end_time": "2026-02-20T21:00:00+09:00",
    "memo": "早めの出勤を希望します",
    "created_at": "2026-02-15T10:30:00+09:00"
  }
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 400 | Bad Request: 必須項目の未入力、形式エラー、不正な日付範囲 |
| 401 | Unauthorized: 認証エラー |
| 403 | Forbidden: 権限なし（他のキャストのシフト作成は不可） |
| 409 | Conflict: 同じ日付に既存シフトが存在 |
| 500 | Internal Server Error: サーバーエラー |

### 例

```json
{
  "error": {
    "code": "409",
    "message": "同じ日付にシフトが既に存在します。"
  }
}
```

---
