# 管理者ダッシュボード取得 API 仕様書

## 概要

管理画面（admin）のダッシュボード表示に必要な情報を集約して取得するAPI。  
売上・予約数の期間別サマリ、出勤情報、案内用情報（キャスト別最短受付時刻・SNS共有用テキスト）を返却する。

本APIはダッシュボード専用の集約API（BFF）とする。

テンプレート系データは本APIでは返却しない。

---

## エンドポイント

### 単一店舗ログイン型

```
GET /admin/dashboard
```

### 複数店舗管理者対応型

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
        "status": "available"
      },
      {
        "cast_id": "cast_002",
        "cast_name": "みさ",
        "earliest_available_time": null,
        "status": "full"
      }
    ],
    "share": {
      "x_text": "本日のご案内可能時間はこちら！",
      "instagram_text": "本日のご案内状況"
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
      "x_text": "本日の出勤：あかり 10:00-19:00",
      "instagram_text": "本日の出勤情報はこちら"
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

---

## 表示ルール（フロント）

- earliest_available_time がある → `HH:mm〜`
- null ＋ status=full → 「満了」表示

---

## 備考

- 分数ではなく時刻で返却する
- ダッシュボード表示専用の値とする
- テンプレートデータは含めない
