# AI Worker Work and Collaboration Profile

[Languages / 语言](../docs/languages.md)

**Max Luo and Morgan Chen, co-founders of Teloa**

> This is a complete English translation of the [authoritative Simplified Chinese profile](work-profile.zh-CN.md). It has not received a full review by native-speaking industry experts. If the texts conflict, the Simplified Chinese text governs; please report translation issues. Any code examples retain their original Chinese values so that examples remain identical across languages.

Date: 2026-10-18. Version: `1.0`. A normative appendix to the [AI Worker, AI Team and AI Organization Open Definition Standard 1.0](core.en.md). It defines work semantics and adaptation requirements, not a new execution engine.

## 1. One contract connecting Worker, Team and Organization

An employee provides work entry points; a team provides its own work entry points and explains how its members share responsibilities; an organization composes teams in turn. All three specify inputs, outputs, dependencies, responsibility and acceptance. An organization does not automatically expose all its members’ work, and joining an organization does not automatically share personal memory.

The work profile appears as a required extension: `id = urn:teloa:profile:ai-work`, `version = "1.0"`, `required = true`. This is the specification’s fixed identifier, not an authentication or online service endpoint. Any host may implement it; a Teloa account is not required. Users MUST NOT change it to optional to bypass critical semantics. A file containing only the core agent fields can claim only core exchange support, not complete AI Worker definition conformance.

The core specification’s rules for non-empty strings, LocalId, fixed references, side-effect-free import and unknown extensions continue to apply. The structures below allow only the listed fields; all are required unless marked optional. Use an explicit empty array when nothing applies; null MUST NOT be substituted. Each type of id is unique only within its owning array, and reference locations are interpreted as specified in this appendix.

## 2. Structure of extension data

| Field | Type | Purpose and default |
| --- | --- | --- |
| responsibility | Responsibility | Judgment rules for a role or composition |
| resources | Resource[] | Catalog of methods, materials, applications and policies; may be empty; listing an item does not mean it must be enabled globally |
| works | Non-empty Work[] | Work entry points promised externally |
| requires | LocalId[], optional | resources required by all work; omission means empty |
| routines | Routine[], optional | Routine arrangements; omission means empty; not automatically enabled |
| authorityRoles | AuthorityRole[], optional | External approval, acceptance or accountability roles; omission means empty |
| memoryPolicyRef | LocalId, optional | References a resource with purpose=policy; counts as a global dependency when present; omission means no claim of cross-task memory capability |
| collaboration | Collaboration | Required for organization; MUST NOT be used by agent |
| governance | Governance, optional | Available only to organization; required when claiming support for this edition’s AI org definition |

**Responsibility** contains five required string arrays: `triggers, autonomousActions, confirmationPoints, escalationRules, deliveryChecks`. They describe when to take on work, autonomous actions, confirmation points, escalation rules and delivery checks, respectively. triggers, autonomousActions and deliveryChecks MUST be non-empty. The other two may be empty when there genuinely are no additional business confirmation or escalation rules. An empty confirmation array does not waive host authorization; an empty escalation array does not permit execution to continue without required inputs or permissions. Descriptions do not grant tool permissions. Core instructions describe continuing responsibilities; Work and resources provide the work details. Whether the text has substantive meaning still requires content review; non-empty strings alone cannot establish that.

**Resource** contains `id:LocalId`, `title:non-empty string`, `purpose`, `binding`, and optional `requires:LocalId[]`. purpose is `method | knowledge | procedure | application | runtime | policy | trigger | template-reference`. These are resource-purpose labels, not mandatory new product resource types. binding contains `kind` and `config`, reusing the core Capability’s four binding kinds and their config rules without redefining the protocols.

Resource.requires references only resources in the same definition; omission means empty. Fixed material SHOULD preferably include a digest; mutable sources MUST have the read version fixed during actual execution. Methods may reference complete skill packages, and SOPs may reference method or procedure resources. Read-only application descriptions, model configurations or policy files MUST NOT be labeled as connected applications, satisfied runtime conditions or enforced policies.

For runtime, policy, trigger or application described as a resource, the adaptation report MUST record its native format, exact version and explicit handler. The handler may be a host feature supporting that format or an explicitly assigned human step. Textual instructions with no party responsible for carrying them out do not satisfy a required condition. This convention preserves existing native formats rather than introducing a general permission or scheduling language.

**AuthorityRole** contains `id:LocalId`, `name:non-empty string`, `kind:human | service`. It is a responsibility slot, not an account. The host MUST bind a trusted principal. service means an external service principal verified by the host; it does not mean a model may appoint itself as approver. When human approval is required, human MUST be declared. The acceptance, escalation or governance contract that references a role determines its function.

## 3. Inputs, outputs and delivery for each work item

| Work field | Type | Meaning |
| --- | --- | --- |
| id, title, instructions | LocalId, non-empty string, non-empty string | Work entry point, name, and objective/method requirements |
| inputs, outputs | JSON Schema objects | Describe the complete input object and complete output object, respectively |
| requires | LocalId[] | resources required for this work; may be empty |
| responsible | ParticipantRef | One primarily responsible position |
| acceptance | Acceptance | Completion checks and acceptance responsibility |

`ParticipantRef` is `self`, `member/<LocalId>` or `authority/<LocalId>`. self refers to the instance corresponding to the current definition. member refers only to a direct Member slot of the current organization, including a consultant participating per task. authority refers to the current data.authorityRoles. agent MUST NOT use member references. All references MUST exist and MUST NOT be guessed from names. responsible MUST be self or member; external people may handle acceptance and escalation but are not disguised as AI executors. When an organization uses self, it means the composition is responsible for external delivery. The host MUST still record the actual executors and the parties responsible for input distribution and output aggregation; the organization’s nominal identity MUST NOT replace actual contributors.

**Acceptance** contains `criteria:non-empty string array`, `reviewer:ParticipantRef`, `independent:boolean`. criteria gives checkable completion requirements; reviewer identifies the acceptance responsibility position. When independent=true, the host MUST demonstrate that the actual reviewing principal differs from the actual executing principal behind the deliverable under review. Composite deliverables require checking actual contributors, not merely comparing primary responsible parties or nominal team identities. Two slots, two conversations or two names do not prove independence. Independent principals do not automatically establish judgment quality either. Acceptance records fix the deliverable version assessed; successful structural validation, model completion and business adoption are recorded separately.

The roots of inputs and outputs MUST have `type:object` and declare `$schema = https://json-schema.org/draft/2020-12/schema`. The standard vocabularies of [JSON Schema 2020-12](https://json-schema.org/draft/2020-12) are reused instead of defining a new type language. A Schema may contain descriptions and `$defs`. All references, including `$ref` and `$dynamicRef`, MUST target the same Schema document; nested `$id` MUST NOT change the reference document boundary. Import does not resolve references over the network. Implementations MUST report unsupported vocabularies or constraints and MUST NOT ignore them while claiming validation has passed. If format is used for critical acceptance checks, the corresponding format-checking capability MUST be explicitly declared and verified.

Missing inputs are reported as requiring supplementation. Missing outputs or type mismatches prevent a claim that the deliverable conforms to its contract. Strings are not implicitly converted to numbers, decimals are not truncated, and false is not converted to 0. Schema default is descriptive; it does not automatically mean that the user supplied or authorized that value. Free-text deliverables may occupy string fields; acceptance still checks business evidence and judgment quality.

**Dependency checks:** First satisfy core capabilities and their R05/R06 conditions. Then take the union of data.requires, memoryPolicyRef, governance.changePolicyRef when present, and the selected Work.requires, recursively expanding Resource.requires. Missing references or resource-dependency cycles are definition errors. A missing connection, permission or interpreter makes the corresponding work not ready; resources exclusive to other work that remain unconfigured do not block the current work. Missing globally required dependencies still block all execution. Parsing support, configuration readiness and actual authorization are reported separately; partial availability MUST NOT be described as readiness to run everything.

## 4. Routines and experience

**Routine** contains `id:LocalId`, `work:LocalId`, `policyRef:LocalId`, and optional `triggerRef:LocalId`. work references the current works; policyRef references a resource with purpose=policy, and triggerRef references one with purpose=trigger. The executing role belongs to the instance bound to the current definition. The host separately binds the creator, schedule owner and actual authorizing principal; they need not be the same as the Worker identity. Import MUST NOT create another employee, widen routine visibility or transfer a routine to another member.

Omitted triggerRef means scheduling is pending. Even when present, it describes only a trigger definition; the instance is disabled by default. The host handles trigger formats. Teloa’s existing daily/weekly or local-event support is claimed only within its actual scope; arbitrary cron schedules or external events MUST NOT be silently approximated. Validate the actual work and actual trigger first, then let the deployer enable it.

A routine policy MUST specify the input window/cursor, behavior for no input and invalid input, trigger deduplication and business-processing bookkeeping, retries, concurrency/catch-up, budget, notifications and pause conditions. Arrangements with missing requirements may be retained but MUST NOT be enabled. Successful triggering is not successful processing. Business-processing state advances only when the agreed deliverable state is achieved; retries MUST NOT bypass business deduplication by using a new trigger identifier. There is no universal fixed retry count or industry rule of “two attempts without progress.”

memoryPolicyRef defines only rules for preserving experience, provenance, scope, review and withdrawal; it carries none of the author’s private memory. A template-reference resource is template material with provenance. Historical logs MUST NOT masquerade as firsthand experience of a new instance. Personal, business, project and group contexts of a shared employee remain separated according to host authorization. One employee instance participating in several teams does not make those teams’ histories mutually readable.

## 5. Team collaboration and organizational governance

| Collaboration field | Type | Meaning |
| --- | --- | --- |
| roles | RoleAssignment[] | Covers every direct Member exactly once |
| coordinator | ParticipantRef, optional | If provided, may reference only a current direct Member slot; omission does not automatically select someone |
| handoffs | Handoff[] | Explicit relationships for transferring responsibility and materials; may be empty |
| sharing | Sharing[] | Intended recipient positions for shared resources; may be empty |
| stop | StopPolicy | Rules for bounded collaboration, lack of progress and escalation |

**RoleAssignment** contains `member:LocalId`, `duty:non-empty string`, `participation:member | consultant`. duty describes the division of responsibilities within this composition without rewriting the member’s original responsibilities; conflicts stop affected work. consultant means a consultant participating per task, without automatically joining a group or receiving its history. Fixed definition references for both types of slot are resolved under the core rules. Instance binding and task authorization establish their participation scope.

Both coordinator and Work.responsible may reference a consultant slot, but assignment MUST be explicit and participation in the current work MUST be confirmed. The consultant still participates only per task and does not thereby take over the team indefinitely or acquire its complete group history. `member/` is syntax for referencing a Member slot; it is not the same concept as the member value of participation.

**Handoff** contains `id:LocalId`, `from:WorkRef`, `to:WorkRef`, `when:non-empty string`, `inputs:object`. WorkRef permits only `member/<memberId>/<workId>`, referencing declared work of a direct member of the current composition; the member definition version comes from Member.ref. It expresses a handoff from a member work item that has produced a deliverable to a receiving member work item. It does not support `self/<workId>` or mix composition entry/exit points into the same reference semantics. Subteams expose their own works externally without direct access through to their internal members.

The keys of Handoff.inputs are top-level field names in the receiving work’s input object, and the values are top-level field names in the sending work’s output object. Both are interpreted as original strings; expressions, scripts and dot paths are not executed. Fields MUST be declared in the respective Schema.properties; copied values MUST also pass actual validation against the receiving Schema. Other inputs required by the receiving work MUST be supplied explicitly. Until complete, report the gaps rather than fabricate a successful receipt. when is a business condition. Handoff begins only when explicitly assigned or confirmed by a supported trigger mechanism; text alone cannot establish implemented listening behavior.

The host’s native orchestration or an explicitly assigned coordinator handles input distribution and output aggregation for a composition’s Work. This edition does not introduce another entry/exit mapping language. The adaptation report MUST specify which composition input reaches which member work, who supplies each required output and from which fixed deliverable, and who checks completeness. Static definition validation does not establish that these mappings have executed. Until mappings and handlers are determined, the corresponding composition work MUST NOT be marked ready. A composite result MUST pass its outputs Schema and acceptance; arbitrary member outputs MUST NOT automatically be concatenated into a team deliverable.

Each actual handoff fixes the source work/Run, deliverable version, sending and receiving principals, mapped materials, permitted scope, and acceptance/rejection result. Handoff is complete only after the receiver confirms receipt and work ownership is established. Transfer only materials required by the contract and already authorized; do not implicitly transfer an entire conversation, private memory or connection credentials. Sending a group message and completing a handoff are recorded separately.

**Sharing** contains `resource:LocalId`, `recipients:non-empty ParticipantRef array`. resource references the current resources; recipients may only be direct Member slots of the current composition, written as `member/<LocalId>`, including consultant slots. Visibility and the specific version still require host authorization. sharing grants no permissions and does not automatically make a shared resource a required dependency of every member. If work needs it, the receiving work’s inputs or dependencies MUST explicitly accommodate it.

**StopPolicy** contains `maxHops:positive integer`, `maxNoProgress:positive integer`, `escalateTo:ParticipantRef`. Counters apply to the same work and its derived handoff chain; switching employees or retrying does not reset them. maxHops limits the number of handoffs. maxNoProgress limits consecutive rounds with no new verifiable evidence or no improvement in declared acceptance items. Reaching either limit stops further delegation and triggers escalation. Resumption requires explicit handling by a trusted principal and a new record. Values are configured for the scenario, not used as employee capability scores.

**Governance** contains `accountable:ParticipantRef`, `changePolicyRef:LocalId`. accountable is the organization-wide accountability position. changePolicyRef references a resource with purpose=policy and automatically counts as a global dependency. The policy MUST specify member appointment/removal, definition and constraint changes, pausing and handling of work in progress. Organization-wide data, budget and execution restrictions are expressed through policy/runtime resources in data.requires. Inherited constraints MUST explicitly identify applicable members and scope and be verified by the host. Constraints cannot automatically expand member permissions; conflicts MUST NOT be resolved by allowing the less restrictive option.

AI Team uses a collaboration contract; an AI Organization definition in this edition MUST also include governance. Both use the same organization core type and are not distinguished by headcount. New organizational powers, actual member management or cross-organization identity systems are not automatically established by this definition specification.

When claiming complete Team or Organization definition conformance, every agent in the member-reference closure MUST satisfy the AI Worker definition contract, and every organization MUST provide this appendix’s collaboration contract. Not every subteam must declare governance. Temporary Agents with only core fields may be invoked under authorization through a Worker’s execution mechanism but MUST NOT masquerade as persistent members conforming to this edition. External human or service acceptance positions are expressed through authorityRoles.

## 6. Host bindings and actual execution

Binding records are stored in the deployment environment. Hosts need not use the same database or JSON structure, but the following information MUST be checkable:

| Record | Information that must be distinguished |
| --- | --- |
| Definition location | Root definition id/revision, the slot path formed by successive Member.id values, and the fixed definition at the path’s end |
| Instance selection | Explicit creation or reuse, actual instance identity and version, and owning trusted principal; no creation during import |
| Identity continuity | Instance identity traceable across tasks; separate identities for models, conversations, temporary Agents and Runs; checkable enablement, pausing, retirement and authorization to resume |
| Task and authorizing principals | Separate records for managing principal, task initiator, routine owner and resource-authorizing principal; sharing a Worker does not mean sharing all of one person’s authorizations |
| Reuse relationship | Whether multiple slots bind to the same instance; sharing in this edition must correspond to the same fixed definition and cannot be inferred from matching names |
| Authority roles | The trusted principal and human/service type corresponding to each authorityRole; approver identity cannot come from a model’s self-identification |
| Execution conditions | Selected work, resources and native formats/versions, actual connections, valid authorization, model and resource limits |
| Versions and changes | Binding version, adopted division of member responsibilities, constraint and material versions, and handling of pending work after changes |

Two instances generated from the same template and one instance serving across teams MUST have different records. Group visibility and execution authorization for a shared instance are checked per work. Budget accounting uses the actual applicable shared allowance; repeated binding MUST NOT produce two allowances where only one exists.

An execution MUST be traceable to the root definition, selected work, actual executing principal, material and resource versions, valid authorization and deliverable version. Member replacement or template upgrades do not silently take over old Runs. Resumption or termination of an existing execution, or creation of a new task, follows an explicit change policy. If the host cannot fulfill a critical constraint, it preserves the definition and reports the blocker rather than demoting the constraint to a note and continuing execution.

Work inputs, retrieved material, historical messages and tool outputs are work materials with provenance, not trusted instructions to change a Worker definition or expand permissions. The host MUST distinguish these materials from authorization sources. A Worker may invoke temporary Agents, but external deliverables remain attributable to the selected Worker; temporary executors do not automatically become Team members. Independent review checks actual contributors and delegation relationships; a new conversation or temporary assistant MUST NOT manufacture an independent identity for reviewing one’s own deliverable.

Execution records distinguish at least work success, failure, partial completion, unknown result and pause; hosts may use their own state names. Partial output or a timeout response MUST NOT automatically be marked as work success. When an external side effect has an unknown result, the original operation’s status MUST be checked before deciding to retry. Task requirements, acceptance conditions, results and limitations define the quality-evaluation scope; this profile prescribes no universal industry score or “pass once to qualify for a role” rule.

## 7. Practical boundaries of adopting OASF and Agent Spec

OASF discovery is carried in a separate extension. It uses the native discovery Schema URI at a fixed version as id, the same schema_version as version, and the unchanged native payload as data. For discovery information only, required=false. When used as a non-ignorable execution-selection condition, required=true MUST be declared and its semantics verified; classification itself still grants no permissions. This uses the Extension wrapper defined by this specification, which differs from the AgentExtension wrapper in an A2A Agent Card. Both formats can carry the native OASF discovery payload, but their wrapper fields are not interchangeable.

For example, the native payload’s `schema_version`, `skills` and `domains` keep their original names and meanings. skills denotes capability categories and domains denotes domains. OASF classification validation may be claimed only for entries that actually exist in the fixed catalog. A host may use trusted fixed Schema/taxonomy snapshots offline; importing a URI does not automatically access a service. If no corresponding classification exists, retain an unmapped explanation rather than inventing an official identifier. [OASF discovery](https://docs.agntcy.org/oasf/discovery-extension/)

OASF record metadata and locators may be export views. Source, version and unmapped fields enter the adaptation report; they cannot replace this specification’s fixed revisions or actual instance identities. The source definition remains the single source of truth; a derived view does not become a second independently editable runtime configuration. [Record Guide](https://docs.agntcy.org/oasf/agent-record-guide/)

Agent Spec’s ideas for references, inputs/outputs and composition inform this edition’s Work and Handoff. Its `$component_ref` is a reference within a configuration; export MUST explicitly map it to this specification’s fixed definitions/resources. Its input/output property lists require conversion to this edition’s object Schemas; renaming fields alone is insufficient. Mapping support for the selected version/subset is claimed only when interfaces, constraints and execution semantics are fully preserved. [Agent Spec 26.1.2](https://oracle.github.io/agent-spec/26.1.2/agentspec/language_spec_26_1_2.html)

Swarm and ManagerWorkers may be adaptation targets for matching teams. Conversation transfer, whether working members can face the user directly, and message flow MUST each be checked. The current contract does not require Teloa to switch to that execution engine or force every team into a manager–worker structure.

## 8. Coverage of the existing design

| Existing element | Representation in this profile |
| --- | --- |
| A01 Role identity and mission | Core id/revision/name/instructions; display information such as avatars is retained in non-critical extensions |
| A02 Responsibility and judgment | The five responsibility field groups; actual authorization is in host bindings |
| A03 Skills | resources with purpose=method, referencing complete method packages or native method resources |
| A04 Knowledge | purpose=knowledge, retaining provenance, version or digest and referenced per work |
| A05 SOP | purpose=procedure and its resource dependencies; collaboration expresses cross-role handoffs |
| A06 Work templates | works objectives, I/O, dependencies, responsible positions and acceptance |
| A07 Routines | routines, native triggers and execution policies; the host records unconfigured/disabled states |
| A08 Application connections | purpose=application, native bindings and actual connections; catalog declarations do not establish successful installation |
| A09 Permissions | Required policy resources, work scope and trusted host authorization; credentials are not exported |
| A10 Runtime and workspace | runtime resource requirements and host bindings for models, budgets and file scope |
| A11 Experience | memoryPolicyRef and source references; actual private experience remains with the instance |
| A12 Deliverables and quality | outputs, acceptance and execution evidence; structure and business quality are checked separately |
| T01 Shared goal | organization.instructions and externally exposed works |
| T02 Role responsibilities | Member, collaboration.roles, Work.responsible and authorityRoles |
| T03 Shared context | sharing, work inputs and scoped host authorization |
| T04 Collaboration and handoffs | handoffs, input mapping, receipt confirmation and stop |
| T05 Work arrangements | Member or composition routines, procedure and coordination responsibilities |
| T06 Permissions and resources | Global/work requires, members’ original permissions and actual host allowances |
| T07 Team acceptance | Composition outputs/acceptance, member contributions and fixed deliverable versions |

The current community repository’s RoleDefinition, GroupDefinition, RoleWorkDelegation, RunRoleSnapshot and resource-package relationships are adaptation targets, not replacements with new product objects. Teloa’s “AI employee” is the product form of a Worker. RoleDefinition, a fixed role version and an actual Role instance all participate in the mapping; renaming alone cannot prove conformance. role-coordination describes only static coordination relationships. Creating groups, delegating work and automatic handoffs each require actual support and authorization.

Teloa’s `kind: employee | twin` denotes product role types. Both may map to Worker if they meet the same work contract; twin grants no right to approve on the represented person’s behalf. The original kind is retained in the adaptation report or a non-critical product-display extension; it MUST NOT be written directly into core kind. `scopes`, together with dataScope/executionScope, maps to required scope policies and host authorization bindings. When a business scope cannot be matched across hosts, related work remains not ready. These are critical restrictions and MUST NOT be stored only in ignorable display extensions. The fields here have been checked against the community repository’s `packages/contract/src/roles.ts`; this does not mean a converter has been implemented.

In public Grok recipes, profile maps to the role description, skills to methods, routines to work and routines awaiting scheduling, plugins to applications awaiting configuration, and memory to template material with provenance. Profile-type work guidelines and log-type author logs are kept separately. gettingStarted is only an entry hint. Missing responsibilities, schedules or acceptance information are reported, not guessed from a name or the original author’s experience. The adaptation report links complete originals and their digests. Critical semantics MUST NOT be hidden in ignorable extensions. Grok Team Bot maps to a shared Worker; its name does not automatically produce a multi-Worker Team.

Grok Team Bot’s public documentation assigns personal routines to their creators, and connections may operate using the current user’s or the Bot’s credentials. Adaptation MUST therefore preserve routine ownership, execution identity and material visibility instead of merging everyone’s routines into all-member tasks of the shared Worker. This is a mapping requirement for the source behavior; it does not require other hosts to copy its default permissions. [Team Bots](https://docs.x.ai/grok-bot/team-bots)

**Where private routines belong:** Personal routines map only to host-side arrangements isolated by owner and visibility, referencing a Work already declared by the Worker. They MUST NOT enter a shared definition’s routines or be exported for sharing. The adaptation report marks them as “preserved as private instance arrangements, excluded from the definition”; the report itself must also respect their visibility scope. A template may enter the definition’s routines only after the original author explicitly publishes it as a template and removes private content. Import still leaves it disabled by default. If ownership, visibility or the work reference cannot be preserved, report the mapping as unsupported; the routine MUST NOT be merged or silently lost.

An adaptation report lists at least the source format/version/digest, target definition revision, field mappings, preserved/converted/unsupported items and the work affected by unsupported items. Keeping an original does not establish successful semantic adaptation. If conversion cannot fulfill permission, handoff or acceptance conditions, it MUST block affected work.

## 9. Positive and negative examples and completion claims

The [complete composition example](../examples/composition-fixture.json) uses a fixed local definition collection to express two employees, two teams reusing both employee definitions, a nested organization, and expectations for host binding and work readiness. The collection wrapper and descriptions are a test-fixture format, not a new distribution protocol or a resource package directly importable into Teloa.

| Check | Required conclusion |
| --- | --- |
| Simple role with no skills, memory or routines | Can be defined; explicit work, I/O, responsibility and acceptance are still required |
| Only a name, prompt and kind=agent | May conform to core exchange; MUST NOT claim a complete AI Worker definition on that basis |
| Empty triggers, autonomous actions or delivery checks | Does not conform to this edition’s work appendix; a product name cannot supply the missing content |
| The same Worker starts two tasks or changes models | The same Worker instance and two separate Runs MUST be traceable; two conversations alone do not demonstrate identity continuity |
| The same employee definition appears in two teams | Demonstrates definition reuse only; slot bindings determine whether it is the same instance |
| One work item lacks a dedicated application while another has complete inputs | The first is not ready; the second still checks global conditions and its own dependencies and is not automatically blocked by the first |
| Missing suborganization version, cycle or unknown required Profile | Stop assembly and report the specific reference; do not substitute versions or ignore it |
| Wrong receiving field, missing required input or output type mismatch | MUST NOT mark handoff as successful or the deliverable as conforming; do not automatically fill values or convert types |
| Two review slots bound to the same actual executing principal | independent=true is not satisfied |
| Consultant participating in only one review | Does not automatically join all groups, read all history or get duplicated as a new employee |
| Schedule missing a trigger or having an incomplete policy | Preserve as a not-ready arrangement; do not enable |
| Unknown OASF category or non-equivalent Agent Spec collaboration pattern or interface constraints | Report unsupported behavior/mapping differences; do not claim comprehensive compatibility |

This profile specifies structural and behavioral contracts. Manuscript checks, example checks, actual connection/model execution, independent-implementation interoperability and industry adoption are reported separately. See the [conformance statement](../CONFORMANCE.md) for verification scope and implementation-report requirements.
