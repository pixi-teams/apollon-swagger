# シフト申請編集 API

## エンドポイント

```
PUT /casts/{castId}/shifts/{shiftId}
```

## HTTPメソッド

```
PUT
```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 |
| --- | --- | --- |
| castId | string | キャストID（ログイン中のキャスト） |
| shiftId | string | シフトID（編集対象） |

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
  "start_time": "2026-02-20T14:00:00+09:00",
  "end_time": "2026-02-20T22:00:00+09:00",
  "memo": "時間を1時間遅らせました"
}
```

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| data | object | 更新結果 | ✕ |
| data.shift_id | string | シフトID | ✕ |
| data.cast_id | string | キャストID | ✕ |
| data.date | string | 日付 | ✕ |
| data.start_time | string | 出勤開始日時 | ✕ |
| data.end_time | string | 出勤終了日時 | ✕ |
| data.memo | string | メモ | ◯ |
| data.updated_at | string | 更新日時 | ✕ |

### レスポンス例

```json
{
  "data": {
    "shift_id": "shift_12345",
    "cast_id": "cast_001",
    "date": "2026-02-20",
    "start_time": "2026-02-20T14:00:00+09:00",
    "end_time": "2026-02-20T22:00:00+09:00",
    "memo": "時間を1時間遅らせました",
    "updated_at": "2026-02-15T11:00:00+09:00"
  }
}
```

---

## エラーレスポンス

| ステータスコード | 内容 |
| --- | --- |
| 400 | Bad Request: 必須項目の未入力、形式エラー、不正な日付範囲 |
| 401 | Unauthorized: 認証エラー |
| 403 | Forbidden: 権限なし（他のキャストのシフト編集は不可） |
| 404 | Not Found: シフトが存在しない |
| 409 | Conflict: 重複するシフトが存在する |
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
