<p align="center">
  <a href="https://www.teloa.ai/en/"><img src="assets/brand/teloa-reading.webp" width="120" height="120" alt="本を読むリスの Teloa"></a>
</p>

# AI Worker Standard 1.0

**AI Worker、AI Team、AI Organization のためのオープンな定義標準。**

**Max Luo、Morgan Chen｜Teloa 共同創業者** · 2026 年 10 月 18 日

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

[Website](https://www.teloa.ai/en/) · [Docs](https://docs.teloa.ai/en/) · [Marketplace](https://market.teloa.ai/en/) · [AI Security](https://security.teloa.ai/?lang=en) · [Blog](https://www.teloa.ai/en/blog/) · [Security reports](https://security.teloa.ai/report/?lang=en)

[![Version 1.0](assets/badges/version.svg)](https://github.com/teloa-ai/ai-worker-standard/releases/tag/v1.0) [![8 core rules](assets/badges/core-rules.svg)](spec/core.ja.md) [![8 languages](assets/badges/languages.svg)](docs/languages.md) [![License Apache-2.0](assets/badges/license.svg)](LICENSE)

**AI Worker** は、タスクをまたいで維持される識別可能な主体として、明確な責務と権限の範囲内で働き、検収可能な成果を提供します。Teloa での製品名は **AI 従業員（AI employee）** です。AI Worker はその背後にある共通の標準概念です。Grok Bot も、以下の対応付けの確認を前提に、この概念で記述できます。

## 1. 基本概念

| 概念 | 記述するもの | 目的 |
| --- | --- | --- |
| AI Worker | 識別子、責務、作業契約、成果の確認条件を持つ作業主体 | タスク、モデル、一時的な Agent が変わっても作業の帰属を追跡する |
| AI Team | 共通の成果に向けた役割分担、引き継ぎ、検収を定める構成 | 協業の成果を契約に基づいて確認する |
| AI Organization | 組織としての責任、資源の境界、変更管理を定める Worker と Team の構成 | 明示的なガバナンスの下でチームを連携させる |

識別の継続性は、専用マシン、常駐プロセス、長期記憶を必要としません。責任とは作業の帰属と追跡可能性であり、法人格を意味しません。グループチャットやメンバー一覧だけでは Team 契約になりません。

## 2. 対象範囲と形式

| 項目 | バージョン 1.0 |
| --- | --- |
| コアオブジェクト | `agent` と `organization` |
| 必須の作業プロファイル | `urn:teloa:profile:ai-work`、バージョン `1.0` |
| Worker／Team／Organization | Worker：コア＋作業プロファイル。Team：協業契約を追加。Organization：さらにガバナンス契約を追加 |
| 交換形式 | JSON。制限付き変換規則に従う YAML による記述も可能 |
| コアの確認項目 | 共通規則 R01–R08 の 8 項目、コア交換ケース T01–T06 の 6 項目 |
| 解釈の基準 | 簡体字中国語のコア仕様と作業プロファイル。他の 7 言語に全文の閲覧用翻訳を提供 |

標準は識別子、不変のリビジョン、参照、責務、作業インターフェース、協業、ガバナンスを定義します。モデル実行、スケジューリング、接続、認可はホストが実装します。定義のインポートはツールのインストール、アカウント接続、作業開始、定例作業の有効化を行いません。6 個のコアフィールドだけを持つ `agent` 文書は、完全な AI Worker 定義ではありません。

## 3. 読み方

| 文書 | 内容 |
| --- | --- |
| [図と検索可能な本文によるガイド](docs/guide.ja.md) | 構成、技術アーキテクチャ、成果の提供フロー、具体例 |
| [コア仕様の全文](spec/core.ja.md) | フィールド、固定リビジョン、参照、共通規則、交換ケース |
| [作業・協業プロファイルの全文](spec/work-profile.ja.md) | 作業契約、資源、引き継ぎ、定例作業、インスタンスのバインド、ガバナンス |
| [サンプル](examples/README.md) | 完全な Worker JSON/YAML、構成サンプル、入出力 |
| [製品・外部仕様との対応](docs/mappings.md) | Teloa、Grok Bot、OASF、Oracle Agent Spec |
| [適合性](CONFORMANCE.md) · [維持管理](GOVERNANCE.md) | 証拠の範囲、貢献方法、管理 |
| [8 言語の索引](docs/languages.md) | 全文の閲覧版と翻訳方針 |

各ガイドは図の内容を本文でも説明します。翻訳ではフィールド名、規則番号、JSON サンプルを保持しています。母語話者の分野専門家による全面的なレビューは未実施です。解釈が異なる場合は、簡体字中国語の 2 仕様を基準とします。共用の対応表、サンプル、管理文書は、言語索引に示す英語または中国語で提供します。

## 4. 完全な Worker から始める

1. [構成と成果提供のガイド](docs/guide.ja.md)を読みます。
2. [document-worker.json](examples/document-worker.json) または同値の [YAML](examples/document-worker.yaml) を確認します。`material` を受け取り、`summary` と `openQuestions` を返します。
3. 責務、入出力スキーマ、必要な資源、検収契約を確認し、[入力例](examples/document-input.json)と[出力例](examples/document-output.json)を比較します。
4. 実行するには、ホスト上で定義を実インスタンス、対応する資源、有効な権限に結び付けます。出力、検収、業務での採用は別々に記録します。

サンプルは契約の例であり、デプロイ用パッケージや適合性検証ツールではありません。構成サンプルには未解決のバインドが含まれ、実運用の準備はできていません。

## 5. 製品名と外部仕様

Teloa の利用者向けの製品概念は **AI 従業員**です。Worker 定義が役割と作業契約を記述し、Worker インスタンスが特定の AI 従業員を識別します。AI チームは Team 契約で協業と成果提供を組織します。

Grok の **Bot** は AI Worker に対応付けられます。複数人が共有する **Team Bot** は、まず共有 Worker として扱い、複数 Worker のチームとは自動的に見なしません。適合性は名前ではなく、定義とホストの動作で決まります。出典と確認項目は[対応表](docs/mappings.md)を参照してください。

OASF からは発見用メタデータと分類体系、Oracle Agent Spec からはコンポーネント参照、インターフェース、構成の考え方を取り入れています。形式やランタイムの完全互換性を意味しません。

## 6. 位置付け、貢献、引用

本標準は **Teloa が維持するオープン仕様**です。採択済みの国際標準や認証制度ではありません。xAI、AGNTCY、Oracle、標準化団体による承認を主張しません。独立した相互運用性には、独立実装による再現可能な証拠が必要です。

具体的な曖昧さ、反例、バージョンを明記した実装報告は [Issues](https://github.com/teloa-ai/ai-worker-standard/issues) に提出してください。変更は[維持管理](GOVERNANCE.md)、主張できる範囲は[適合性](CONFORMANCE.md)を参照してください。

引用例：**Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** 機械可読の引用情報：[CITATION.cff](CITATION.cff)。

## 7. ライセンス

本リポジトリの独自の内容は [Apache-2.0](LICENSE) で提供します。製品名と外部仕様の権利は各権利者に帰属します。閲覧・実装に Teloa のアカウントやサービスは不要です。

Teloa の名称とリスのイラストには[別途ブランド使用規則](assets/brand/README.md)が適用されます。
