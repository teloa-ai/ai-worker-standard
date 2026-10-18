# Product and specification mappings / 产品与规范映射

Reference date: **2026-10-18**. Target: core `0.1.0-draft.7` and work profile `0.1.0-draft.2`.

These mappings are the authors' interpretation of the cited product documents and Teloa's public contracts. They are not vendor endorsements or evidence that a converter has been implemented. A concrete adapter must preserve the source, identify its version or content digest, and report what it retained, converted or could not support.

以下对应关系是本草案的适配解释。产品名称不构成符合性声明，字段对应也不证明真实运行已经完成。权限、验收或交接语义无法保留时，必须阻止受影响的工作。

## Teloa

| Teloa concept | Target | Required handling |
| --- | --- | --- |
| AI employee / AI 员工 | Worker | Preserve the role definition, adopted revision and actual instance separately |
| `kind: employee` or `twin` | Core `kind: agent` plus the work profile | Retain original kind in adapter metadata; a twin has no implicit right to approve as the human |
| Role instructions | `instructions` and `responsibility` | Identify triggers, autonomous actions, confirmation points, escalation and delivery checks; report missing content |
| Role skills, knowledge and procedures | `resources` | Preserve source, version/digest and per-work dependencies; a reference alone does not prove the content reached a Run |
| Work template / delegation | `works`, actual task and Run | Separate the reusable contract from one assignment and its frozen inputs |
| Group / project | Team context or host binding | A group becomes a mapped Team only with member definitions, roles, input distribution, handoffs, output assembly and acceptance |
| `scopes`, `dataScope`, `executionScope` | Required policy and host authorization | Preserve restrictions as enforced conditions; never keep them only in an optional display extension |
| Plans / routines | Work-linked arrangements | Retain owner, visibility and actual trigger support; imports remain disabled |
| Role memory | Instance-private state and a memory policy | Export the policy or deliberately published reference material, not private experience by default |
| Result / business candidate | Fixed artifact and acceptance records | Distinguish generation, review, owner adoption and any external action |

The public contract source is [Teloa `roles.ts`](https://github.com/teloa-ai/teloa/blob/main/packages/contract/src/roles.ts). `main` is a navigation link; an actual adapter must pin its own source revision and digest. Host validation may normalize text or limit fields. Preserve the original definition and report any projection differences rather than claiming a lossless round trip through a product-specific form.

Teloa 的既有角色、群、项目、任务和成果可承载这些关系；本草案不要求增加一套重名对象。绑定表和运行证据要能解释真实关系。某个平台字段存在，不等于本次配置已经有效。

## Grok Bot

Grok's design describes a persistent Bot and separates reusable Skills from Routines. We map a qualifying Bot to a Worker, and reusable recipe material to a candidate definition rather than to an already authorized instance. [Grok design](https://x.ai/news/designing-grok-bot) · [Skills and routines](https://docs.x.ai/grok-bot/skills-routines-and-automations)

| Grok concept | Candidate mapping | Boundary |
| --- | --- | --- |
| Bot / profile | Worker; instructions and responsibilities | A complete Work contract may need additional information |
| Skills | Method resources | Preserve the method and its dependencies, not just a skill name |
| Files / knowledge | Knowledge resources | Preserve provenance and actual visibility |
| Plugins / tools | Application resources and host connections | Keep actual credentials outside definitions; verify the effective principal |
| Memory in a public recipe | Deliberately shared reference material or policy | Author logs are sourced examples, not the new instance's lived history |
| Routine template | Work plus an inactive arrangement | Missing trigger or policy means not ready |
| Personal routine | Private host arrangement referencing a Work | Do not include in a shared definition or shared export |
| Team Bot | Shared Worker | Do not infer a multi-Worker Team from the product name |

The Team Bot documentation describes one Bot shared by people, private conversations, and routines belonging to their creator. Its connector authentication can use the current person or the Bot's own configured credential. An adapter must preserve those distinctions instead of merging user permissions or routines. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

**共享 Worker 与 AI Team：**多人使用同一 Bot，是一个 Worker 面向多个使用者。多个 Worker 分担一份共同成果，才进一步核对 Team 合同。一个产品可以同时具备这两种关系。

个人例行只留在按所有者和可见范围隔离的宿主安排中。只有作者明确把它发布为去除私有内容的模板，才可以进入定义的 `routines`；模板导入后仍不启用。无法保留这些范围时，报告无法映射，不能静默丢失或转成全员任务。

## OASF

OASF's record and discovery definitions provide names, descriptions, schema versions, skill/domain taxonomies and resource locators. Preserve native payloads and pin their schema/taxonomy version; do not invent official classification identifiers. [Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/) · [Discovery Extension](https://docs.agntcy.org/oasf/discovery-extension/)

本草案的工作附录规定了 discovery 扩展的包装方式。仅作发现信息时可以是 optional；若用于不可忽略的执行选择条件，则必须是 required，并由宿主验证。分类没有授予权限，也不代替 Work 合同。OASF 分类中的 `skills` 不等于本规范引用的可执行 Agent Skills 文件包。

## Oracle Agent Spec

Agent Spec 26.1.2 offers component references, JSON-Schema-based interfaces, and composition components such as Swarm and ManagerWorkers. These are useful source concepts; its configuration-local `$component_ref` differs from this draft's cross-definition `id` + `revision`. [Specification 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

接口属性列表与本规范的根对象 Schema 需要显式转换；协作还需核对会话转交、消息流、面向用户的成员以及实际权限。本草案不采用隐式输入输出类型转换，不强制主管制，也不声称兼容其全部运行时。

## Adapter report / 适配报告

A useful report identifies the source format/version/digest, target definition revision, each field mapping, preserved and unsupported semantics, affected work, and host bindings still required. For composed Work, also identify input distribution, each required output's producer and fixed artifact, and the completeness checker. Report the actual result rather than treating a mapping table as successful execution.

报告至少回答：原件是什么、转换了什么、丢失或未支持什么、哪些工作受影响，以及何时可以执行。对同一 Worker 的复用要有实例绑定；对独立复核要核对实际贡献者。记录中出现两个名称，不能代替这两项证明。
