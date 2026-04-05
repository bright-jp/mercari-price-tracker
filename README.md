# Mercari Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Mercari Price Tracker](https://img.shields.io/badge/Mercari%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/mercari)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/mercari)

リアルタイムのMercari価格トラッキング - 日本および米国を代表するC2Cマーケットプレイス。開始方法は2つあります: **フルマネージド**のインテリジェンスプラットフォーム、または独自のパイプラインを構築するための**セルフサービスAPI**です。

---

## Option 1: Bright Insights - AI搭載価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/mercari)** は、Bright Dataのフルマネージドなリテールインテリジェンスプラットフォームです。scraperの構築も、インフラの保守も不要で、構造化され分析可能な価格データをダッシュボード、データフィード、またはBIツールにそのまま提供します。

**チームがBright Insightsを選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードとデータフィードで数分で本番稼働
- 🤖 **AI搭載のレコメンデーション** - 会話型AIアシスタントが数百万のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆるWebサイト、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイの統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - スキーマ変更、サイト更新、データ品質をBright Dataが自動で処理

**主なユースケース:**
- ✅ Mercariでの**再販価値トレンドの追跡**
- ✅ 特定商品の**出品価格と販売価格の監視**
- ✅ プラットフォーム間の**アービトラージ機会の特定**
- ✅ MAPポリシー準拠の監視と価格違反の検出
- ✅ 競合のプロモーションと販促動向の追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムやAIモデルに直接投入

> **月額$250から - [お客様向け見積もりを取得 →](https://brightdata.jp/products/insights/price-tracker/mercari)**

---

## Option 2: Web Scraper APIによるセルフサービス

独自のパイプラインを構築したいですか？ Bright Dataの**Web Scraper API**は、proxyやscrapingインフラを管理することなく、Mercariの商品データ（価格、在庫状況、レビューなど）にプログラムからアクセスできます。

### 前提条件

- Python 3.9以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Dataの**API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Mercari products用のBright Data **Web Scraper ID**（[Web Scrapers control panelで確認](https://brightdata.jp/cp/scrapers)）

### セットアップ

1. **このrepositoryをclone**

   ```bash
   git clone https://github.com/bright-jp/mercari-price-tracker.git
   cd mercari-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Web Scraper IDの確認方法**
   > [Bright Data Control Panel](https://brightdata.jp/cp/scrapers) にログインし、**Web Scrapers** に移動して、
   > "Mercari" を検索し、Web Scraper ID（形式: `gd_xxxxxxxxxxxx`）をコピーします。

---

## 使用方法

### 1. URLで特定の商品を追跡

Mercariの商品URLのリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.mercari.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリURLで商品を閲覧

Mercariのカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://mercari.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには以下のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 出品URL |
| `title` | 商品タイトル |
| `brand` | ブランド |
| `price` | 出品価格 |
| `currency` | 通貨コード |
| `condition` | 商品状態（新品/中古など） |
| `size` | サイズ（該当する場合） |
| `seller_name` | 出品者ユーザー名 |
| `seller_rating` | 出品者評価 |
| `images` | 商品画像URL |
| `description` | 商品説明 |
| `timestamp` | 収集タイムスタンプ |

### サンプル出力

```json
[
  {
    "url": "https://www.mercari.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://mercari.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返すレコードの最大数 |
| `include_errors` | boolean | `true` | 結果にエラーレポートを含める |
| `notify` | string (URL) | - | snapshotの準備完了時に呼び出すWebhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または`ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.mercari.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Mercari Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/mercari)
- 🔧 [Mercari Scraper API](https://brightdata.jp/products/web-scraper/mercari)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) により構築 - 業界をリードするWebデータプラットフォーム。*