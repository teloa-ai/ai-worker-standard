# AI Worker: how an employee works and a team delivers

[Languages / 语言](languages.md)

**Max Luo and Morgan Chen, co-founders of Teloa** · 18 October 2026

In Teloa, people create and work with **AI employees**. **AI Worker is the underlying standards concept**: it describes how an AI employee works through persistent identity, responsibilities, work contracts and collaboration boundaries. The standard also describes how AI employees form teams and agree on deliverables.

A name and a role prompt are enough to start experimenting with an AI employee. A second assignment raises more practical questions: is this the same AI employee as before? Which material did it use? What reached the next member? Who decides whether to accept the result?

The AI Worker standard makes these agreements explicit, so people can check what an AI employee undertakes and delivers, using technical terms that can be shared across products.

This guide introduces **AI Worker Standard 1.0**, including its core and work profile. Examples are teaching material, not customer results or execution evidence. Read the [full English core specification](../spec/core.en.md) and [work profile](../spec/work-profile.en.md). The [Chinese specifications](../spec/core.zh-CN.md) are normative for this edition.

The three core concepts have distinct roles:

| Concept | Definition | Question it answers |
| --- | --- | --- |
| **AI Worker** | An AI work entity with a persistent identity that undertakes responsibilities, accepts work and delivers checkable results within an explicit authorization boundary | Who is this AI employee, what does it do and how is completion checked? |
| **AI Team** | A composition organized around a shared deliverable, with explicit member responsibilities, material handoffs and acceptance requirements | How do several AI employees complete a job together? |
| **AI Organization (AI Org)** | A composition of workers and teams that adds organizational accountability, resource and information boundaries, and change governance | Who is accountable across teams, and how are resources and changes managed? |

Persistent identity means the same worker can be identified across tasks. It does not require continuous operation, a dedicated machine or long-term memory. Responsibility describes work attribution and traceability, not legal personality. The prose, tables and steps below explain the relationships; the diagrams provide a visual overview of the same content.

## 01 What makes up an AI employee

Consider an AI employee responsible for document summaries. Its job is to summarize supplied material and list unanswered questions. It can perform this job repeatedly using different models. Each result still belongs to the same AI employee, represented technically by a particular Worker instance, and records the definition revision used.

![Teloa AI employees and Grok Bots mapped to AI Worker, with five component groups](../diagrams/en/worker.png)

*Teloa AI employees and Grok Bots mapped to AI Worker, with five component groups.*

The arrows map concepts: Teloa retains AI employee as its product concept, and Grok retains Bot. Both can be described through AI Worker; the diagram itself does not establish completed adaptation or vendor certification.

**The five component groups of an AI employee** describe what its role needs to specify. Resources follow the work; an employee does not need every available tool.

| Component group | General meaning | A document-summary AI employee’s configuration |
| --- | --- | --- |
| **Role and responsibilities** | Persistent identity, mission, responsibility boundaries, autonomous decisions, confirmation points and escalation rules | Summarize supplied material; report contradictions without inventing facts |
| **Methods and evidence** | Skills supply methods, knowledge supplies evidence, and SOPs describe steps, checks and exception handling; preserve their sources and versions separately | Use the current input; add Skills, knowledge or SOPs when the job needs them |
| **Work and arrangements** | Work contracts specify goals, inputs, outputs, dependencies, responsibility and acceptance; routines specify when to invoke a work interface | Provide a `summarize` interface, invoked on assignment, with no scheduled routine |
| **Execution conditions** | Applications and tools, data scope, effective authorization, models, budgets and workspace requirements | Read authorized material; obtain separate approval for external publication |
| **Experience and deliverables** | Memory policies govern retaining, using and withdrawing experience; artifacts and evidence have fixed versions and undergo acceptance | Return a summary and open questions; the owner checks them against the source |

A memory store, external application and schedule are optional. Dependencies follow the work promised. External research needs a connection and authorization; summarizing supplied text does not need an application added merely to complete a checklist.

A complete Worker definition requires identity and revision, responsibilities and decision boundaries, and at least one work contract with inputs, outputs, a responsible position and acceptance. Actual use also requires the host to bind a specific instance, execution conditions and trusted authorization. The five groups organize identity, responsibility, Skills, knowledge, SOPs, work templates, routines, applications, permissions, runtime conditions, experience, and deliverables and quality. The [work profile](../spec/work-profile.zh-CN.md) specifies the fields.

## 02 A team needs traceable handoffs

For a material-review task, an analyst AI employee reads the evidence and a review AI employee checks the conclusions. The team delivers a report and a review. A group chat supports communication; the team contract also specifies the shared deliverable, responsibilities, handoff conditions and stopping rules.

**An AI Team combines a shared goal, members and responsibilities, shared-context boundaries, handoffs, work arrangements, resource constraints and acceptance.**

| Team element | Agreement to make explicit |
| --- | --- |
| Shared goal | What work the team accepts and what it delivers externally |
| Members and responsibilities | Each member's duties and how standing members and task-specific consultants participate |
| Shared context | Which material and version may reach which member; do not share entire conversations or private memories by default |
| Handoffs | Who sends which fixed artifact to whom, how inputs are mapped and how the receiver accepts or rejects them |
| Work arrangements | Who distributes inputs and assembles outputs: host orchestration or an explicitly assigned coordinator |
| Resource constraints | Authorization, applications, budgets and stopping conditions for each job, including where to escalate lack of progress |
| Acceptance | Which outputs are checked, who accepts which version and whether independent review is required |

A coordinator is optional; not every team needs a manager. A member list, group chat or organization chart does not replace these agreements.

The composition has its own work interface. For example, `deliver` takes `evidence` and returns `report`, `evidence` and `review`. The coordinator or host must identify who receives the input, which fixed artifact supplies each output and who checks completeness. Without those mappings, the work is not ready.

![Teloa AI employees collaborate under a Team contract; people approve and accept](../diagrams/en/delivery.png)

*Teloa AI employees collaborate under a Team contract; people approve and accept.*

The illustrated example has six steps:

1. **The human owner assigns the work.** Specify the goal, inputs and authorization scope, select the team's work interface and agree on delivery and acceptance requirements.
2. **The host or coordinator validates and distributes.** Check contracts, dependencies and instance bindings, distribute only permitted material and establish task ownership.
3. **The analyst AI employee submits its result.** Perform the assigned work, fix the report and evidence versions, and record actual contributors.
4. **The review AI employee confirms receipt and reviews.** Check the inputs, accept them and produce a fixed review. On rejection, record the reason and return it for coordination.
5. **The host or coordinator checks completeness.** Assemble fixed sources according to the team contract, check required outputs and retain contributor and handoff records.
6. **The human owner accepts and decides on adoption.** Accept a specific artifact version and separately record whether to use it in the business; acceptance does not authorize external actions.

This example assigns analysis, review and human acceptance to separate participants; each team follows its own contract. Preserve the evidence chain across steps: **authorized input → fixed report → review → final deliverable package**.

This contract fragment comes from the [complete composition fixture](../examples/composition-fixture.json):

```json
{
  "id": "review-report",
  "from": "member/analyst/inspect",
  "to": "member/quality/review",
  "when": "已有固定报告与证据且获得本项交接授权",
  "inputs": {
    "report": "report",
    "evidence": "evidence"
  }
}
```

The keys in `inputs` name receiver fields; the values name sender output fields. The Chinese condition requires a fixed report and evidence plus authorization for the handoff. This definition does not dispatch a task. An actual handoff also records the actors, source Run, artifact version, permitted material and the receiver's acceptance or rejection. A message in a group does not establish that usable input reached the receiver.

In the diagram, the review AI employee contributes a review. The designated owner accepts the complete deliverable. Independence must be checked against actual contributors. A reviewer who edits the report becomes a contributor to that revision and cannot independently accept that same revision.

## 03 What an AI Organization adds

An organization composes teams and workers, with accountability, resource constraints and change rules across them. More members or another group-chat layer does not create those agreements.

**An AI Org combines workers and teams, accountability across teams, resource and information boundaries, and escalation and change rules.**

| Organization element | What it adds to existing teams |
| --- | --- |
| Composition of workers and teams | Identify members and fixed definition revisions; a subteam participates through its own work interfaces without exposing every internal capability |
| Accountability across teams | Identify the trusted principal accountable for the organization and escalation destinations for issues across teams |
| Resource and information boundaries | Specify which data, budget and execution limits apply to which members, for the host to check and enforce |
| Escalation and change rules | Define appointment and removal, definition changes, suspension and recovery, and handling of work already in progress |

![Worker, AI Team and AI Org composition, illustrated with Teloa AI employees](../diagrams/en/composition.png)

*Worker, AI Team and AI Org composition, illustrated with Teloa AI employees.*

A security-operations organization could combine an investigation team and a detection-engineering team, with a shared quality AI employee. Sharing requires an explicit binding to the same instance. Two references to one role template do not prove that one employee occupies both slots.

The organization also identifies who is accountable, who appoints or removes members, how paused work is handled, and which data or budget constraints apply to which members. A worker serving two teams does not automatically gain both teams' histories or double a shared budget.

This is an expressible organization structure. A product needs binding, authorization and execution evidence to establish that it operates that structure. It differs from the downloadable structural fixture: that fixture's A and B teams reuse both the analyst and reviewer to illustrate instance bindings, and do not configure a real detection-engineering role.

## 04 Definition, instance and Run

A Worker definition describes an AI employee’s role and work contracts. A Worker instance corresponds to a particular AI employee in Teloa. A Run is one execution by that employee. A template update preserves the versions used by old Runs; changing a model does not require assigning the worker's history to a new identity.

![A Teloa example: Worker definition, AI employee instance, Run and acceptance](../diagrams/en/architecture.png)

*A Teloa example: Worker definition, AI employee instance, Run and acceptance.*

**The technical path is: standard definition → contract validation → host binding → a Run → fixed artifacts → acceptance records → business adoption.** Each stage answers a different question:

| Layer | Object or stage | What is recorded and checked |
| --- | --- | --- |
| Standard definition | Core definition and work profile | `id`, `revision`, `kind` and fixed member references; responsibilities, Work I/O, dependencies, collaboration and acceptance requirements |
| Host configuration | Contract and capability validation | Whether references exist, I/O contracts are complete and required capabilities are supported; missing requirements leave the affected work not ready |
| Host configuration | Worker instance and bindings | A particular AI employee's persistent identity, plus material, models, application connections and effective authorization for the job |
| Execution | Work, task and Run | Work is a reusable contract; a task is a specific assignment; a Run records the inputs, definition versions and actual executing identity for one execution |
| Delivery | Fixed artifacts | Inspectable content, versions, evidence and actual contributors, beyond a status saying that execution ended |
| Delivery | Acceptance records | Who checked which artifact version against which criteria, and the finding; independent acceptance also checks actual contributors |
| Business decision | Adoption records | Whether the owner decides to use the result; adoption is recorded separately from acceptance and does not grant publication or containment authority |

For example, revision 2 of a document-summary role is a definition. A document assistant created from it is an instance. This week's and next week's summaries are separate tasks, each retaining its Runs and artifacts. Updating the template does not rewrite old Runs or transfer old artifacts to a new employee.

The core format retains two kinds: `agent` and `organization`. A Worker adds the required work profile to `agent`. A Team adds collaboration to `organization`. An Organization also supplies governance. The same reference and revision rules serve all three arrangements.

The host is the platform operating the definitions, such as Teloa. It binds them to actual instances, materials, connections, models and authorization principals, checks inputs and dependencies, and records execution and artifacts. Importing a definition does not connect accounts, grant access, start execution or enable routines. Structural validation, configuration readiness, successful execution, artifact acceptance and business adoption each need their own evidence.

## 05 An example you can check field by field

The [document-summary Worker](../examples/document-worker.json) is a complete definition. Its `summarize` work takes a nonempty `material` string and returns a nonempty `summary` and an `openQuestions` string array. Below is an English rendering of the hand-written [sample input](../examples/document-input.json) and [output](../examples/document-output.json), not a recorded model response:

```json
{
  "material": "Example weekly note: Project A completed three document summaries this week. The second lacks a source date, which the owner requested. The material provides no work-hour or customer-feedback data."
}
```

```json
{
  "summary": "Project A completed three document summaries this week. The second still needs its source date.",
  "openQuestions": [
    "What is the source date of the second document?",
    "The material provides no work-hour or customer-feedback data, so efficiency and satisfaction cannot be assessed from it."
  ]
}
```

Schema validation can catch missing fields and wrong types. The owner still checks whether the content follows the source. It would be unsupported to claim an efficiency improvement from material containing no time data. Acceptance applies to a fixed output revision; changes require the agreed checks again.

The same relationship applies to SOC work. If teaching material contains only one WAF HTTP 403 record, the report should explain what that record supports and identify missing backend or endpoint evidence. It cannot establish compromise or complete safety. Deeper investigation needs the missing data; containment needs appropriate tools and authorization. This example connects to no live environment and performs no attack.

## 06 Mapping Grok Bot and Teloa

Teloa’s AI employee is the product expression of an AI Worker; Grok Bot can also be described as an AI Worker. AI employee and Bot remain the product concepts. AI Worker supplies a shared technical definition, with responsibilities, work contracts and host behavior checked during adaptation. Grok's public design describes persistent Bots and components such as Skills and Routines. It supports a conceptual comparison, not a claim that a converter or official compatibility certification exists. [Grok design](https://x.ai/news/designing-grok-bot)

| Product concept | Standard counterpart | Check during adaptation |
| --- | --- | --- |
| Teloa AI employee; Grok Bot | Worker definition and instance | Persistent identity, responsibilities, Work I/O and acceptance |
| Role prompt; Bot profile | `instructions` and `responsibility` | Concrete boundaries; do not infer missing contracts from names |
| Skills, knowledge, procedures | Resources classified by purpose | Versions, provenance and work dependencies |
| Tools or plugins | Capability references, application resources and host connections | Effective authorization; credentials stay in the host |
| Teloa group and project | Communication and context containers | Explicit responsibilities, handoffs and team output contracts are still needed |
| Grok Team Bot | Shared Worker | Separate user authority, private conversations and personal routines |

Grok's Team Bot documentation describes one Bot used by multiple people. That differs from several Workers collaborating on a deliverable. Personal routines stay in private host arrangements, outside shared definitions. Only deliberately published and sanitized templates can be shared; import still leaves them disabled. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

See the [mapping document](mappings.md) for field correspondence and unsupported cases.

## 07 Reusing existing specifications

OASF supplies discovery records, domains and skill taxonomies. The standard preserves that native metadata and its versions, while Work contracts define inputs, outputs and acceptance. A taxonomy skill and an executable Skill package are different objects. [OASF Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Oracle Agent Spec offers useful component-reference, interface and composition designs. This standard borrows those ideas. Mapping a concrete version still requires converting interfaces, references and execution semantics; renamed fields alone do not establish interoperability. [Oracle Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

JSON carries the normative exchange definition. YAML is an optional authoring input with restricted conversion: reject duplicate keys, anchors, aliases, explicit tags, non-finite numbers and multiple documents, and preserve number and string meanings. `revision: "1"` is a string; `revision: 1` fails the field type check. Both files must not become independently editable sources for the same revision.

## 08 Starting with one real job

Choose an existing job and specify its inputs, outputs, dependencies, responsibility and acceptance. After binding real instances, check the material actually received and the fixed deliverables before expanding to a team. A host that cannot enforce a critical constraint must mark the affected work as not ready.

Industry adoption will need independent interoperability evidence, non-SOC cases and public change discussions. The repository is licensed under Apache-2.0. A concrete ambiguity or counterexample is a useful place to contribute. [Repository](https://github.com/teloa-ai/ai-worker-standard) · [Issues](https://github.com/teloa-ai/ai-worker-standard/issues)
