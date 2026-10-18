# AI Worker: how an employee works and a team delivers

**Max and Morgan, co-founders of Teloa** · 18 October 2026

A name and a role prompt are enough to start experimenting. A second assignment raises more practical questions: is this the same worker as before? Which material did it use? What reached the next member? Who decides whether to accept the result?

The AI Worker draft connects persistent identity, responsibilities, work interfaces and acceptance. It gives products a way to describe what an AI work entity undertakes, and gives users a way to check its deliverables.

This guide explains core `0.1.0-draft.7` and work profile `0.1.0-draft.2`. It is an open draft. Examples are teaching material, not customer results or execution evidence. The [Chinese specifications](../spec/core.zh-CN.md) are normative for this edition.

## What makes up a Worker

Consider a document-summary worker. Its job is to summarize supplied material and list unanswered questions. It can perform this job repeatedly using different models. Each result still belongs to a particular Worker instance and records the definition revision used.

![Five groups of Worker components](../diagrams/en/worker.png)

| Component group | A concrete summary-worker configuration |
| --- | --- |
| Role and responsibilities | Summarize supplied material; report contradictions without inventing facts |
| Methods and evidence | The current input; add Skills, knowledge or procedures when the job needs them |
| Work and arrangements | A `summarize` work interface, invoked on assignment, with no scheduled routine |
| Execution conditions | Read authorized material; obtain separate approval for external publication |
| Experience and deliverables | Return a summary and open questions; the owner checks them against the source |

A memory store, external application and schedule are optional. Dependencies follow the work promised. External research needs a connection and authorization; summarizing supplied text does not need an application added merely to complete a checklist.

A complete Worker definition requires identity and revision, responsibilities and decision boundaries, and at least one work contract with inputs, outputs, a responsible position and acceptance. The five groups organize the explanation; the work profile specifies the fields.

## A team needs traceable handoffs

For a material-review task, an analyst reads the evidence and a reviewer checks the conclusions. The team delivers a report and a review. A group chat supports communication; the team contract also specifies the shared deliverable, responsibilities, handoff conditions and stopping rules.

The composition has its own work interface. For example, `deliver` takes `evidence` and returns `report`, `evidence` and `review`. The coordinator or host must identify who receives the input, which fixed artifact supplies each output and who checks completeness. Without those mappings, the work is not ready.

![Team delivery and acceptance](../diagrams/en/delivery.png)

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

In the diagram, the reviewer contributes a review. The designated owner accepts the complete deliverable. Independence must be checked against actual contributors. A reviewer who edits the report becomes a contributor to that revision and cannot independently accept that same revision.

## What an AI Organization adds

An organization composes teams and workers, with accountability, resource constraints and change rules across them. More members or another group-chat layer does not create those agreements.

![An Organization composes teams and defines governance](../diagrams/en/composition.png)

A security-operations organization could combine an investigation team and a detection-engineering team, with a shared quality reviewer. Sharing requires an explicit binding to the same instance. Two references to one role template do not prove that one employee occupies both slots.

The organization also identifies who is accountable, who appoints or removes members, how paused work is handled, and which data or budget constraints apply to which members. A worker serving two teams does not automatically gain both teams' histories or double a shared budget.

This is an expressible organization structure. A product needs binding, authorization and execution evidence to establish that it operates that structure. It differs from the downloadable structural fixture: that fixture's A and B teams reuse both the analyst and reviewer to illustrate instance bindings, and do not configure a real detection-engineering role.

## Definition, instance and Run

A definition describes a role and its work contracts. An instance is the particular worker in a deployment. A Run is one execution by that worker. A template update preserves the versions used by old Runs; changing a model does not require assigning the worker's history to a new identity.

![Definitions, host bindings, execution and deliverables](../diagrams/en/architecture.png)

The core format retains two kinds: `agent` and `organization`. A Worker adds the required work profile to `agent`. A Team adds collaboration to `organization`. An Organization also supplies governance. The same reference and revision rules serve all three arrangements.

The host is the platform operating the definitions, such as Teloa. It binds them to actual instances, materials, connections, models and authorization principals, checks inputs and dependencies, and records execution and artifacts. Importing a definition does not grant access.

## An example you can check field by field

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

## Mapping Grok Bot and Teloa

Teloa calls its product entity an AI employee; Grok calls it a Bot. This draft uses Worker for a common definition while checking each contract. Grok's public design describes persistent Bots and components such as Skills and Routines. It supports a conceptual comparison, not a claim that a converter or official compatibility certification exists. [Grok design](https://x.ai/news/designing-grok-bot)

| Product concept | Draft counterpart | Check during adaptation |
| --- | --- | --- |
| Teloa AI employee; Grok Bot | Worker definition and instance | Persistent identity, responsibilities, Work I/O and acceptance |
| Role prompt; Bot profile | `instructions` and `responsibility` | Concrete boundaries; do not infer missing contracts from names |
| Skills, knowledge, procedures | Resources classified by purpose | Versions, provenance and work dependencies |
| Tools or plugins | Capability references, application resources and host connections | Effective authorization; credentials stay in the host |
| Teloa group and project | Communication and context containers | Explicit responsibilities, handoffs and team output contracts are still needed |
| Grok Team Bot | Shared Worker | Separate user authority, private conversations and personal routines |

Grok's Team Bot documentation describes one Bot used by multiple people. That differs from several Workers collaborating on a deliverable. Personal routines stay in private host arrangements, outside shared definitions. Only deliberately published and sanitized templates can be shared; import still leaves them disabled. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

See the [mapping document](mappings.md) for field correspondence and unsupported cases.

## Reusing existing specifications

OASF supplies discovery records, domains and skill taxonomies. The draft preserves that native metadata and its versions, while Work contracts define inputs, outputs and acceptance. A taxonomy skill and an executable Skill package are different objects. [OASF Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Oracle Agent Spec offers useful component-reference, interface and composition designs. This draft borrows those ideas. Mapping a concrete version still requires converting interfaces, references and execution semantics; renamed fields alone do not establish interoperability. [Oracle Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

JSON carries the normative exchange definition. YAML is an optional authoring input with restricted conversion: reject duplicate keys, anchors, aliases, explicit tags, non-finite numbers and multiple documents, and preserve number and string meanings. `revision: "1"` is a string; `revision: 1` fails the field type check. Both files must not become independently editable sources for the same revision.

## Starting with one real job

Choose an existing job and specify its inputs, outputs, dependencies, responsibility and acceptance. After binding real instances, check the material actually received and the fixed deliverables before expanding to a team. A host that cannot enforce a critical constraint must mark the affected work as not ready.

Industry adoption will need independent interoperability evidence, non-SOC cases and public change discussions. The repository is licensed under Apache-2.0. A concrete ambiguity or counterexample is a useful place to contribute. [Repository](https://github.com/teloa-ai/ai-worker-standard) · [Issues](https://github.com/teloa-ai/ai-worker-standard/issues)
