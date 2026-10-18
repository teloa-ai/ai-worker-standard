# AI Worker Standard 1.0 — Languages

**Max Luo and Morgan Chen, co-founders of Teloa**

The repository opens in **English**. Every language below has its own introduction, complete core specification, complete work and collaboration profile, and guide with text explanations. The Simplified Chinese core and profile are the normative source; the other seven languages are complete reading translations, not separate specifications.

| Language | Introduction | Core specification | Work and collaboration profile | Guide |
| --- | --- | --- | --- | --- |
| English | [README](../README.md) | [Full text](../spec/core.en.md) | [Full text](../spec/work-profile.en.md) | [Text and diagrams](guide.en.md) |
| 简体中文 | [介绍](../README.zh-CN.md) | [规范全文](../spec/core.zh-CN.md) | [规范全文](../spec/work-profile.zh-CN.md) | [图文指南](guide.zh-CN.md) |
| 繁體中文 | [介紹](../README.zh-Hant.md) | [全文譯本](../spec/core.zh-Hant.md) | [全文譯本](../spec/work-profile.zh-Hant.md) | [圖文指南](guide.zh-Hant.md) |
| 日本語 | [紹介](../README.ja.md) | [全文](../spec/core.ja.md) | [全文](../spec/work-profile.ja.md) | [図と本文](guide.ja.md) |
| 한국어 | [소개](../README.ko.md) | [전문](../spec/core.ko.md) | [전문](../spec/work-profile.ko.md) | [그림과 본문](guide.ko.md) |
| Deutsch | [Einführung](../README.de.md) | [Volltext](../spec/core.de.md) | [Volltext](../spec/work-profile.de.md) | [Text und Diagramme](guide.de.md) |
| Français | [Présentation](../README.fr.md) | [Texte intégral](../spec/core.fr.md) | [Texte intégral](../spec/work-profile.fr.md) | [Texte et schémas](guide.fr.md) |
| Español | [Introducción](../README.es.md) | [Texto completo](../spec/core.es.md) | [Texto completo](../spec/work-profile.es.md) | [Texto y diagramas](guide.es.md) |

## 1. Translation and interpretation

- The two normative documents are [core.zh-CN.md](../spec/core.zh-CN.md) and [work-profile.zh-CN.md](../spec/work-profile.zh-CN.md). Translation does not create a new version or change accepted behavior.
- Translations preserve section numbering, field names, identifiers, requirement strength, reference scope and acceptance/rejection conditions. JSON examples retain the source values, including Chinese sample text, so readers can compare the same definition across languages.
- The core specifications translate Mermaid display labels while retaining their structure. Guide images are available in Simplified Chinese and English: the Traditional Chinese guide uses the Simplified Chinese images; Japanese, Korean, German, French and Spanish guides use English images. Every guide explains their meaning in its own language, without requiring the images to understand the concepts.
- The English guide includes localized sample input/output for explanation; the shared example files and translated normative specifications retain the canonical values. Explanatory examples do not amend the contract.
- Translations have not undergone complete review by native-language domain experts. Report a suspected mismatch with the language, section, exact text and proposed correction through [Issues](https://github.com/teloa-ai/ai-worker-standard/issues). Interpretation differences are resolved against the two normative Chinese documents.

## 2. Shared resources

These resources are shared rather than duplicated across eight directories. Their language coverage is stated here so that a complete translated specification is not mistaken for a translation of every repository file.

| Resource | Language / format |
| --- | --- |
| [Product and specification mappings](mappings.md) | Chinese and English sections |
| [Examples and their explanation](../examples/README.md) | English explanation; JSON/YAML with canonical source strings |
| [Conformance and evidence](../CONFORMANCE.md) | English, with a Chinese boundary summary |
| [Governance](../GOVERNANCE.md) | English, with a Chinese summary |
| [Diagram source files](../diagrams/README.md) | Simplified Chinese and English; SVG, PNG and Mermaid |
| [Citation](../CITATION.cff) | Machine-readable author and publication information |
| [License](../LICENSE) | Apache-2.0 license text |

## 3. Product terminology

**AI Worker** is the shared standard concept. Teloa's product concept remains **AI employee** — **AI 员工** in Simplified Chinese and **AI 員工** in Traditional Chinese. Local translations may explain that name, but must not replace the product with an anonymous general-purpose Agent. Grok Bot is another possible product mapping, subject to contract and host checks.

**AI Team** describes a collaboration contract; **AI Organization** adds organizational governance. Sharing one Worker among people does not by itself establish a team of multiple Workers. The glossary and boundaries in each core specification remain the reference for these distinctions.

## 中文说明

仓库首页默认英文。八种语言均提供介绍、核心规范全文、工作与协作应用规范全文和图文指南；字段、规则编号及规范 JSON 示例保持一致。简体中文两份规范是解释依据，其他语言是完整阅读译本，尚未经过母语行业专家完整复核。

配图继续提供中英文源文件，其他语言的指南以完整正文解释图意。共用的映射、示例说明、符合性与维护文件按上表保留现有语言。Teloa 的产品概念始终是 **AI 员工**，AI Worker 是其背后的通用标准概念。
