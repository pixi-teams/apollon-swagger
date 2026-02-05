# 本日の予約情報 API

## エンドポイント

```jsx
/casts/{castId}/reservations/today

```

---

## HTTPメソッド

```jsx
GET

```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| castId | string | ログインキャストのID | ✕ |

---

### クエリパラメータ

（なし）

---

### リクエストボディ

※ GET のため **なし**

```jsx
{}

```

---

## 正常系レスポンス

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| count | number | 本日の予約件数 | ✕ |
| reservations | array | 本日の予約一覧 | ✕ |
| reservations[].reservationId | string | 予約ID | ✕ |
| reservations[].startTime | string (ISO8601) | 予約開始時刻 | ✕ |
| reservations[].endTime | string (ISO8601) | 予約終了時刻 | ✕ |
| reservations[].customerName | string | 顧客名（イニシャル・匿名も可） | ◯ |
| reservations[].menu | string | メニュー／コース名 | ✕ |
| reservations[].options | array | オプション一覧（例: 追加メニュー） | ◯ |
| reservations[].notes | string | 施術メモ | ◯ |
| reservations[].roomName | string | ルーム名 | ◯ |
| reservations[].shimeiType | string | 指名タイプ（none/honshimei/photo） | ✕ |

---

```json
{
  "count": 3,
  "reservations": [
    {
      "reservationId": "r1",
      "startTime": "2025-11-15T14:00:00+09:00",
      "endTime": "2025-11-15T15:00:00+09:00",
      "customerName": "山田",
      "menu": "60分コース",
      "options": ["ヘッドスパ", "指名料"],
      "notes": "肌荒れ対応",
      "roomName": "Room A",
      "shimeiType": "honshimei"
    },
    {
      "reservationId": "r2",
      "startTime": "2025-11-15T16:00:00+09:00",
      "endTime": "2025-11-15T17:00:00+09:00",
      "customerName": "S.K",
      "menu": "90分コース",
      "options": [],
      "notes": "",
      "roomName": "Room B",
      "shimeiType": "none"
    }
  ]
}

```

---

## エラーレスポンス

```jsx
{
  "error": {
    "message": "予約情報の取得に失敗しました",
    "code": "RESERVATION_TODAY_FETCH_FAILED"
  }
}

```