# Conformance and evidence

This repository publishes a definition contract. It does not certify products or provide a complete conformance implementation.

| Claim | Required evidence | Does not establish |
| --- | --- | --- |
| Core exchange | The core's T01–T06 cases, correct revision/reference handling, error reports and lossless JSON round trips | A Worker has been configured or can run |
| Worker definition | Core plus the required work profile; meaningful responsibilities and at least one complete Work contract | Actual identity, application access or output quality |
| Team / Organization definition | Closed member references, collaboration contracts; governance for an Organization | Dispatch, live handoffs or enforcement |
| Host readiness | Fixed definitions bound to actual instances, supported handlers, required resources and effective authorization | A run succeeded |
| Execution | Fixed inputs and resource versions, actual contributors, handoff receipts, outputs and acceptance against that output version | Other inputs, customers, environments or external operations succeeded |
| Independent interoperability | Reproducible reports from independent implementations, exact versions and preserved semantics | Universal runtime portability |

## Initial publication checks

The supplied examples are checked for the documented subset: definition shape and references, required work profile, work schemas, member handoff fields, resource references and example input/output. JSON and YAML Worker files must represent the same JSON value. These are editorial example checks, not a reusable validator or full core exchange test suite.

The composition fixture deliberately includes unresolved application and host-handler bindings. It must remain **not ready for live execution**. A composed definition and an illustrative binding record cannot prove that a receiver obtained the material or that a reviewer is independent.

For the runnable subset of a host, verify the actual mapping from composite inputs to member work, each required composite output to a fixed artifact, and the named handler checking completeness. If that mapping is missing, report the work as not ready.

Independent review requires a reviewer distinct from the actual contributors to the reviewed artifact. Two names, role slots or sessions are insufficient. If a reviewer changes the deliverable, treat them as a contributor when reviewing that revised artifact.

## Reporting template

An implementation report should include:

1. Core and profile versions, implementation and version, and the exact claim being made.
2. Source definitions and immutable revisions; supported bindings, extensions and unsupported requirements.
3. Inputs, expected results and observed results, including rejection cases and round-trip preservation where applicable.
4. Host readiness evidence, if claimed: instance bindings, resource handlers and authorization checks, with secrets removed.
5. Execution evidence, if claimed: actual contributors, fixed artifact references, handoff outcomes and acceptance. Record business adoption or external actions separately.

## 中文边界

“定义完整”“宿主已就绪”“一次工作成功”“业务负责人已采用”是四种不同结论。声明范围要对应证据；本仓的示例检查不能代替真实连接、授权、模型执行、独立复核或跨平台互通。个人例行留在宿主私有安排中，导出共享定义时不得带出。
