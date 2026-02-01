# データベース設計書

このドキュメントは `openapi.yml` の定義に基づいたデータベース設計です。
今後スキーマが追加・変更されることを前提としています。

## ER図

![ER図](./er_diagram.mmd)

※ ER図は [er_diagram.mmd](./er_diagram.mmd) に定義されています。


## テーブル定義詳細

### 1. stores (店舗)
店舗情報を管理します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | 店舗ID (PK) |
| name | string | Yes | 店舗名 |
| catch_phrase | string | No | キャッチフレーズ |
| description | text | No | 店舗詳細説明 |
| address | string | Yes | 住所 |
| access | string | Yes | アクセス情報 |
| business_hours | string | Yes | 営業時間 |
| phone_number | string | Yes | 電話番号 |
| line_id | string | Yes | LINE ID |
| created_at | timestamp | Yes | 作成日時 |
| updated_at | timestamp | Yes | 更新日時 |

### 2. casts (キャスト)
キャスト情報を管理します。認証情報(Supabase Auth)と紐づきます。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | キャストID (PK) |
| store_id | string | Yes | 所属店舗ID (FK) |
| user_id | string | Yes | Supabase AuthのUser ID |
| name | string | Yes | キャスト名 |
| email | string | No | メールアドレス |
| status | string | Yes | ステータス |
| created_at | timestamp | Yes | 作成日時 |
| updated_at | timestamp | Yes | 更新日時 |

### 3. customers (顧客)
顧客情報を管理します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | 顧客ID (PK) |
| store_id | string | Yes | 管理店舗ID (FK) |
| name | string | Yes | 氏名 |
| kana | string | No | カナ |
| phone | string | No | 電話番号 |
| email | string | No | メールアドレス |
| notes | text | No | 顧客メモ |
| first_visit_date | date | No | 初回来店日 |
| last_visit_date | date | No | 最終来店日 |
| visit_count | int | Yes | 来店回数 (デフォルト0) |
| created_at | timestamp | Yes | 作成日時 |
| updated_at | timestamp | Yes | 更新日時 |

### 4. courses (コース/メニュー)
店舗ごとのコース料金を管理します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | コースID (PK) |
| store_id | string | Yes | 店舗ID (FK) |
| name | string | Yes | コース名 |
| price | int | Yes | 料金 |
| duration_minutes | int | Yes | 所要時間(分) |
| description | text | No | 説明 |
| is_active | boolean | Yes | 有効フラグ |

### 5. shifts (シフト)
キャストの出勤情報を管理します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | シフトID (PK) |
| cast_id | string | Yes | キャストID (FK) |
| date | date | Yes | 日付 |
| start_time | datetime | No | 出勤開始日時 |
| end_time | datetime | No | 出勤終了日時 |
| status | string | Yes | ステータス (通常, 遅番, 休み, etc) |
| created_at | timestamp | Yes | 作成日時 |
| updated_at | timestamp | Yes | 更新日時 |

### 6. reservations (予約)
予約情報を管理します。Web予約とキャスト入力の両方に対応します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | 予約ID (PK) |
| store_id | string | Yes | 店舗ID (FK) |
| cast_id | string | Yes | キャストID (FK) |
| customer_id | string | No | 顧客ID (FK) - 新規Web予約時はNULLまたは作成後に紐付け |
| reservation_datetime | datetime | Yes | 予約日時 |
| shimei_type | string | Yes | 指名種別 (photo, regular) |
| extension | string | No | 延長時間 |
| discount | string | No | 割引 |
| payment_method | string | Yes | 支払い方法 |
| reservation_method | string | Yes | 予約経路 |
| reservation_status | string | Yes | ステータス |
| treatment_note | text | No | 施術メモ |
| customer_request | text | No | ご要望 |
| course_name | string | No | コース名 (スナップショット) |
| total_fee | int | No | 合計金額 |
| created_at | timestamp | Yes | 作成日時 |
| updated_at | timestamp | Yes | 更新日時 |

### 7. reservation_advertisements (予約_広告媒体)
予約時の広告媒体（複数選択可）を管理します。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | ID (PK) |
| reservation_id | string | Yes | 予約ID (FK) |
| media_name | string | Yes | 媒体名 |

### 8. visit_histories (来店履歴)
来店実績を管理します。予約完了後に作成される想定、または予約テーブルでステータス管理でも可ですが、履歴として独立させると集計が容易です。

| カラム名 | データ型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | string | Yes | ID (PK) |
| customer_id | string | Yes | 顧客ID (FK) |
| cast_id | string | Yes | キャストID (FK) |
| store_id | string | Yes | 店舗ID (FK) |
| visit_date | date | Yes | 来店日 |
| course_name | string | No | コース名 |
| total_fee | int | No | 支払い金額 |
| notes | text | No | メモ |
| created_at | timestamp | Yes | 作成日時 |

## 補足
- IDはUUIDまたはCUIDの利用を想定しています。
- `SalesSummary` などの集計データは、`reservations` または `visit_histories` テーブルから集計可能です。
- `TodaysShift` は `shifts` テーブルから取得します。
- `CustomerDetail` の `visitCount` などは `visit_histories` の count または `customers` テーブルのキャッシュカラムを利用します。
