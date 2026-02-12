# **Apple ログイン API**

## **概要**

Apple Sign-In を利用した管理者・スタッフのログインフローです。

---

## **1. OAuth 認証開始**

### **エンドポイント**

```
/admin/auth/oauth/start?provider=apple

```

### **HTTPメソッド**

```
GET

```

### **クエリパラメータ**

| パラメータ名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| provider | string | 認証プロバイダ（"apple" 固定） | ✕ |

### **正常系レスポンス（200 OK）**

| フィールド名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| authUrl | string | Apple 認証画面のURL | ✕ |

### **レスポンス例**

```json
{
  "authUrl": "https://appleid.apple.com/auth/authorize?client_id=..."
}

```

---

## **2. OAuth コールバック**

### **エンドポイント**

```
/admin/auth/oauth/callback

```

### **HTTPメソッド**

```
GET

```

### **クエリパラメータ**

| パラメータ名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| code | string | Apple から返される認証コード | ✕ |
| state | string | CSRF 防止用の state パラメータ | ✕ |

---

## **3. トークン交換**

### **エンドポイント**

```
/admin/auth/oauth/token

```

### **HTTPメソッド**

```
POST

```

### **リクエストボディ**

| パラメータ名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| code | string | コールバックで取得した認証コード | ✕ |
| storeId | string | 店舗ID | ✕ |

### **正常系レスポンス（200 OK）**

| フィールド名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| isSuccess | boolean | ログイン成功フラグ | ✕ |
| accessToken | string | アクセストークン（JWT） | ✕ |
| refreshToken | string | リフレッシュトークン | ✕ |
| userId | string | Supabase Auth のユーザーID | ✕ |
| storeId | string | 店舗ID | ✕ |
| staffId | string | スタッフID | ✕ |
| staffName | string | スタッフ名 | ✕ |
| role | string | ロール（"admin" または "staff"） | ✕ |

### **レスポンス例**

```json
{
  "isSuccess": true,
  "accessToken": "xxxxx.yyyyy.zzzzz",
  "refreshToken": "rrrrr.sssss.ttttt",
  "userId": "a1b2c3d4e5",
  "storeId": "1001",
  "staffId": "stf-00123",
  "staffName": "管理者A",
  "role": "admin"
}

```

---

## **エラーレスポンス**

### **400 Bad Request**

```json
{
  "error": "Invalid authorization code"
}

```

### **404 Not Found（ユーザーが存在しない）**

```json
{
  "error": "User not found"
}

```

### **500 Internal Server Error**

```json
{
  "error": "Internal server error"
}

```

---
