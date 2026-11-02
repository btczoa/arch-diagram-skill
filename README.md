# architecture-diagram-skill

`architecture-diagram` スキルを別 PC やチーム環境に `npx skills add` で 1 コマンド導入するための配布用 Git リポジトリです。

---

## 概要

システム構成図、インフラ配置図、クラウドアーキテクチャ図、セキュリティ境界図、ネットワーク構成図を、プロフェッショナルなライトモードの単一 HTML（インライン SVG）として生成するスキルです。

* **Light Mode Editorial**: スレート基調の清潔感あるライトモードテーマ。
* **Pure SVG & CSS**: JavaScript 不要、単一 HTML ファイルとして即座にブラウザで表示可能。
* **Semantic Components**: フロントエンド、バックエンド、DB、メッセージキュー、セキュリティ境界などを識別しやすい統一パレットで描画。

---

## インストール方法

### 1. GitHub リポジトリから直接インストール
```bash
npx skills add btczoa/architecture-diagram-skill
```

または明示的にスキル名を指定する場合：
```bash
npx skills add btczoa/architecture-diagram-skill --skill architecture-diagram
```

### 2. ローカル環境からの検証・追加
```bash
npx skills add ./path/to/architecture-diagram-skill
```

---

## インストール後の確認

```bash
# スキル一覧の確認
npx skills list

# スキルの実行確認
npx skills run architecture-diagram
```

---

## ディレクトリ構成

```text
architecture-diagram-skill/
├── README.md
├── skill.json                      # スキル定義マニフェスト
├── SKILL.md                        # ルート定義
└── skills/
    └── architecture-diagram/       # スキル本体
        ├── SKILL.md                # スキル仕様・プロンプト定義
        └── assets/
            └── template.html       # アーキテクチャ図用ベーステンプレート
```
