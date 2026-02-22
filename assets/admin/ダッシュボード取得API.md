# 管理者ダッシュボード取得 API 仕様書

## 概要

管理画面（admin）のダッシュボード表示に必要な情報を集約して取得するAPI。  
売上・予約数の期間別サマリ、出勤情報、案内用情報（キャスト別最短受付時刻・SNS店舗URL）を返却する。

本APIはダッシュボード専用の集約API（BFF）とする。

テンプレート系データは本APIでは返却しない。

---

## エンドポイント

```
GET /admin/dashboard
```

または

```
GET /admin/stores/{storeId}/dashboard
```

---

## 認証

- 必須（管理者認証）
- 店舗IDは認証情報またはパスパラメータから解決する

---

## レスポンス

### 200 OK

```json
{
  "summary_cards": {
    "today": {
      "sales": 120000,
      "reservation_count": 18,
      "diff_rate": 0.12,
      "diff_label": "vs_yesterday"
    },
    "yesterday": {
      "sales": 98000,
      "reservation_count": 15,
      "diff_rate": -0.08,
      "diff_label": "vs_day_before"
    },
    "this_week": {
      "sales": 540000,
      "reservation_count": 92,
      "diff_rate": 0.05,
      "diff_label": "vs_last_week"
    },
    "this_month": {
      "sales": 2100000,
      "reservation_count": 360,
      "diff_rate": 0.18,
      "diff_label": "vs_last_month"
    }
  },

  "guide_info": {
    "casts": [
      {
        "cast_id": "cast_001",
        "cast_name": "あかり",
        "earliest_available_time": "14:30",
        "status": "available",
        "badges": {
          "is_new_cast": true,
          "is_popular": false,
          "area": "渋谷",
          "room": "VIPルーム"
        }
      },
      {
        "cast_id": "cast_002",
        "cast_name": "みさ",
        "earliest_available_time": null,
        "status": "full",
        "badges": {
          "is_new_cast": false,
          "is_popular": true,
          "area": null,
          "room": "スタンダードルーム"
        }
      }
    ],
    "share": {
      "x_url": "https://x.com/store_account",
      "instagram_url": "https://instagram.com/store_account"
    }
  },

  "shift_info": {
    "date": "2026-02-17",
    "casts": [
      {
        "cast_id": "cast_001",
        "cast_name": "あかり",
        "start_time": "10:00",
        "end_time": "19:00"
      }
    ],
    "share": {
      "x_url": "https://x.com/store_account",
      "instagram_url": "https://instagram.com/store_account"
    }
  }
}
```

---

## guide_info.casts フィールド

| フィールド | 型 | 説明 |
|------------|------|------|
| cast_id | string | キャストID |
| cast_name | string | 表示名 |
| earliest_available_time | string \| null | 最短受付可能時刻（HH:mm） |
| status | string | available / full |
| badges | object | バッジ情報（下記参照） |

### badges フィールド

| フィールド | 型 | 説明 |
|------------|------|------|
| is_new_cast | boolean | `casts.created_at` から3ヶ月以内なら true |
| is_popular | boolean | 本指名（`shimei_type = 'regular'`）の累計件数が閾値（TBD）超過なら true |
| area | string \| null | `stores.area` が設定されている場合のみ返す（未設定時は null） |
| room | string | `casts.room_name` から取得（必ず返す、null不可） |

> **備考**
> - バッジは `guide_info.casts` のみに付与する（`shift_info.casts` は対象外）
> - 人気バッジの閾値は実装時に決定する（TBD）

---

## share フィールド

| フィールド | 型 | 説明 |
|------------|------|------|
| x_url | string | 店舗のXアカウントURL |
| instagram_url | string | 店舗のInstagramアカウントURL |

---

## 備考

- SNS投稿テキストはAPIでは生成しない
- 店舗SNSリンクのみ返却する
- ダッシュボード専用BFFとして扱う
- テンプレートデータは含めない
