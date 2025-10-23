# 🚀 続編：APIを使ってみよう！〜準備と実践編〜
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Gifted-People-Support-Association/api-seminar/blob/main/section02-practice/notebook.ipynb)

この資料は、Web APIの概念を理解した方が、実際にAPIを操作するための基礎知識と手順をまとめたものです。<br>
概要についてはこちらをご覧ください。

## 1. APIを動かすための３つの道具
APIを使うには、レストランの注文に必要な「場所」「鍵」「道具」の３つが必要です。

### 🔑 道具 1：APIキー（認証キー）
- 役割: あなたが正規の利用者であることを証明する「会員証」や「特別な鍵」です。
- 使い方: 多くのAPIでは、このキーを注文票（URL）の一部として添える必要があります。
- 注意: 🔑 絶対に他人に教えたり、公開したりしないでください。

### 📍 道具 2：エンドポイント（住所）
- 役割: サーバーが注文を受け付けている「窓口の住所」です。
- 使い方: どのAPI（天気予報、通貨換算など）のどの機能（今日の天気、来週の予報など）を使うかによって、住所（URL）が変わります。
  - 例（Gemini API）:
文章生成機能: https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent
  - チャットセッション開始機能: https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent
  - 構成: 通常のウェブサイトのURL（例：https://team-shiny.org）と似た形をしています。

### 💻道具 3：クライアント（注文道具）
- 役割: あなたのプログラムから、エンドポイントへ注文票（リクエスト）を送信するための道具です。
- 使い方: 実際にプログラムを書く場合、JavaScriptやPythonといった言語を使いますが、テストや練習では、ブラウザや専用のツールを使います。

本ゼミでは
- ブラウザ専用ツール：Postman (https://www.postman.com)
- Python専用ライブラリ：requests (https://pypi.org/project/requests)
- Python + Gemini専用ライブラリ：python-genai (https://github.com/googleapis/python-genai)

を通じて実際にAPIを操作していきます。

## 2 APIキーの取得と設定（Google Colabの場合）
APIキーは、Webサービス提供元の専用画面で取得します。
ここでは、Gemini APIとOpenWeatherMapのキー取得とColabでの安全な利用手順を説明します。

### 2.1 Gemini API のキー取得手順

GeminiのAPIを呼び出すには、APIキーが必要です。Google AI Studioで発行したAPIキーを、Google Colabのシークレット機能を使って安全に管理します。

#### ステップ 1: Google Colabでシークレット機能を開く

左サイドバーにある **🔑 鍵アイコン（Secrets）** をクリックします。

#### ステップ 2: Google AI StudioでAPIキーを作成

1. **管理画面に移動**: Google Colabのシークレットタブにある「Google AI Studioでキーを管理」をクリックします。
   - 新しいタブで [Google AI Studio](https://aistudio.google.com/apikey) の管理画面が開きます。
2. **APIキーの作成**: 
   - 「APIキーを作成」ボタンをクリックします。
   - プロジェクトの選択画面が表示された場合は、既存のプロジェクトを選択するか、新規プロジェクトを作成します。
3. **APIキーの確認**: 
   - 作成されたAPIキー（例: `AIzaSyC...`）が表示されます。
   - このキーは後でColabにインポートするため、このタブは開いたままにしておきます。

> **注意**: 作成したAPIキーは他人に見せたり、公開リポジトリにアップロードしたりしないでください。

#### ステップ 3: ColabにAPIキーをインポート

1. **Colabに戻る**: Google Colabのタブに戻ります。
2. **キーをインポート**: シークレットタブの「Google AI Studioからキーをインポート」をクリックします。
3. **キーを選択**: 先ほど作成したAPIキーが一覧に表示されるので、使用するキーを選択します。
4. **インポート完了**: シークレットタブに **`GOOGLE_API_KEY`** という名前が追加されていることを確認します。
5. **ノートブックへのアクセスを許可**: `GOOGLE_API_KEY` の右側にある **「ノートブックでのアクセス」をオン（✔）** にします。

#### ステップ 4: 動作確認

以下のコードセルを実行し、APIキーが正しく設定されていることを確認します。
```python
import os
from google.colab import userdata

# シークレットからAPIキーを取得
api_key = userdata.get('GOOGLE_API_KEY')

# 環境変数に設定
os.environ['GOOGLE_API_KEY'] = api_key

# 確認（キー自体は表示しない）
if api_key:
    print("✅ APIキーの設定が完了しました。")
    print(f"キーの先頭: {api_key[:10]}...")
else:
    print("❌ エラー: APIキーが設定されていません。")
```

**期待される出力**:
```
✅ APIキーの設定が完了しました。
キーの先頭: AIzaSyC...
```

エラーが発生しないことを確認できたら、Gemini APIを使用する準備が整いました。

#### ステップ 5: Gemini APIのテスト

APIキーが正しく動作するか、簡単なテストを行います。
```python
from google import genai

# クライアントの初期化
client = genai.Client()

# テストリクエスト
response = client.models.generate_content(
    model='gemini-2.0-flash',
    contents='こんにちは！'
)

print("--- Gemini APIからの応答 ---")
print(response.text)
print("---------------------------")
```

正常に応答が返ってくれば、セットアップは完了です。

#### トラブルシューティング

| エラー | 原因 | 解決方法 |
|--------|------|----------|
| `GOOGLE_API_KEY`が取得できない | シークレットへのアクセスが許可されていない | 「ノートブックでのアクセス」がオンになっているか確認 |
| `Invalid API key` | APIキーが無効または期限切れ | Google AI Studioで新しいキーを作成し直す |
| `Quota exceeded` | 無料枠の利用制限に達した | 時間をおいてから再試行、または有料プランへのアップグレードを検討 |
| `Module not found` | ライブラリがインストールされていない | `!pip install google-genai` を実行 |

#### 参考リンク

- [Google AI Studio](https://aistudio.google.com/)
- [Gemini API ドキュメント](https://ai.google.dev/gemini-api/docs)

### 2.2 OpenWeatherMap API のキー取得手順
OpenWeatherMapは、世界中の天気情報を提供する無料のAPIサービスです。
**例 2: 天気予報 API（データ取得と処理）**　のセクションで使用します。

#### ステップ 1: アカウントの作成

1. **公式サイトにアクセス**: [OpenWeatherMap](https://openweathermap.org/) にアクセスします。
2. **サインアップ**: 右上の「Sign In」→「Create an Account」をクリックします。
3. **情報入力**: ユーザー名、メールアドレス、パスワードを入力し、利用規約に同意してアカウントを作成します。
4. **メール認証**: 登録したメールアドレスに認証メールが届くので、リンクをクリックしてアカウントを有効化します。

#### ステップ 2: APIキーの取得

1. **ログイン**: OpenWeatherMapにログインします。
2. **APIキーページへ移動**: ダッシュボードから「API keys」タブをクリックします。
3. **デフォルトキーの確認**: アカウント作成時に自動的に生成されたデフォルトのAPIキーが表示されます。
4. **キーのコピー**: 表示されているAPIキー（例: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6`）をコピーします。

> **注意**: APIキーは作成後、有効になるまで数時間かかる場合があります。すぐに使えない場合は少し待ってから試してください。

#### ステップ 3: 環境変数への設定

APIキーをプログラムに直接書き込むのは危険です。環境変数として設定する方法を選択してください。

**Google Colab での設定（推奨）**
```python
from google.colab import userdata
import os

# Colabの秘密鍵機能から取得
api_key = userdata.get('WEATHER_API_KEY')
os.environ['WEATHER_API_KEY'] = api_key
```

Colabの左サイドバーにある鍵アイコンをクリックし、以下を設定します：
- **名前**: `WEATHER_API_KEY`
- **値**: コピーしたAPIキー
- **ノートブックでのアクセス**: オン（✔）

#### ステップ 4: 動作確認

以下のコードを実行して、正しく天気情報が取得できることを確認します。
```python
import requests
import os

API_KEY = os.environ.get("WEATHER_API_KEY")
if not API_KEY:
    print("エラー: APIキーが設定されていません。")
    exit()

url = f"http://api.openweathermap.org/data/2.5/weather?q=Tokyo&appid={API_KEY}&units=metric&lang=ja"
response = requests.get(url)

if response.status_code == 200:
    print("✅ API接続成功！")
    data = response.json()
    print(f"都市: {data['name']}")
    print(f"気温: {data['main']['temp']}℃")
else:
    print(f"❌ エラー: {response.status_code}")
    print(response.text)
```

#### トラブルシューティング

| エラー | 原因 | 解決方法 |
|--------|------|----------|
| `401 Unauthorized` | APIキーが無効または未設定 | キーを正しくコピーしたか確認、数時間待ってから再試行 |
| `429 Too Many Requests` | リクエスト制限超過 | 無料プランは1分間に60回まで。時間をおいて再試行 |
| `404 Not Found` | 都市名が正しくない | 英語の都市名を使用（例: `Tokyo`, `Osaka`） |

#### 参考リンク

- [OpenWeatherMap API ドキュメント](https://openweathermap.org/api)
- [Current Weather Data API](https://openweathermap.org/current)
- [価格プラン](https://openweathermap.org/price)

### 2.3 その他のAPIキー取得
（参考リンク集など）

## 3. 「注文票（リクエストURL）」の書き方
APIへの注文票（リクエスト）は、実はあなたがブラウザでウェブサイトを見るときに入力するURLとよく似た構造をしています。

### 構成要素

注文票（URL）は、主に次の２つの部分でできています。

1. **ベースURL（住所）**: どこに注文を出すかを示す、固定の部分。
   - 例: `https://api.example.com/data/`
2. **パラメータ（質問内容）**: 欲しい情報を具体的に指定する、変化する部分。
   - 例: `city=Tokyo` や `format=json`

### 注文票の組み立て例

これらの要素は、**「?」と「&」**という記号を使って一つのURLにまとめられます。

| 項目 | 内容 | 記号 |
|------|------|------|
| ベースURL (住所) | `https://api.example.com/weather` | |
| パラメータ開始 | 最初の質問が始まる合図 | `?` |
| 質問 1 (都市名) | `city=Tokyo` | |
| 質問の区切り | 次の質問を区切る | `&` |
| 質問 2 (APIキー) | `key=xxxxxxxxx` | |

**【注意】** URLに日本語などの特殊文字（半角英数字と一部記号以外）が含まれる場合、サーバーが正しく認識できるよう**URLエンコード（符号化）**が必要です。プログラミング言語のライブラリ（Pythonのrequestsなど）は通常これを自動で行ってくれます。

### 完成した注文票（URL）

```
https://api.example.com/weather?city=Tokyo&key=xxxxxxxxx
```

このURLをクライアント（プログラム）から送信することで、サーバーに「キーxxxxを使って、東京の天気を教えてください」という注文が届きます。

## 4. 受け取る情報（レスポンス）を覗いてみよう

注文（リクエスト）がサーバーに届くと、コック（サーバー）はデータという料理を作り、**JSON (ジェイソン)**という決まった形式で返してくれます。

### JSONのイメージ

JSONは、返ってきたデータが何なのかを分かりやすくするための「ラベル付きの箱」のイメージです。

| JSON (データ) の例 | 説明 |
|-------------------|------|
| `"city": "Tokyo"` | 「city」というラベルの箱に「Tokyo」というデータが入っている。 |
| `"temp": 22.5` | 「temp (温度)」というラベルの箱に「22.5」というデータが入っている。 |

あなたのプログラムは、このJSONの**「ラベル」**を頼りに、必要なデータ（例: "temp"の箱の中身）を正確に取り出して表示することができます。

## 4.5 Postmanを使ったAPI操作の実践

プログラミングを始める前に、**Postman**という専用ツールを使ってAPIの動作を確認しましょう。Postmanは、コードを書かずにAPIリクエストを送信し、レスポンスを確認できる便利なツールです。

### 4.5.1 Postmanとは

Postmanは、API開発とテストのための無料ツールです。以下のような特徴があります：

- コードを書かずにAPIをテスト可能
- リクエストとレスポンスを視覚的に確認
- リクエストの履歴を保存・再利用
- チームでのAPI仕様の共有が容易

### 4.5.2 Postmanのインストール

#### デスクトップアプリ版（推奨）

1. **公式サイトにアクセス**: [Postman公式サイト](https://www.postman.com/downloads/) にアクセスします。
2. **ダウンロード**: お使いのOS（Windows、Mac、Linux）に対応したバージョンをダウンロードします。
3. **インストール**: ダウンロードしたファイルを実行し、画面の指示に従ってインストールします。
4. **アカウント作成**: 初回起動時にアカウント作成を求められますが、スキップして使い始めることも可能です。

#### Web版

アカウントを作成すれば、ブラウザから [Postman Web](https://web.postman.co/) を利用することもできます。

### 4.5.3 Postmanの基本画面

Postmanを起動すると、以下の主要な要素が表示されます：

| 要素 | 説明 |
|------|------|
| **リクエストメソッド** | GET、POST、PUT、DELETEなどのHTTPメソッドを選択 |
| **URL欄** | APIのエンドポイントを入力 |
| **Params（パラメータ）** | URLパラメータを入力（自動的にURLに追加される） |
| **Headers（ヘッダー）** | 認証情報やコンテンツタイプなどを設定 |
| **Body（本文）** | POSTリクエストなどで送信するデータを入力 |
| **Send（送信）ボタン** | リクエストを送信 |
| **Response（レスポンス）** | サーバーからの応答を表示 |

### 4.5.4 実践例 1：天気予報API（GETリクエスト）

OpenWeatherMap APIを使って、東京の天気情報を取得してみましょう。

#### 手順

1. **新しいリクエストを作成**
   - 「New」→「HTTP Request」をクリック
   - または、中央の「+」タブをクリック

2. **メソッドとURLを設定**
   - リクエストメソッド：`GET` を選択
   - URL欄に以下を入力：
     ```
     http://api.openweathermap.org/data/2.5/weather
     ```

3. **パラメータを設定**
   - 「Params」タブをクリック
   - 以下のパラメータを追加：

   | KEY | VALUE | 説明 |
   |-----|-------|------|
   | `q` | `Tokyo` | 都市名 |
   | `appid` | `YOUR_API_KEY` | あなたのAPIキー（2.2節で取得） |
   | `units` | `metric` | 温度を摂氏で取得 |
   | `lang` | `ja` | 日本語で取得 |

   パラメータを入力すると、URL欄が自動的に以下のように変更されます：
   ```
   http://api.openweathermap.org/data/2.5/weather?q=Tokyo&appid=YOUR_API_KEY&units=metric&lang=ja
   ```

4. **リクエストを送信**
   - 青い「Send」ボタンをクリック

5. **レスポンスを確認**
   - 下部の「Response」セクションに結果が表示されます
   - 「Pretty」タブを選択すると、JSONが見やすく整形されます
   - 「Status: 200 OK」が表示されていれば成功です

**期待されるレスポンス例**：
```json
{
  "coord": {
    "lon": 139.6917,
    "lat": 35.6895
  },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "快晴",
      "icon": "01d"
    }
  ],
  "main": {
    "temp": 22.5,
    "feels_like": 22.1,
    "temp_min": 20.0,
    "temp_max": 25.0,
    "pressure": 1013,
    "humidity": 60
  },
  "name": "Tokyo"
}
```

#### レスポンスの読み方

- `main.temp`: 現在の気温（22.5℃）
- `weather[0].description`: 天気の説明（快晴）
- `name`: 都市名（Tokyo）

### 4.5.5 実践例 2：Gemini API（POSTリクエスト）

Gemini APIを使って、AIに質問を送信してみましょう。

#### 手順

1. **新しいリクエストを作成**
   - 「+」タブをクリックして新しいリクエストを作成

2. **メソッドとURLを設定**
   - リクエストメソッド：`POST` を選択
   - URL欄に以下を入力：
     ```
     https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent
     ```

3. **APIキーをパラメータに追加**
   - 「Params」タブをクリック
   - 以下のパラメータを追加：

   | KEY | VALUE |
   |-----|-------|
   | `key` | `YOUR_GEMINI_API_KEY` |

4. **ヘッダーを設定**
   - 「Headers」タブをクリック
   - 以下のヘッダーを追加：

   | KEY | VALUE |
   |-----|-------|
   | `Content-Type` | `application/json` |

5. **リクエスト本文（Body）を設定**
   - 「Body」タブをクリック
   - 「raw」を選択
   - 右側のドロップダウンで「JSON」を選択
   - 以下のJSONを入力：

   ```json
   {
     "contents": [
       {
         "parts": [
           {
             "text": "Web APIとは何か、小学生にもわかるように簡潔に説明してください。"
           }
         ]
       }
     ]
   }
   ```

6. **リクエストを送信**
   - 「Send」ボタンをクリック

7. **レスポンスを確認**
   - 「Response」セクションでAIの応答を確認
   - `candidates[0].content.parts[0].text` の中に、AIが生成したテキストが含まれています

**期待されるレスポンス例**：
```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "Web APIは、インターネット上で情報をやり取りするための「窓口」のようなものです..."
          }
        ],
        "role": "model"
      }
    }
  ]
}
```

### 4.5.6 リクエストの保存と再利用

作成したリクエストは保存して、後で再利用できます。

1. **リクエストを保存**
   - 「Save」ボタン（または Ctrl+S / Cmd+S）をクリック
   - リクエスト名を入力（例：「Tokyo Weather」「Gemini Test」）
   - コレクション（フォルダのようなもの）を作成または選択
   - 「Save」をクリック

2. **保存したリクエストを開く**
   - 左サイドバーの「Collections」から、保存したリクエストを選択

### 4.5.7 トラブルシューティング

| 問題 | 原因 | 解決方法 |
|------|------|----------|
| `401 Unauthorized` | APIキーが無効または未設定 | APIキーを正しく入力したか確認 |
| `404 Not Found` | エンドポイントのURLが間違っている | URLのスペルミスを確認 |
| `400 Bad Request` | リクエストの形式が間違っている | JSON形式が正しいか確認（閉じカッコなど） |
| レスポンスが表示されない | ネットワークエラー | インターネット接続を確認 |

### 4.5.8 Postmanを使うメリット

- **学習に最適**: コードを書く前に、APIの動作を理解できる
- **デバッグが簡単**: エラーの原因を特定しやすい
- **ドキュメント作成**: APIの使い方をチームで共有できる
- **テスト自動化**: 複雑なテストシナリオを作成・実行可能

> **ヒント**: Postmanで動作確認ができたら、「Code」ボタンをクリックすると、同じリクエストをPythonやJavaScriptなど、様々な言語のコードに自動変換してくれます！

## 5. プログラムからのAPI使用例

実際にプログラムからAPIに注文を送る、実用的な7つの例を紹介します。

### 💻 例 1: Gemini API（AI機能の利用 - 基本版）

汎用的な通信ライブラリを使うことで、他のAPIと同様に、AI機能もシンプルな注文とJSON処理で実行できることを学びます。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | Google Gemini 2.5 Flash | 高性能なAIモデル |
| クライアント | Python (requestsライブラリを想定) | 他のAPIと同じ方法でアクセスする基本構造の理解 |
| 学習のメリット | 質問文を直接URLやデータとして送り、AIの回答をJSONで受け取る、基本構造の理解。 | |

#### 🚀 具体的な手順 (requests想定)

1. **エンドポイント確認**: Gemini APIのAPIアドレスを確認し、リクエストを送信するためのURLを特定します。
2. **キー設定**: APIキーをリクエストのヘッダー（特別な情報欄）に組み込むための設定を行います。
3. **データ準備**: 質問文（プロンプト）やモデルの設定を、JSON形式の注文データとして作成します。
4. **リクエスト送信**: `requests.post()` を使い、ヘッダーと**注文データ（本文）**を添えてサーバーに送信します。
5. **結果抽出**: 返ってきた巨大なJSONデータの中から、`['candidates'][0]['content']['parts'][0]['text']` のように、階層を辿ってAIが生成したテキスト部分を特定し、取り出します。

**Pythonコード例：requestsによるGemini APIアクセス**

```python
import requests
import json
import os  # 環境変数からキーを読み込むために使用

# 1. 準備：APIキーとエンドポイントを定義
# (Colab環境の場合、os.environ['GEMINI_API_KEY']にキーが設定されている前提)
API_KEY = os.environ.get('GEMINI_API_KEY')
if not API_KEY:
    print("エラー: GEMINI_API_KEYが設定されていません。")
    # ここでは実行せずに終了
    exit()

ENDPOINT = "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent"

# 2. キー設定：ヘッダーに認証情報を組み込む
HEADERS = {
    "Content-Type": "application/json"
}
# APIキーはURLのクエリパラメータとして渡す
URL = f"{ENDPOINT}?key={API_KEY}"

# 3. データ準備：質問文をJSON形式の注文データとして作成
PROMPT = "Web APIとは何か、小学生にもわかるように簡潔に説明してください。"

PAYLOAD = {
    "contents": [
        {
            "parts": [
                {
                    "text": PROMPT
                }
            ]
        }
    ]
}

# 4. リクエスト送信：POSTメソッドでヘッダーと注文データ(本文)を添えて送信
print("APIにリクエストを送信中...")
response = requests.post(
    URL,
    headers=HEADERS,
    data=json.dumps(PAYLOAD)  # Pythonの辞書をJSON文字列に変換
)

# 5. 結果抽出：応答の処理
if response.status_code == 200:
    data = response.json()
    
    # 複雑なJSON構造からテキスト部分を抽出
    try:
        generated_text = data['candidates'][0]['content']['parts'][0]['text']
        print("\n--- AIからの応答 ---")
        print(generated_text)
        print("--------------------")
    except (KeyError, IndexError):
        print("\nエラー: JSON構造からテキストを抽出できませんでした。応答を確認してください。")
        # デバッグ用にJSON全体を表示しても良い
        # print(json.dumps(data, indent=2))
else:
    print(f"\nエラーが発生しました。ステータスコード: {response.status_code}")
    print("エラーメッセージ:", response.text)
```

> **コラム**: AIへの質問はURLのパラメータではなく、**リクエストの本文（Body）**に入れて送ることが多いです。これは、質問文が長くなるためです。

### 💻 例 2: 天気予報 API（データ取得と処理）

外部サービスから実用的なデータを取得し、それをプログラムで処理する、最も基礎的で重要な例です。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | OpenWeatherMapなど（公開されているもの） | 実際の天気データを取得できる |
| クライアント | Python (requestsライブラリ) | 最も一般的で学習しやすいWebアクセス方法 |
| 学習のメリット | 注文票（URL）の送信と、返ってきたJSONデータから必要な情報だけを取り出す（JSON処理）練習ができる。 | |

#### 🚀 具体的な手順 (requests利用)

1. **URL組み立て**: エンドポイントに、都市名とAPIキーをパラメータとして追加し、プログラム内で注文票URLを完成させます（例: Pythonのf-stringなど）。
2. **リクエスト送信**: `requests.get()` を使い、完成したURLでサーバーにアクセスします。
3. **データ解析**: 返ってきたJSONデータを読み込みます。データ内の「main」というラベルの中にある「temp」というラベルを探すため、`data['main']['temp']` のようにラベルを辿ってデータを抽出します。
4. **表示**: 抽出した温度（例: 285.15 K）を、人間が読める摂氏温度（℃）に計算（-273.15）して表示します。

**Pythonコード例：requestsによる天気予報 APIアクセス**

```python
import requests
import json
import os

# 仮のAPIキーとエンドポイントを設定
# 実際にはOpenWeatherMapなどのAPIキーとエンドポイントを使用してください
API_KEY_WEATHER = "YOUR_WEATHER_API_KEY"  # 実際にはここにキーを設定
CITY_NAME = "Tokyo"
URL_BASE = "http://api.openweathermap.org/data/2.5/weather"

# 1. URL組み立て: パラメータを辞書で定義
params = {
    'q': CITY_NAME,
    'appid': API_KEY_WEATHER,
    'units': 'metric',  # 温度を摂氏(℃)で取得するよう指定 (APIによって異なります)
    'lang': 'ja'        # 日本語で取得するよう指定
}

print(f"APIにリクエストを送信中... (都市: {CITY_NAME})")

# 2. リクエスト送信: requests.get() を使ってURLとパラメータを送信
# requestsライブラリがパラメータを自動でURLに組み込んでくれます
try:
    response = requests.get(URL_BASE, params=params)
    response.raise_for_status()  # HTTPエラーが発生した場合に例外を発生させる

    # 3. データ解析: JSONデータを読み込む
    data = response.json()

    # 4. データ抽出と表示: JSONデータから必要な情報を取り出す
    temp_celsius = data['main']['temp']  # main > temp のラベルを辿る
    weather_desc = data['weather'][0]['description']  # weather (リストの0番目) > description を辿る

    print("\n--- 天気予報 APIからの応答 ---")
    print(f"都市: {data['name']}")
    print(f"天気: {weather_desc}")
    print(f"現在の気温: {temp_celsius:.1f} ℃")
    print("------------------------------")

except requests.exceptions.RequestException as e:
    print(f"\nエラー: APIアクセス中に問題が発生しました。キーやURLを確認してください。")
    print(f"詳細: {e}")
except KeyError:
    print("\nエラー: 応答JSONの形式が想定と異なります。データ構造を確認してください。")
```

> **コラム**: 天気予報などの情報取得は、リクエストの本文を使わないシンプルな GETメソッドを使うことが多いです。

### 💻 例 3: 地図・位置情報 API（視覚的データの利用）

地図サービス（Google Maps Platformなど）のAPIを利用し、位置情報やルート情報をプログラムに取り込みます。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | 地図サービス提供元 | 緯度・経度から住所への変換などが可能 |
| クライアント | JavaScript / Python | Webアプリ制作に欠かせない要素 |
| 学習のメリット | 座標データが地図上の視覚的な情報に変わるプロセスを体験できる。 | |

#### 🚀 具体的な手順 (JavaScript想定)

1. **HTML準備**: 地図を表示させたい場所に、特定のIDを持った空の`<div>`（例: `<div id="map"></div>`）を配置します。
2. **ライブラリ読み込み**: 地図表示用のJavaScriptライブラリを、HTMLの`<script>`タグで読み込みます。
3. **地図初期化**: JavaScriptコード内で、APIキーと、地図の中心となる緯度・経度を指定して、地図を`id="map"`の要素に描画する命令を実行します。
4. **マーク設定**: APIの関数を使って、特定の場所を示す**ピン（マーカー）**を緯度・経度に合わせて設置する命令を追加します。

> **コラム**: 地図APIの多くは、ウェブブラウザ上で動かすJavaScriptコードから直接呼び出し、地図を描画します。

### 💻 例 4: 通貨換算 API（リアルタイムデータの利用）

リアルタイムで変動する数値データを取得し、その処理を練習します。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | ExchangeRate-APIなど | 最新の為替レートを取得できる |
| クライアント | Python / JavaScript | データのリアルタイム性を体験できる |
| 学習のメリット | 常に変化する外部データを取得し、プログラム内で計算・整形する練習ができる。 | |

#### 🚀 具体的な手順 (requests利用)

1. **URL作成**: 基準となる通貨（例: USD）を指定したエンドポイントと、APIキーでURLを決定します。
2. **リクエスト送信**: requestsでAPIにアクセスし、最新のレート情報を含むJSONデータを受け取ります。
3. **データ抽出**: JSON内の「rates」という辞書構造（ラベルの集まり）から、「JPY」（日本円）の数値を取り出します。
4. **計算・表示**: 取り出した数値（例: 155.20）を使って、ユーザーが入力した金額（例: 100ドル）を掛け算し、結果を小数点第2位まで丸めて表示します。

> **コラム**: 金融系のAPIは、無料版だと一日の利用回数や取得できるデータの鮮度（リアルタイムかどうか）に厳しい制限があることが多いです。

### 💻 例 5: 画像認識・分析 API（データの「理解」）

単に数値やテキストを取得するだけでなく、画像という非定型なデータをAPIに渡して分析させる例です。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | Google Cloud Vision API など | 画像の内容（何が写っているか、テキスト、感情など）を解析する |
| クライアント | Python / JavaScript | 非テキストデータの処理を学ぶ良い機会 |
| 学習のメリット | プログラムが「見る」ことをAPIが代行している仕組みを理解できる。 | |

#### 🚀 具体的な手順 (Python想定)

1. **画像準備**: プログラムが読み込める場所に画像ファイル（例: cat.jpg）を用意します。
2. **データ変換**: Pythonの標準機能を使って、画像ファイルをBase64という特殊なテキストデータ（バイナリデータをテキスト化したもの）に変換します。
3. **リクエスト送信**: 変換したBase64テキストデータを、解析を依頼するJSONのリクエスト本文に組み込んで送信します。
4. **解析結果抽出**: 返ってきたJSONデータの中から、`['labelAnnotations']`といった特定のラベルを探し、画像に写っていた**オブジェクト名（例: "Cat"）と信頼度（例: 95%）**を取り出します。

> **コラム**: 画像などの大きなデータを送る場合、URLに収まらないため、リクエスト本文 (Body) を使い、データ形式をBase64に変換する必要があります。

### 💻 例 6: 公共データ API（社会的なデータ活用）

政府や自治体が提供するオープンデータを利用する例です。これは、APIが社会的な情報インフラとしても機能していることを示しています。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | e-Stat（政府統計の総合窓口）など | 統計データ、行政情報、災害情報などを取得 |
| クライアント | Python / JavaScript | 信頼性の高い大規模なデータセットを扱う練習になる |
| 学習のメリット | 公共性の高いデータを取得し、それを基にアプリを作れるという、APIの社会貢献性や応用力を感じられる。 | |

#### 🚀 具体的な手順 (requests利用)

1. **データID特定**: 欲しい統計（例: 「都道府県別の人口」）のデータIDを、APIのドキュメントで調べます。
2. **URL組み立て**: エンドポイントに、データIDとAPIキーをパラメータとして指定した注文票URLを組み立てます。
3. **リクエスト送信**: APIにアクセスし、複雑で階層の深いJSONデータを受け取ります。
4. **フィルタリング**: 受け取ったJSONデータの中から、`['GET_STATS']['STATISTICAL_DATA'][0]['DATA_INF']` のように、必要なデータが格納されている階層を正確に辿って情報を取り出します。

> **コラム**: 公共データは非常にデータ量が大きいため、必要な情報だけを効率よく取り出すための高度なJSON処理のスキルが求められます。

### 💻 例 7: Gemini API（AI機能の利用 - SDK応用版）

公式SDKを利用することで、汎用的なrequestsを使うよりも簡単に、認証や複雑な機能（会話の継続など）を実行できることを示します。

| 項目 | 内容 | 特徴 |
|------|------|------|
| API | Google Gemini 2.5 Flash | 公式SDKを利用 |
| クライアント | Python (Colab環境 / 公式SDK) | requestsよりもずっと簡単に複雑な処理が可能 |
| 学習のメリット | SDKの便利さ（認証処理の自動化、機能の使いやすさ）を知り、API選びの視点を身につける。 | |

#### 🚀 具体的な手順 (SDK利用)

1. **ライブラリインストール**: Colabのセルで、`!pip install google-genai` を実行し、必要なライブラリをインストールします。
2. **クライアント初期化**: APIキーが環境変数に設定されている前提で、SDKのクライアントオブジェクトを初期化します。
3. **リクエスト送信**: SDK独自のシンプルなメソッドを呼び出し、AIに質問を送ります。
4. **結果表示**: 返ってきた結果オブジェクトから、textプロパティを使って回答を抽出します。

**Pythonコード例：SDKによるGemini APIアクセス**

```python
# 1. ライブラリインストール（Colabで実行）
# !pip install google-genai

# 2. 認証情報の設定 (Colabでの秘密鍵読み込み後を想定)
import os
from google import genai

# 3. クライアント初期化
# 環境変数 'GEMINI_API_KEY' が設定されているため、キーの指定は不要
try:
    client = genai.Client()
except Exception as e:
    print(f"エラー: クライアント初期化に失敗しました。キーが設定されているか確認してください。")
    exit()

# 4. リクエスト送信：SDK独自のシンプルなメソッド
PROMPT = "Web APIのSDKを使うメリットを箇条書きで３つ教えてください。"
print(f"質問: {PROMPT}\n")

# models.generate_content メソッドで簡単にAIに質問を送信
response = client.models.generate_content(
    model='gemini-2.5-flash',
    contents=PROMPT
)

# 5. 結果表示：response.text を使うだけで簡単に回答全体を取得
print("--- AIからの応答 (SDK利用) ---")
print(response.text)
print("------------------------------")

# 4. 応用（チャット機能の利用）：会話の履歴を保持
# chat = client.chats.create(model='gemini-2.5-flash')
# print("\nチャットを開始しました。")
# response = chat.send_message("私はPythonを学習中です。")
# print(f"AI: {response.text}")
# response = chat.send_message("次に何を学ぶべきですか？")  # 以前の会話を記憶している
# print(f"AI: {response.text}")
```

> **コラム**: 多くの企業が、利用者が使いやすいようにこのような**SDK（開発キット）**を提供しています。複雑なAPIを使う場合は、SDKの利用が断然おすすめです。