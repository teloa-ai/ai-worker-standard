<p align="center">
  <a href="https://www.teloa.ai/en/"><img src="assets/brand/teloa-reading.webp" width="120" height="120" alt="閱讀中的 Teloa 小松鼠"></a>
</p>

# AI Worker 開放定義標準 1.0

**以一套工作契約描述 AI Worker、AI 團隊與 AI 組織。**

**Max Luo、Morgan Chen｜Teloa 共同創辦人** · 2026 年 10 月 18 日

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

[Website](https://www.teloa.ai/en/) · [Docs](https://docs.teloa.ai/en/) · [Marketplace](https://market.teloa.ai/en/) · [AI Security](https://security.teloa.ai/?lang=en) · [Blog](https://www.teloa.ai/en/blog/) · [Security reports](https://security.teloa.ai/report/?lang=en)

[![Version 1.0](assets/badges/version.svg)](https://github.com/teloa-ai/ai-worker-standard/releases/tag/v1.0) [![8 core rules](assets/badges/core-rules.svg)](spec/core.zh-Hant.md) [![8 languages](assets/badges/languages.svg)](docs/languages.md) [![License Apache-2.0](assets/badges/license.svg)](LICENSE)

**AI Worker** 是身分與職責能跨任務延續、在授權範圍內工作，並交付可驗收成果的 AI 工作個體。在 Teloa 中，產品名稱是 **AI 員工**；AI Worker 是背後的通用標準概念。Grok Bot 亦可依此概念描述，實際適配須核對下文的映射要求。

## 1. 核心概念

| 概念 | 描述內容 | 解決的問題 |
| --- | --- | --- |
| AI Worker | 具有明確身分、職責、工作契約與交付檢查的工作個體 | 跨任務、模型與臨時 Agent 仍可追溯工作歸屬 |
| AI Team | 圍繞共同成果分工協作，明確交接與驗收的組合 | 讓協作具有可檢查的交付契約 |
| AI Organization | 具備組織責任、資源邊界與變更治理的個體與團隊組合 | 讓團隊依明確治理規則共同工作 |

持續身分不要求獨佔機器、永久程序或長期記憶。「責任」指工作歸屬與可追溯性，不代表法律人格。群組聊天或成員名單本身並非團隊契約。

## 2. 範圍與格式

| 項目 | 1.0 的約定 |
| --- | --- |
| 核心物件 | `agent` 與 `organization` |
| 必要工作應用規範 | `urn:teloa:profile:ai-work`，版本 `1.0` |
| Worker／Team／Organization | Worker：核心＋工作應用規範；Team：加入協作契約；Organization：再加治理契約 |
| 交換格式 | JSON；可依受限轉換規則以 YAML 撰寫 |
| 核心檢查 | 八項通用規則 R01–R08；六項核心交換案例 T01–T06 |
| 解釋依據 | 簡體中文核心規範與工作應用規範；另有七種語言的完整閱讀譯本 |

標準定義身分、不可變修訂、參照、職責、工作介面、協作與治理。模型執行、排程、連線及授權由宿主實作。匯入定義不會安裝工具、連接帳號、開始工作或啟用例行任務。只有六個核心欄位的 `agent` 檔案，尚非完整 AI Worker 定義。

## 3. 閱讀入口

| 文件 | 內容 |
| --- | --- |
| [圖解與可搜尋正文](docs/guide.zh-Hant.md) | 組成、技術架構、交付流程與完整範例 |
| [核心規範全文](spec/core.zh-Hant.md) | 欄位、固定修訂、參照、通用規則與交換案例 |
| [工作與協作應用規範全文](spec/work-profile.zh-Hant.md) | 工作契約、資源、交接、例行、實例綁定與治理 |
| [範例](examples/README.md) | 完整 Worker JSON/YAML、組合定義與輸入輸出 |
| [產品與外部規範映射](docs/mappings.md) | Teloa、Grok Bot、OASF 與 Oracle Agent Spec |
| [符合性範圍](CONFORMANCE.md) · [維護規則](GOVERNANCE.md) | 證據邊界、參與方式與維護 |
| [八語言閱讀索引](docs/languages.md) | 完整閱讀版本與翻譯規則 |

每份指南均以正文解釋配圖。譯本保留欄位名稱、規則編號與 JSON 範例，尚未經母語領域專家完整複核；解釋有分歧時，以兩份簡體中文規範為準。共用的映射、範例及維護文件使用英文及／或中文，索引逐項標示。

## 4. 從一個完整 Worker 開始

1. 閱讀[組成與交付指南](docs/guide.zh-Hant.md)。
2. 查看 [document-worker.json](examples/document-worker.json) 或等價的 [YAML](examples/document-worker.yaml)。它接收 `material`，傳回 `summary` 與 `openQuestions`。
3. 檢查職責、輸入輸出約束、必要資源與驗收契約，對照[範例輸入](examples/document-input.json)及[範例輸出](examples/document-output.json)。
4. 如需執行，在宿主中綁定實際實例、受支援資源與有效授權；分別記錄成果、驗收與業務採用。

這些檔案是契約範例，並非部署套件或符合性驗證器。組合範例包含尚未解決的綁定，不能直接進行實際執行。

## 5. 產品名稱與外部規範

Teloa 面向使用者的產品概念始終是 **AI 員工**。Worker 定義描述職務與工作契約，Worker 實例識別一位具體 AI 員工；AI 團隊透過 Team 契約組織協作交付。

Grok 的 **Bot** 可映射為 AI Worker。多人共用的 **Team Bot** 先視為共享 Worker，不自動等同多個 Worker 組成的團隊。是否符合取決於定義與宿主行為，而非名稱。來源與核對項目見[映射說明](docs/mappings.md)。

本規範吸收 OASF 的探索分類，以及 Oracle Agent Spec 的元件參照、介面與組合思路；不因此宣稱格式或執行階段全面相容。

## 6. 地位、參與與引用

這是 **Teloa 維護的開放規範**，尚未取得國際標準地位，也不提供認證。不聲稱獲得 xAI、AGNTCY、Oracle 或標準組織背書。獨立互通須由獨立實作提供可重現證據。

歡迎透過 [Issues](https://github.com/teloa-ai/ai-worker-standard/issues) 提交具體歧義、反例與附版本的實作報告。變更見[維護規則](GOVERNANCE.md)，聲明範圍見[符合性範圍](CONFORMANCE.md)。

建議引用：**Max Luo、Morgan Chen. AI Worker 開放定義標準 1.0. Teloa, 2026.** 機器可讀引用：[CITATION.cff](CITATION.cff)。

## 7. 授權條款

原創內容採用 [Apache-2.0](LICENSE) 授權。產品名稱與第三方規範保留各自權利。閱讀或實作本規範不需要 Teloa 帳號或服務。

Teloa 名稱與小松鼠素材遵循[獨立品牌使用規則](assets/brand/README.md)。
