<p align="center">
  <a href="https://www.teloa.ai/en/"><img src="assets/brand/teloa-reading.webp" width="120" height="120" alt="阅读中的 Teloa 小松鼠"></a>
</p>

# AI Worker 开放定义标准 1.0

**用一套工作合同描述 AI Worker、AI 团队和 AI 组织。**

**Max Luo、Morgan Chen｜Teloa 联合创始人** · 2026 年 10 月 18 日

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

[Website](https://www.teloa.ai/en/) · [Docs](https://docs.teloa.ai/en/) · [Marketplace](https://market.teloa.ai/en/) · [AI Security](https://security.teloa.ai/?lang=en) · [Blog](https://www.teloa.ai/en/blog/) · [Security reports](https://security.teloa.ai/report/?lang=en)

[![Version 1.0](assets/badges/version.svg)](https://github.com/teloa-ai/ai-worker-standard/releases/tag/v1.0) [![8 core rules](assets/badges/core-rules.svg)](spec/core.zh-CN.md) [![8 languages](assets/badges/languages.svg)](docs/languages.md) [![License Apache-2.0](assets/badges/license.svg)](LICENSE)

**AI Worker** 是身份和职责能够跨任务延续、在授权范围内工作、交付可验收成果的 AI 工作个体。在 Teloa 中，产品名称是 **AI 员工**，AI Worker 是其背后的通用标准概念。Grok Bot 也可按这一概念描述，实际适配须核对下文的映射要求。

## 1. 核心概念

| 概念 | 描述什么 | 解决什么问题 |
| --- | --- | --- |
| AI Worker | 具有明确身份、职责、工作合同和交付检查的工作个体 | 跨任务、模型和临时 Agent 仍可追溯工作归属 |
| AI Team | 围绕共同成果分工协作，明确交接与验收的组合 | 让协作具备可检查的交付合同 |
| AI Organization | 具有组织责任、资源边界和变更治理的个体与团队组合 | 让团队在明确的治理规则下共同工作 |

持续身份不要求独占机器、永久进程或长期记忆。“责任”指工作归属与可追溯性，不表示法律人格。群聊或成员名单本身不是团队合同。

## 2. 范围与格式

| 项目 | 1.0 的约定 |
| --- | --- |
| 核心对象 | `agent` 与 `organization` |
| 必需工作应用规范 | `urn:teloa:profile:ai-work`，版本 `1.0` |
| Worker／Team／Organization | Worker：核心＋工作应用规范；Team：增加协作合同；Organization：再加治理合同 |
| 交换格式 | JSON；可按受限转换规则使用 YAML 编写 |
| 核心检查 | 八项通用规则 R01–R08；六项核心交换用例 T01–T06 |
| 解释依据 | 简体中文核心规范与工作应用规范；另有七种语言的完整阅读译本 |

标准定义身份、不可变修订、引用、职责、工作接口、协作和治理。模型执行、调度、连接与授权由宿主实现。导入定义不会安装工具、连接账号、开始工作或启用例行任务。只有六个核心字段的 `agent` 文件，还不是完整 AI Worker 定义。

## 3. 阅读入口

| 文档 | 内容 |
| --- | --- |
| [图解与可搜索正文](docs/guide.zh-CN.md) | 组成、技术架构、交付流程和完整示例 |
| [核心规范全文](spec/core.zh-CN.md) | 字段、固定修订、引用、通用规则和交换用例 |
| [工作与协作应用规范全文](spec/work-profile.zh-CN.md) | 工作合同、资源、交接、例行、实例绑定和治理 |
| [示例](examples/README.md) | 完整 Worker JSON/YAML、组合定义和输入输出 |
| [产品与外部规范映射](docs/mappings.md) | Teloa、Grok Bot、OASF 与 Oracle Agent Spec |
| [符合性范围](CONFORMANCE.md) · [维护规则](GOVERNANCE.md) | 证据边界、参与方式与维护 |
| [八语言阅读索引](docs/languages.md) | 完整阅读版本与翻译规则 |

每份指南都用正文解释配图。译本保留字段名、规则编号和 JSON 示例，尚未经过母语行业专家完整复核；解释有分歧时以两份简体中文规范为准。共用的映射、示例和维护文档使用英文和／或中文，索引中逐项标明。

## 4. 从一个完整 Worker 开始

1. 阅读[组成与交付指南](docs/guide.zh-CN.md)。
2. 查看 [document-worker.json](examples/document-worker.json) 或等价的 [YAML](examples/document-worker.yaml)。它接收 `material`，返回 `summary` 和 `openQuestions`。
3. 检查职责、输入输出约束、必要资源和验收合同，对照[示例输入](examples/document-input.json)与[示例输出](examples/document-output.json)。
4. 如需运行，在宿主中绑定实际实例、受支持的资源和有效授权；分别记录成果、验收与业务采用。

这些文件是合同示例，不是部署包或符合性验证器。组合示例包含尚未解决的绑定，不能直接用于真实执行。

## 5. 产品名称与外部规范

Teloa 面向用户的产品概念始终是 **AI 员工**。Worker 定义描述岗位与工作合同，Worker 实例标识一位具体 AI 员工；AI 团队通过 Team 合同组织协作交付。

Grok 的 **Bot** 可以映射为 AI Worker。多人共用的 **Team Bot** 先按共享 Worker 理解，不自动等于多个 Worker 组成的团队。是否符合取决于定义与宿主行为，不能只看名称。来源与核对项见[映射说明](docs/mappings.md)。

本规范吸收 OASF 的发现分类，以及 Oracle Agent Spec 的组件引用、接口与组合思路；不据此宣称格式或运行时全面兼容。

## 6. 地位、参与与引用

这是 **Teloa 维护的开放规范**，尚未取得国际标准地位，也不提供认证。不声称得到 xAI、AGNTCY、Oracle 或标准组织的背书。独立互操作须由独立实现提供可复现证据。

欢迎通过 [Issues](https://github.com/teloa-ai/ai-worker-standard/issues) 提交具体歧义、反例和带版本的实现报告。变更见[维护规则](GOVERNANCE.md)，声明范围见[符合性范围](CONFORMANCE.md)。

建议引用：**Max Luo、Morgan Chen. AI Worker 开放定义标准 1.0. Teloa, 2026.** 机器可读引用：[CITATION.cff](CITATION.cff)。

## 7. 许可

原创内容采用 [Apache-2.0](LICENSE) 许可。产品名称与第三方规范保留各自权利。阅读或实现本规范不需要 Teloa 账号或服务。

Teloa 名称与小松鼠素材遵循[独立品牌使用规则](assets/brand/README.md)。
