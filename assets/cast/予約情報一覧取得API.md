# **予約情報一覧取得 API**

## エンドポイント

```jsx
/casts/reservations

```

## HTTPメソッド

```jsx
GET

```

---

## リクエスト

### パスパラメータ

なし

---

### クエリパラメータ

| パラメータ名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| date_from | string (YYYY-MM-DD) | この日付以降の予約を取得 | ◯ |
| date_to | string (YYYY-MM-DD) | この日付以前の予約を取得 | ◯ |
| status | string | 絞り込みステータス（`reserved / completed / canceled / nocall`） | ◯ |
| sort | string | 予約日時の並び順（`asc` / `desc`） | ◯ |

---

### リクエストボディ

```jsx
なし（GET のため）

```

---

## 正常系レスポンス

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| reservations | array | 予約一覧リスト | ✕ |
| reservations[].reservationId | string | 予約ID | ✕ |
| reservations[].createdAt | string | 予約作成日時 | ✕ |
| reservations[].customerName | string | 顧客名 | ✕ |
| reservations[].reservationDatetime | string | 予約開始日時 | ✕ |
| reservations[].courseName | string | コース名 | ✕ |
| reservations[].isDesignated | boolean | 指名の有無 | ✕ |
| reservations[].price | number | 売上金額 | ✕ |
| reservations[].reward | number | キャスト報酬金額 | ✕ |
| reservations[].paymentMethod | string | 決済方法 | ✕ |
| reservations[].paymentDetailId | string | 決済詳細ID | ◯ |
| reservations[].route | string | 予約経路（WEB/電話/店舗など） | ✕ |
| reservations[].status | string | 予約ステータス | ✕ |
| reservations[].roomName | string | ルーム名 | ◯ |
| reservations[].shimeiType | string | 指名タイプ（none/honshimei/photo） | ✕ |
| reservations[].endDatetime | string | 予約終了日時 | ✕ |

```json
{
  "reservations": [
    {
      "reservationId": "abc123",
      "createdAt": "2025-01-02T10:00:00Z",
      "customerName": "山田太郎",
      "reservationDatetime": "2025-01-05T13:00:00Z",
      "courseName": "60分コース",
      "isDesignated": true,
      "price": 12000,
      "reward": 6000,
      "paymentMethod": "credit",
      "paymentDetailId": "pay_987",
      "route": "WEB",
      "status": "reserved",
      "roomName": "Room A",
      "shimeiType": "honshimei",
      "endDatetime": "2025-01-05T14:00:00Z"
    }
  ]
}

```

---

## エラーレスポンス

### 400 Bad Request

```json
{
  "error": "Invalid query parameter format"
}

```

### 401 Unauthorized

```json
{
  "error": "Unauthorized"
}

```

### 500 Internal Server Error

```json
{
  "error": "Internal server error"
}

```