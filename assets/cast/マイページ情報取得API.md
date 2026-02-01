# マイページ情報取得 API

## エンドポイント

```
/casts/{castId}/mypage
```

## HTTPメソッド

```
GET
```

---

## リクエスト

### パスパラメータ

| パラメータ名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| castId | string | ログインキャストのID | ✕ |

### クエリパラメータ

（なし）

### リクエストボディ

GET のため **なし**

### ヘッダー

| ヘッダー名 | 説明 |
| --- | --- |
| Authorization | `Bearer <accessToken>`（必須） |

---

## 正常系レスポンス（200 OK）

| フィールド名 | 型 | 説明 | null許容(◯ / ✕) |
| --- | --- | --- | --- |
| castId | string | キャストID（例: cst-00001） | ✕ |
| stageName | string | 源氏名 | ✕ |
| imageUrl | string | プロフィール画像URL | ✕ |
| description | string | 店舗からのキャスト紹介文 | ✕ |
| height | number | 身長(cm) | ◯ |
| bust | number | バスト(cm) | ◯ |
| waist | number | ウエスト(cm) | ◯ |
| hip | number | ヒップ(cm) | ◯ |
| shopName | string | 所属店舗名 | ✕ |

### レスポンス例

```json
{
  "castId": "cst-00001",
  "stageName": "アリス",
  "imageUrl": "https://example.com/alice.jpg",
  "description": "新人セラピストのアリスです。癒やしの時間を提供します。",
  "height": 160,
  "bust": 85,
  "waist": 58,
  "hip": 86,
  "shopName": "Apollon Ginza"
}
```

---

## エラーレスポンス

### 401 Unauthorized（トークン無効）

```json
{
  "error": "Invalid or expired token"
}
```

### 404 Not Found（キャストが存在しない）

```json
{
  "error": "Cast not found"
}
```

### 500 Internal Server Error

```json
{
  "error": "Internal server error"
}
```

---

## 💡 補足

- `description` は店舗管理者がキャスト紹介用に設定する文章を想定
- `height` / `bust` / `waist` / `hip` は未設定の場合 `null` を返す
- `shopName` はキャストが所属する店舗名を Shop テーブルから取得して返す
