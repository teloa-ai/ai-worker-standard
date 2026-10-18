# AI Worker Standard 1.0

**An open definition standard for AI Workers, AI Teams and AI Organizations.**

By **Max Luo and Morgan Chen, co-founders of Teloa** · 18 October 2026

[简体中文](README.zh-CN.md) · [Read the illustrated guide](docs/guide.en.md) · [中文图解](docs/guide.zh-CN.md)

An **AI Worker** is an AI work entity with an identity that persists across tasks, defined responsibilities, an authorization boundary, and deliverables that can be checked. A worker can use several models or temporary agents. It does not need a dedicated machine, a permanent process, or long-term memory.

An **AI Team** combines workers around a shared deliverable, with explicit responsibilities, handoffs and acceptance. An **AI Organization** adds accountability, resource constraints and change governance to a composition of workers and teams.

These terms describe working arrangements. They do not confer legal personality or authority to act on someone's behalf.

![Worker, AI Team and AI Org composition, illustrated with Teloa AI employees](diagrams/en/composition.png)

## What this standard specifies

The core format has two kinds: `agent` and `organization`. The required work profile describes responsibilities, work inputs and outputs, dependencies, acceptance and collaboration. A six-field `agent` document alone is a core definition, not a complete Worker definition.

| Read | Purpose |
| --- | --- |
| [Core specification](spec/core.zh-CN.md) | Identity, immutable revisions, composition, capability references and eight common rules |
| [Work and collaboration profile](spec/work-profile.zh-CN.md) | Work contracts, handoffs, private routines, instance bindings and governance |
| [Illustrated guide](docs/guide.en.md) | Architecture and a worked example |
| [Product mappings](docs/mappings.md) | Teloa, Grok Bot, OASF and Oracle Agent Spec |
| [Examples](examples/README.md) | Complete Worker JSON/YAML, composed definitions and sample input/output |
| [Conformance boundaries](CONFORMANCE.md) | What an implementation can claim and what evidence it needs |
| [Governance](GOVERNANCE.md) | Maintenance, feedback, changes and licensing |

**Version 1.0:** core and work profile; the profile is identified by `urn:teloa:profile:ai-work`. The normative specification is currently in Simplified Chinese. English guides are explanatory; a normative English edition requires a separately reviewed publication.

JSON is the normative exchange format. YAML is an optional authoring format with a restricted conversion profile. Importing either format does not install tools, connect accounts, start a worker, enable routines or authorize execution.

## Where existing products fit

Teloa’s **AI employee** is the product expression of an **AI Worker**. People create AI employees and form AI teams; Worker definitions and instances describe their identity, responsibilities and work contracts, while Team contracts describe collaboration and delivery. Grok’s **Bot** can also be described as an AI Worker. A conforming mapping requires the definitions and host behavior to satisfy the contracts; the products retain their own names. Grok's **Team Bot** is a shared Bot: it is not, by its name alone, a team of multiple AI Workers. The [mapping guide](docs/mappings.md) explains the distinction and the checks still required.

OASF contributes discovery metadata and taxonomies. Oracle Agent Spec contributes useful ideas for component references, interfaces and composition. Reusing these ideas is not a claim of complete format or runtime compatibility. The mapping guide links the primary sources and records the boundaries.

## Status and participation

This is a **Teloa-maintained open standard**, not an adopted international standard or a certification scheme. There is no claim of endorsement by xAI, AGNTCY, Oracle or a standards organization. Current examples illustrate the contract; they are not a deployment package or proof of cross-platform interoperability.

The next interoperability milestone is reproducible exchange testing by two independent implementations, including a non-SOC use case. Contributions should report the exact specification/profile versions, the behavior tested and unsupported requirements.

Use [Issues](https://github.com/teloa-ai/ai-worker-standard/issues) for a concrete ambiguity, counterexample or implementation report. Submit wording changes with the affected section and expected behavior. See [GOVERNANCE.md](GOVERNANCE.md).

## License

Original material in this repository is licensed under [Apache-2.0](LICENSE). Product names and linked third-party specifications retain their respective rights. No product account or Teloa service is required to read or implement this standard.
