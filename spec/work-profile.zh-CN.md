# AI Worker 工作与协作应用规范

日期：2026-10-18。版本：`1.0`。属于 [AI Worker、AI Team 与 AI Organization 开放定义标准 1.0](core.zh-CN.md)的规范性附录。它定义工作语义与适配要求，不定义新的运行引擎。

## 1. 用一份合同连接 Worker、Team 和 Organization

员工提供工作入口；团队提供自己的工作入口，并说明成员如何分担；组织继续组合团队。三者都明确输入、输出、依赖、责任与验收。组织不会自动暴露成员所有工作，加入组织也不会自动分享个人记忆。

工作应用规范作为一个必需扩展出现：`id = urn:teloa:profile:ai-work`、`version = "1.0"`、`required = true`。这是本规范的固定标识，不是身份认证或在线服务入口；任何宿主均可实现，不要求 Teloa 账号。使用者不得改成 optional 来绕过关键语义。只有核心 agent 字段的文件只能声明核心交换支持，不能声明完整 AI Worker 定义符合性。

核心规范中的非空字符串、LocalId、固定引用、无副作用导入与未知扩展规则继续适用。下列结构均只允许列出的字段；除注明可选外均必需。没有适用内容时使用明确的空数组，不能用 null 代替。各类 id 只在所属数组唯一，引用位置按本附录明确解释。

## 2. 扩展 data 的结构

| 字段 | 类型 | 用途与缺省 |
| --- | --- | --- |
| responsibility | Responsibility | 岗位或组合体的判断规则 |
| resources | Resource[] | 方法、资料、应用和策略目录；允许为空，列出不等于整体必须启用 |
| works | 非空 Work[] | 对外承诺的工作入口 |
| requires | LocalId[]，可选 | 所有工作共同需要的 resources；省略视为空 |
| routines | Routine[]，可选 | 例行安排；省略视为空，不自动启用 |
| authorityRoles | AuthorityRole[]，可选 | 外部批准、验收或责任角色；省略视为空 |
| memoryPolicyRef | LocalId，可选 | 指向 purpose=policy 的资源；存在时计入整体依赖；省略表示不声明跨任务记忆能力 |
| collaboration | Collaboration | organization 必需，agent 不得使用 |
| governance | Governance，可选 | 仅 organization 可用；声称支持本版 AI org 定义时必须提供 |

**Responsibility** 含五个必需字符串数组：`triggers、autonomousActions、confirmationPoints、escalationRules、deliveryChecks`。分别说明何时接手、自主动作、确认点、升级规则和交付检查。triggers、autonomousActions、deliveryChecks 必须非空；另两项在确实无额外业务确认或升级规则时可为空。空确认数组不免除宿主授权，空升级数组不允许在缺少输入或权限时继续执行。描述不授予工具权限。核心 instructions 说明持续职责，工作细节由 Work 和资源提供；文字是否有实质含义仍需内容评审，不能仅凭字符串非空判断。

**Resource** 含 `id:LocalId、title:非空字符串、purpose、binding`，以及可选 `requires:LocalId[]`。purpose 为 `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`。这些是资源用途标签，不是必须新增的产品资源类型。binding 含 `kind、config`，复用核心 Capability 的四类绑定及其 config 规则，不重复定义协议。

Resource.requires 只引用同一定义的 resources，省略视为空。固定资料优先带 digest；可变来源须在实际执行时固定读取版本。方法可引用完整技能包，SOP 可引用方法或流程资源。不能将仅可读的应用说明、模型配置或政策文件标为应用已连接、运行环境已满足或政策已执行。

对于 runtime、policy、trigger 或以资源形式描述的 application，适配报告必须记录其原生格式、精确版本和明确处理者。处理者可以是支持该格式的宿主功能或经明确交办的人工环节；只有文字说明且无人承担执行时，不满足必要条件。此约定保留现有原生格式，不另造通用权限或调度语言。

**AuthorityRole** 含 `id:LocalId、name:非空字符串、kind:human | service`。它是职责槽位，不是账号。宿主必须绑定可信主体，service 表示宿主确认的外部服务主体，不代表模型可以自己成为批准人。需要人工批准时必须声明 human；角色作用由引用它的验收、升级或治理合同确定。

## 3. 每项工作的输入输出与交付

| Work 字段 | 类型 | 含义 |
| --- | --- | --- |
| id、title、instructions | LocalId、非空字符串、非空字符串 | 工作入口、名称及目标/方法要求 |
| inputs、outputs | JSON Schema 对象 | 分别描述完整输入对象和完整输出对象 |
| requires | LocalId[] | 本项工作需要的 resources；允许为空 |
| responsible | ParticipantRef | 一个主要负责位置 |
| acceptance | Acceptance | 完成检查与验收责任 |

`ParticipantRef` 是 `self`、`member/<LocalId>` 或 `authority/<LocalId>`。self 指当前定义对应的实例；member 只指当前 organization 的直接 Member 槽位，包括按任务参与的 consultant；authority 指当前 data.authorityRoles。agent 不得使用 member 引用。所有引用必须存在，不能按名称猜测。responsible 必须是 self 或 member；外部人员可负责验收和升级，不被伪装成 AI 执行者。organization 使用 self 时，表示组合体对外交付负责，宿主仍须记录实际执行者、输入分发与输出汇总的承担者，不能以组织名义代替实际贡献者。

**Acceptance** 含 `criteria:非空字符串数组、reviewer:ParticipantRef、independent:boolean`。criteria 为可检查的完成要求；reviewer 是验收职责位置。independent=true 时，宿主必须证明实际复核主体与被审成果的实际执行主体不同；复合成果须核对实际贡献者，不能只比较主要负责者或团队名义身份。两个槽位、两个会话或两个名称都不构成独立性证明。独立主体也不自动证明判断质量。验收记录固定被审的成果版本；结构校验通过、模型完成和业务采用分别记录。

inputs、outputs 的根必须为 `type:object`，并声明 `$schema = https://json-schema.org/draft/2020-12/schema`。复用 [JSON Schema 2020-12](https://json-schema.org/draft/2020-12) 的标准词汇，不自建类型语言。Schema 可以包含说明及 `$defs`；所有引用（含 `$ref`、`$dynamicRef`）只能指向本 Schema 文档，不允许借嵌套 `$id` 改变引用的文档边界。导入不联网解析引用。实现须报告不支持的词汇或约束，不得忽略后称验证通过。format 如用于关键验收，须显式声明并验证相应格式检查能力。

输入缺项时报告待补充；输出缺项或类型不符时不能声明符合该成果合同。不隐式把字符串转数值、截断小数或把 false 转为 0。Schema default 是描述，不自动代表用户提供或授权了该值。自由文本成果可以放在字符串字段中；业务证据与判断质量仍由 acceptance 检查。

**依赖检查：**先满足核心 capabilities 及其 R05/R06 条件，再取 data.requires、memoryPolicyRef、governance.changePolicyRef（如存在）和所选 Work.requires 的并集，递归展开 Resource.requires。缺失引用或资源依赖循环是定义错误。缺少连接、权限或解释器是相应工作未就绪；其他工作的专用资源未配置不阻塞当前工作。整体必要依赖缺失仍阻塞整体执行。解析支持、配置就绪与实际授权分别报告，不能将局部可用声明为全部可运行。

## 4. 例行与经验

**Routine** 含 `id:LocalId、work:LocalId、policyRef:LocalId`，以及可选 `triggerRef:LocalId`。work 引用当前 works；policyRef 指 purpose=policy 的资源，triggerRef 指 purpose=trigger 的资源。执行岗位归属于当前定义绑定的实例；创建人、调度所有者和实际授权主体由宿主另行绑定，不必与 Worker 身份相同。不得在导入时另造员工、扩大例行可见性或把例行移给其他成员。

triggerRef 缺省表示待安排；即使存在，也只表示触发定义，实例默认未启用。触发格式由宿主处理；Teloa 现有 daily/weekly 或本地事件只声明实际支持范围，任意 cron 或外部事件不能静默近似。先验证真实工作与实际触发，再经部署者启用。

例行 policy 必须明确输入窗口/游标、无输入与失效输入行为、触发去重及业务处理记账、重试、并发/补跑、预算、通知和暂停条件。缺项时可以保留安排，但不得启用。成功触发不等于处理成功；只有约定的成果状态达成才推进业务处理状态，重试不得用新触发编号绕过业务去重。没有统一的固定重试数或“两次无进展”行业规则。

memoryPolicyRef 只定义经验保存、来源、范围、审阅与撤回规则，不携带作者的私有记忆。template-reference 资源是带来源的模板材料；历史日志不能冒充新实例亲历事实。共享员工的个人、业务、项目与群上下文仍按宿主授权分隔。同一员工实例参与多个团队不意味着团队历史互相可读。

## 5. 团队协作与组织治理

| Collaboration 字段 | 类型 | 含义 |
| --- | --- | --- |
| roles | RoleAssignment[] | 覆盖每个直接 Member，恰好一次 |
| coordinator | ParticipantRef，可选 | 如提供，只能指当前直接 Member 槽位；省略不自动选人 |
| handoffs | Handoff[] | 明确的责任和材料交接关系；允许为空 |
| sharing | Sharing[] | 共享资源的期望接收位置；允许为空 |
| stop | StopPolicy | 有限协作、无进展与升级规则 |

**RoleAssignment** 含 `member:LocalId、duty:非空字符串、participation:member | consultant`。duty 说明在本组合中的分工，不改写成员原职责；冲突时停止受影响工作。consultant 表示按任务参与的顾问，不自动加入群或得到群历史。两类槽位的固定定义引用都按核心规则解析；参与范围在实例绑定和任务授权中落实。

coordinator 与 Work.responsible 均可指向 consultant 槽位，但必须明确交办并取得当前工作的参与确认；它仍只按任务参与，不因此长期接管团队或获得完整群历史。`member/` 是引用 Member 槽位的语法，与 participation 中的 member 取值不是同一概念。

**Handoff** 含 `id:LocalId、from:WorkRef、to:WorkRef、when:非空字符串、inputs:对象`。WorkRef 只允许 `member/<memberId>/<workId>`，引用当前组合体直接成员的已声明工作，成员定义版本取自 Member.ref。它表示已产出成果的成员工作向接收成员工作交接；不支持 `self/<workId>`，不把组合体入口/出口混入同一引用语义。子团队对外暴露自己的 works，不直接穿透其内部成员。

Handoff.inputs 的键是接收工作输入对象的顶层字段名，值是发送工作输出对象的顶层字段名，均按原字符串解释，不执行表达式、脚本或点号路径。字段须在对应 Schema.properties 中声明；复制值还须通过接收方 Schema 的实际校验。接收工作所需的其他输入必须明确补齐；未齐时返回缺口，不伪造成功接收。when 是业务条件，只有被明确交办或由已支持触发机制确认时才进入交接，不凭一段文字宣称监听已实现。

组合体 Work 的输入分发和输出汇总由宿主的原生编排或明确交办的协调者落实，本版不另造入口/出口映射语言。适配报告必须列明：哪个组合体输入进入哪个成员工作、每个必需输出由谁及哪个固定成果提供、由谁检查完整性。定义静态校验不证明这些映射已执行；映射或处理者未确定时，相应组合体工作不得标为就绪。组合结果必须通过其 outputs Schema 与 acceptance，不能把成员任意输出自动拼成团队成果。

每次实际交接固定来源工作/Run、成果版本、发送与接收主体、映射后的材料、允许范围和接收/拒收结果。接收方确认并建立工作归属后才算交接完成。只传合同所需且已授权的材料，不隐式转交整个会话、私人记忆或连接凭据。群中发送消息与交接成功分别记账。

**Sharing** 含 `resource:LocalId、recipients:非空 ParticipantRef 数组`。resource 引用当前 resources，接收者只能为当前组合体的直接 Member 槽位，即 `member/<LocalId>`，包括 consultant 槽位；是否可见及具体版本仍由宿主授权。sharing 不是权限授予，也不会把共享资源自动变成所有成员的必要依赖；工作需要时须由接收工作输入或其依赖明确承接。

**StopPolicy** 含 `maxHops:正整数、maxNoProgress:正整数、escalateTo:ParticipantRef`。计数作用于同一次工作及其衍生交接链，不因换员工或重试清零。maxHops 限制交接次数；maxNoProgress 限制连续没有新增可验证证据或没有已声明验收项改善的轮次。达到任一限制时停止继续派工并升级；恢复须有可信主体的明确处理和新记录。具体数值由场景配置，不是员工能力评分。

**Governance** 含 `accountable:ParticipantRef、changePolicyRef:LocalId`。accountable 为组织范围的责任位置；changePolicyRef 指 purpose=policy 的资源，并自动计入整体依赖。该策略必须明确成员任职/撤任、定义与约束变更、暂停及在途工作处理。组织级数据、预算和执行限制通过 data.requires 中的 policy/runtime 资源表达；需继承的约束必须明确适用成员和作用域，并由宿主核实。约束不能自动扩大成员权限；冲突不得择宽放行。

AI Team 使用协作合同；本版 AI Organization 定义还必须具备 governance。两者使用相同 organization 核心类型，不按人数划分。任意新增组织权限、实际成员管理或跨组织身份系统不在这份定义规范中自动成立。

声称完整 Team 或 Organization 定义符合性时，其成员引用闭包中的 agent 均须满足 AI Worker 定义合同，organization 均须提供本附录的协作合同；不是每个子团队都必须声明 governance。只有核心字段的临时 Agent 可由 Worker 的运行机制按授权调用，但不能冒充一个已符合本版的持续成员。外部人工或服务验收位置使用 authorityRoles 表达。

## 6. 宿主绑定与实际执行

绑定记录存放于部署环境，不强制所有宿主使用同一数据库或 JSON 结构，但必须可以核对以下信息：

| 记录项 | 必须区分的内容 |
| --- | --- |
| 定义位置 | 根定义 id/revision、逐级 Member.id 组成的槽位路径、路径终点的固定定义 |
| 实例选择 | 明确新建还是复用、实际实例身份与版本、所属可信主体；导入阶段不创建 |
| 连续身份 | 实例身份可跨任务追踪；模型、会话、临时 Agent 和 Run 身份分别记录；启用、暂停、退休与恢复授权可核对 |
| 任务与授权主体 | 管理主体、任务发起人、例行所有者、资源授权主体分别记录；共享 Worker 不意味着共用一人的全部授权 |
| 复用关系 | 多个槽位是否绑定同一实例；本版共用须对应相同固定定义，不能按同名推断 |
| 责任角色 | authorityRoles 对应的可信主体与 human/service 类型；批准人不能来自模型自报身份 |
| 运行条件 | 所选工作、资源和原生格式/版本、实际连接、有效授权、模型及资源限制 |
| 版本与变更 | 绑定版本、采用的成员分工、约束与材料版本；变更后的待办处理方式 |

同一模板生成两个实例与同一实例跨团队任职必须有不同记录。共用实例的群可见性和执行授权按所属工作核对，预算按实际适用的同一额度记账，不能通过重复绑定获取两份原本只有一份的额度。

一次执行必须能追溯根定义、所选工作、实际执行主体、资料及资源版本、有效授权和成果版本。成员替换或模板升级不悄悄接管旧 Run；原执行恢复、终止或新建任务按明确变更策略处理。若宿主无法兑现关键约束，保留定义并报告阻塞，不将其降成备注继续运行。

工作输入、检索资料、历史消息及工具输出是带来源的工作材料，不是更改 Worker 定义或扩大权限的可信指令。宿主必须区分这些材料与授权来源。Worker 可调用临时 Agent，但对外交付仍归于所选 Worker；临时执行者不自动成为 Team 成员。独立复核检查实际贡献者及代理关系，不能借新会话或临时助手为自己的成果制造独立身份。

执行记录至少区分工作成功、失败、部分完成、结果未知与暂停，允许宿主使用自己的状态名称。部分输出或超时回包不能自动标为工作成功；有外部副作用且结果未知时，须先核对原操作状态再决定重试。任务要求、验收条件、结果与限制构成质量评估范围；不规定所有行业共用的分数或“通过一次即可上岗”。

## 7. OASF 与 Agent Spec 的实际吸收边界

OASF discovery 作为独立扩展承载，使用固定版本的原生 discovery Schema URI 作为 id、相同的 schema_version 作为 version，原生 payload 原样置于 data。仅作发现信息时 required=false；用于不可忽略的执行选择条件时必须声明 required=true 并验证其语义，分类本身仍不授予权限。此处使用本规范的 Extension 包装；它不同于 A2A Agent Card 中的 AgentExtension 包装。两种格式均可承载原生 OASF discovery payload，但其包装字段不能直接混用。

例如原生 payload 的 `schema_version、skills、domains` 保持原名和含义。skills 是能力类别，domains 是领域；只有固定目录中真实存在的条目才可声明 OASF 分类验证通过。宿主可离线使用可信的固定 Schema/分类快照，不因导入 URI 自动访问服务。没有对应分类时保留未映射说明，不编造官方编号。[OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

OASF 的 record 元数据和 locators 可作为导出视图，来源、版本及未映射字段进入适配报告；它们不能取代本规范的固定修订或实际实例身份。源定义保持单一真源，派生视图不成为第二份可独立修改的运行配置。[Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Agent Spec 的引用、输入输出与组合思想用于本版 Work 和 Handoff。其 `$component_ref` 属于配置内引用；导出时须显式建立它与本规范固定定义/资源的对应关系。其输入输出属性列表与本版对象 Schema 需要转换，不能只改字段名。只在完整保留接口、约束和执行语义时声明所选版本/子集的映射支持。[Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm、ManagerWorkers 可以作为匹配团队的适配目标；会话转交、工作成员能否直接面对用户以及消息流必须逐项核对。当前合同不要求 Teloa 切换到其运行引擎，也不把全部团队强制转换为主管—工作者模式。

## 8. 对现有设计的覆盖

| 现有要素 | 本规范的去向 |
| --- | --- |
| A01 岗位身份与使命 | 核心 id/revision/name/instructions；头像等展示资料以非关键扩展保留 |
| A02 责任与判断 | responsibility 五组字段；实际授权在宿主绑定 |
| A03 Skills | purpose=method 的 resources，引用完整方法包或原生方法资源 |
| A04 知识 | purpose=knowledge，保留来源、版本或摘要，按工作引用 |
| A05 SOP | purpose=procedure 及其资源依赖；跨角色交接由 collaboration 表达 |
| A06 工作模板 | works 的目标、I/O、依赖、负责位置和验收 |
| A07 例行任务 | routines、原生触发和执行策略；未配置/未启用状态由宿主记录 |
| A08 应用连接 | purpose=application、原生绑定与实际连接；目录声明不代表安装成功 |
| A09 权限 | 必要 policy 资源、工作范围、宿主可信授权；不导出凭据 |
| A10 运行与空间 | runtime 资源要求与宿主模型、预算、文件范围绑定 |
| A11 经验 | memoryPolicyRef 和来源参考；实际私有经验留在实例 |
| A12 成果与质量 | outputs、acceptance 与执行证据；结构与业务质量分别检查 |
| T01 共同目标 | organization.instructions 与对外 works |
| T02 岗位责任 | Member、collaboration.roles、Work.responsible 和 authorityRoles |
| T03 共享上下文 | sharing、工作输入以及宿主按范围授权 |
| T04 协作交接 | handoffs、输入映射、接收确认与 stop |
| T05 工作安排 | 成员或组合体的 routines、procedure 和协调分工 |
| T06 权限资源 | 整体/工作 requires、成员原权限和宿主实际额度 |
| T07 团队验收 | 组合体 outputs/acceptance、成员贡献及固定成果版本 |

当前社区仓的 RoleDefinition、GroupDefinition、RoleWorkDelegation、RunRoleSnapshot 和资源包关系作为适配对象，不改成新的产品对象。Teloa“AI 员工”对应 Worker 产品形态；RoleDefinition、固定岗位版本及实际 Role 实例共同参与映射，单独改名不能证明符合。role-coordination 只表示静态协调关系；创建群、派工和自动交接各自需要实际支持与授权。

Teloa `kind: employee | twin` 是产品角色类型，两者只要满足同一工作合同均可映射为 Worker；twin 不产生代表本人批准的权利。原 kind 在适配报告或非关键产品展示扩展中保留，不能直接写入核心 kind。`scopes` 连同 dataScope/executionScope 映射为必要的范围策略及宿主授权绑定；跨宿主无法对应业务范围时，相关工作保持未就绪。它们属于关键限制，不能只存入可忽略的展示扩展。此处字段已对照社区仓 `packages/contract/src/roles.ts`，不代表转换器已经实现。

Grok 公开 recipe 的 profile 映射岗位说明，skills 映射方法，routines 映射工作与待安排的例行，plugins 映射待配置应用，memory 映射带来源的模板材料；其中 profile 类工作准则与 log 类作者日志分别保留。gettingStarted 仅作为入口提示。缺少职责、日程或验收信息时报告缺项，不从名字或原作者经验猜造。完整原件及其摘要由适配报告关联，关键语义不得藏入可忽略扩展。Grok Team Bot 映射共享 Worker，不按名称自动生成多 Worker 的 Team。

Grok Team Bot 的公开文档将个人例行归属于创建者，连接也可能按当前用户或 Bot 凭据工作。因此适配时必须保留例行所有者、运行身份及资料可见范围，不将所有人的例行合并成共享 Worker 的全员任务。这是来源行为的映射要求，不要求其他宿主复制其权限默认值。[Team Bots](https://docs.x.ai/grok-bot/team-bots)

**私人例行的落点：**个人例行只映射为宿主侧按所有者与可见范围隔离的安排，引用 Worker 已声明的 Work；不得进入共享定义的 routines 或被共享导出。适配报告标记“保留为私人实例安排，未进入定义”，报告本身也须服从其可见范围。只有原作者明确发布为模板并去除私有内容后，才可将模板放入定义的 routines；导入仍默认不启用。无法保留所有者、可见性或工作引用时，报告无法映射，不得合并或静默丢失。

适配报告至少列出源格式/版本/摘要、目标定义修订、字段对应、保留/转换/未支持项，以及未支持项影响的工作。原件留存不等于语义适配成功。转换不能兑现权限、交接或验收条件时必须阻止受影响工作。

## 9. 正反例与完成声明

[完整组成示例](../examples/composition-fixture.json)使用固定的本地定义集合表达：两名员工、两个团队复用这两份员工定义、一个嵌套组织，以及宿主绑定与工作就绪的预期。集合外壳和说明是测试夹具格式，不是新增分发协议，也不是可直接导入 Teloa 的资源包。

| 检查 | 必须得到的结论 |
| --- | --- |
| 零技能、零记忆、零例行的简单岗位 | 可定义；明确工作、I/O、责任和验收仍必需 |
| 只有名称、提示词和 kind=agent | 可以符合核心交换；不得据此声明完整 AI Worker 定义 |
| triggers、自主动作或交付检查为空 | 不符合本版工作附录；不能靠产品名称补足 |
| 同一 Worker 启动两次任务、更换模型 | 必须能追踪同一 Worker 实例及两个独立 Run；仅两个会话不能证明身份连续性 |
| 同一员工定义在两个团队出现 | 只说明定义复用；是否同一实例由槽位绑定决定 |
| 一个工作缺专用应用，另一工作输入齐全 | 前者未就绪；后者仍核对整体条件及自己的依赖，不自动被前者阻塞 |
| 子组织版本缺失、循环或未知必需 Profile | 停止装配并报告具体引用，不换版或忽略 |
| 错误接收字段、缺必要输入或输出类型不符 | 不能标交接成功或成果符合；不自动补值、转换类型 |
| 两个复核槽位绑定同一实际执行主体 | independent=true 不满足 |
| 顾问只参与某项复核 | 不自动加入全部群、读取全部历史或复制新员工 |
| 计划缺触发或策略不完整 | 保留为未就绪安排，不启用 |
| OASF 分类未知或 Agent Spec 协作模式或接口约束不等价 | 报告不支持/映射差异，不宣称全面兼容 |

本规范声明的是结构与行为合同。文稿检查、示例检查、真实连接/模型执行、独立实现互通和行业采用分别报告。验证范围与实现报告要求见[符合性说明](../CONFORMANCE.md)。
