# Maintenance and changes

This standard is maintained by Teloa. Its founding authors are **Max and Morgan, co-founders of Teloa**. The maintainers review contributions openly; maintenance by Teloa does not imply industry consensus.

## Proposing a change

Open an issue describing the affected section, a concrete example, the current interpretation and the proposed behavior. Implementation reports should name the exact core/profile versions, supported subset and evidence that another implementer can reproduce. Do not include credentials, private conversations, customer data or internal operational logs.

A pull request should distinguish editorial changes from changes to structure or behavior. Maintainers review contract changes against examples, acceptance and rejection cases, and compatibility. An unresolved semantic question remains open; merging an explanatory article does not resolve it.

## Version policy

- The current normative documents are the two Chinese specifications linked from the README. Explanatory guides, diagrams and examples do not override them. Report a contradiction as an issue.
- The core and each extension have their own exact version. A structural or semantic change to an extension requires a new extension version. A change to core interpretation requires a new core version.
- A definition's `id` and `revision` identify immutable content. An adapter must preserve the old definition and create a new revision when its JSON value changes. Specification upgrades must be explicit; no automatic migration or implicit permission expansion is permitted.
- Editorial fixes may clarify wording without changing accepted behavior. Fixes that alter acceptance or rejection behavior are semantic changes even if only one sentence changes.
- Each published Git tag is an immutable snapshot. Later work goes into new commits and releases; never replace a published tag to hide a correction.
- Breaking changes require an explicit specification version, migration instructions and updated examples. Implementations must declare the exact versions they support.

## Participation and claims

Contribution does not require a Teloa account, subscription or deployment. Contributions are submitted under the repository's Apache-2.0 license. Third-party material must be identified and licensed for inclusion.

There is currently no certification program or conformance logo. An implementation report must state its scope under [CONFORMANCE.md](CONFORMANCE.md). Adoption by one product, an automated review or an example check is not evidence of industry consensus.

## 中文说明

本仓由 Teloa 维护，发起作者为 Teloa 联合创始人 Max、Morgan。任何人均可提交问题、反例和实现报告。变更须区分文字澄清与语义变化；改变接受或拒绝结果的修订必须升级相应规范版本。已发布标签和旧定义保持不变。贡献按 Apache-2.0 许可提交，不得包含他人私有数据或未经许可的材料。

## Scope of nested scheduling

Version 1.0 defines StopPolicy counting within a work and its derived handoff chain. Combining multiple enclosing policies in a nested composition is outside this contract. Implementations must disclose how they handle that boundary; a definition-conformance claim does not establish interoperable nested scheduling.
