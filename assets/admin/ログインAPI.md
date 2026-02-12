# **管理者・スタッフログイン API**

## **エンドポイント**

```
/admin/auth/login

```

## **HTTPメソッド**

```
POST

```

---

## **リクエスト**

### **リクエストボディ**

| パラメータ名 | 型 | 説明 | null許容 |
| --- | --- | --- | --- |
| storeId | string | 店舗ID | ✕ |
| emailAddress | string | メールアドレス | ◯ |
| staffId | string | スタッフID | ◯ |
| password | string | パスワード | ✕ |

**補足**

- `emailAddress` と `staffId` は **どちらか必須（OR 条件）**

---

## **正常系レスポンス（200 OK）**

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
  "error": "storeId and (emailAddress or staffId) and password are required"
}

```

### **401 Unauthorized（パスワード不一致）**

```json
{
  "error": "Invalid password"
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
