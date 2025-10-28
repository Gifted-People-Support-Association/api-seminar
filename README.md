# API プチゼミ - Web APIの基礎から実践まで

このリポジトリは、Web APIの基本概念から実際の使用方法まで、初心者にもわかりやすく学べる教材セットです。Google Gemini APIやOpenWeatherMap APIを実際に使いながら、APIの仕組みを体験的に学習できます。

## 📚 目次

- [概要](#概要)
- [学習内容](#学習内容)
- [環境構築](#環境構築)
- [セクション構成](#セクション構成)
- [使用するAPI](#使用するapi)
- [必要なもの](#必要なもの)

## 🎯 概要

このゼミでは、以下のトピックを扱います：

- **APIとは何か**：ソフトウェア間の通信の仕組みを理解する
- **REST API**：現代のWeb APIで最も使われている設計手法を学ぶ
- **実践的な使用例**：実際のAPIサービスを使ってデータを取得・処理する

## 📖 学習内容

### Section 01: APIとRESTの基本（導入編）

初めての方でも理解できるよう、APIの概念を日常の例え（レストランでの注文）を使って説明します。

**主な内容：**
- APIの基本概念と役割
- REST APIの仕組み（HTTPメソッド、エンドポイント、リソース）
- 実用例：Google Maps API、ChatGPT API
- APIを使うメリットと社会的な重要性

**キーワード：**
- アプリケーション・プログラミング・インターフェース
- RESTful API
- HTTPメソッド（GET, POST, PUT, DELETE）
- エンドポイント

### Section 02: APIを使ってみよう（実践編）

実際にAPIキーを取得し、PythonやPostmanを使ってAPIにリクエストを送信する実践的な内容です。

**主な内容：**
1. **APIを動かすための3つの道具**
   - APIキー（認証）
   - エンドポイント（アクセス先のURL）
   - クライアント（リクエストを送る道具）

2. **APIキーの取得と設定**
   - Gemini API（Google AI Studio）
   - OpenWeatherMap API
   - Google Colabでの安全な管理方法

3. **実践的なAPI使用例（7種類）**
   - 例1: Gemini API（requestsライブラリを使った基本版）
   - 例2: 天気予報API（データ取得と処理）
   - 例3: 地図・位置情報API（視覚的データの利用）
   - 例4: 通貨換算API（リアルタイムデータ）
   - 例5: 画像認識API（非テキストデータの処理）
   - 例6: 公共データAPI（社会的なデータ活用）
   - 例7: Gemini API（公式SDKを使った応用版）

**キーワード：**
- APIキー、環境変数、シークレット管理
- リクエスト/レスポンス
- JSON形式のデータ処理
- Python requestsライブラリ
- 公式SDK（Software Development Kit）

## 🛠 環境構築

### 前提条件

- Python 3.13以上
- uvパッケージマネージャー（推奨）またはpip

### セットアップ手順

1. **リポジトリのクローン**
```bash
git clone <repository-url>
cd api-seminar
```

2. **仮想環境の作成とアクティベート**

**uvを使う場合（推奨）：**
```bash
uv venv
source .venv/bin/activate  # Linux/Mac
# または
.venv\Scripts\activate  # Windows
```

**標準のvenvを使う場合：**
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# または
.venv\Scripts\activate  # Windows
```

3. **依存パッケージのインストール**

**uvを使う場合：**
```bash
uv pip install -e .
```

**pipを使う場合：**
```bash
pip install -e .
```

4. **環境変数の設定**

`.env`ファイルをプロジェクトルートに作成し、APIキーを設定します：

```bash
# .env
GOOGLE_API_KEY=your_google_api_key_here
WEATHER_API_KEY=your_openweathermap_api_key_here
```

> **注意**: `.env`ファイルはGitにコミットしないでください。このファイルは`.gitignore`に含まれています。

## 📂 セクション構成

```
api-seminar/
├── section01-introduction/     # APIとRESTの基本概念
│   └── README.md              # 導入編の説明資料
├── section02-practice/         # 実践的なAPI使用
│   ├── README.md              # 実践編の説明資料
│   └── notebook.ipynb         # Google Colab用ハンズオン教材
├── pyproject.toml             # プロジェクト設定と依存関係
├── .env                       # 環境変数（APIキー）※作成が必要
├── .gitignore                 # Git管理対象外ファイル
└── README.md                  # このファイル
```

## 🔑 使用するAPI

### 1. Google Gemini API
- **用途**: AI生成テキスト、チャット機能
- **取得先**: [Google AI Studio](https://aistudio.google.com/apikey)
- **無料枠**: あり（リクエスト数制限あり）

### 2. OpenWeatherMap API
- **用途**: 世界中の天気情報取得
- **取得先**: [OpenWeatherMap](https://openweathermap.org/)
- **無料枠**: 1分間に60リクエストまで

## 📦 必要なもの

このプロジェクトで使用するPythonパッケージ：

| パッケージ | バージョン | 用途 |
|----------|----------|------|
| `google-genai` | ≥1.44.0 | Gemini API公式SDK |
| `requests` | ≥2.32.5 | HTTP通信ライブラリ（汎用的なAPI呼び出し） |
| `python-dotenv` | ≥1.1.1 | 環境変数管理 |
| `ipykernel` | ≥7.0.1 | Jupyter Notebook対応 |

## 🚀 使い方

### ローカル環境で学習する場合

1. 各セクションのREADME.mdを順番に読む
2. `section02-practice/notebook.ipynb`をJupyter NotebookまたはVS Codeで開く
3. セルを順番に実行しながら学習する

### Google Colabで学習する場合（推奨）

1. **ノートブックを開く**
   - [このリンク](https://colab.research.google.com/github/Gifted-People-Support-Association/api-seminar/blob/main/section02-practice/notebook.ipynb)をクリックしてGoogle Colabで開く
   - または、`section02-practice/notebook.ipynb`をGoogle Colabにアップロード

2. **APIキーを設定する**
   - 左サイドバーの🔑アイコン（Secrets）をクリック
   - 以下の2つのシークレットを追加：
     - 名前: `GOOGLE_API_KEY` / 値: あなたのGemini APIキー
     - 名前: `WEATHER_API_KEY` / 値: あなたのOpenWeatherMap APIキー
   - 各シークレットの「ノートブックでのアクセス」をオン（✔）にする

3. **セルを順番に実行しながら学習する**
   - 各セルの実行ボタン（▶）をクリック、または `Shift + Enter` で実行

## 🎓 学習の進め方

1. **Section 01を読む**（15-20分）
   - APIとRESTの基本概念を理解する
   - 実用例を通じてAPIの重要性を認識する

2. **APIキーを取得する**（10-15分）
   - Google AI StudioでGemini APIキーを取得
   - OpenWeatherMapでアカウント作成とAPIキー取得

3. **Section 02でハンズオン**（60-90分）
   - notebook.ipynbを開く
   - 実際にコードを実行してAPIの動作を確認
   - 7つの実用例を試してみる

4. **応用にチャレンジ**（自由時間）
   - 他のAPIを探して試してみる
   - 自分のプロジェクトにAPIを組み込む

## APIプチゼミ演習問題

このゼミで学んだ知識を確認するための選択式演習問題（全12問）を用意しています。各問題には詳しい解説が付いており、理解度を深めることができます。

**出題範囲：**
- APIの基本概念とその役割
- HTTPメソッド（GET、POST、PUT、DELETE）の使い分け
- エンドポイント、APIキー、パラメータなどの用語理解
- JSON形式のデータ構造とアクセス方法
- URLパラメータの記法（`?`と`&`の使い方）
- GET/POSTリクエストの違い（データの格納場所）
- SDK（公式ライブラリ）を使うメリット
- RESTful APIの設計原則

**演習問題へのアクセス：**
https://gemini.google.com/share/64897ecccbae

## 💡 Tips

- **エラーが出た場合**：各セクションのトラブルシューティングセクションを参照
- **APIキーの管理**：絶対に公開リポジトリにアップロードしない
- **無料枠の制限**：各APIサービスの利用制限を確認してから使用
- **SDK vs requests**：複雑なAPIは公式SDKの使用を推奨

## 🔗 参考リンク

### 公式ドキュメント
- [Google Gemini API ドキュメント](https://ai.google.dev/gemini-api/docs)
- [OpenWeatherMap API ドキュメント](https://openweathermap.org/api)
- [Python requests ライブラリ](https://requests.readthedocs.io/)

### 学習リソース
- [RESTful APIとは？（MDN Web Docs）](https://developer.mozilla.org/ja/docs/Glossary/REST)
- [HTTPメソッドの基礎](https://developer.mozilla.org/ja/docs/Web/HTTP/Methods)

## 📝 ライセンス

このプロジェクトは[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)の下で公開されています。

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: https://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png

詳細は[LICENSE](LICENSE)ファイルをご覧ください。

## 🤝 貢献

質問や改善提案がある場合は、Issueを作成してください。

---

**Happy Learning!** 🎉
