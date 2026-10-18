<p align="center">
  <a href="https://www.teloa.ai/en/"><img src="assets/brand/teloa-reading.webp" width="120" height="120" alt="Teloa the squirrel reading"></a>
</p>

# AI Worker Standard 1.0

**An open definition standard for AI Workers, AI Teams and AI Organizations.**

By **Max Luo and Morgan Chen, co-founders of Teloa** · 18 October 2026

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

[Website](https://www.teloa.ai/en/) · [Docs](https://docs.teloa.ai/en/) · [Marketplace](https://market.teloa.ai/en/) · [AI Security](https://security.teloa.ai/?lang=en) · [Blog](https://www.teloa.ai/en/blog/) · [Security reports](https://security.teloa.ai/report/?lang=en)

[![Version 1.0](assets/badges/version.svg)](https://github.com/teloa-ai/ai-worker-standard/releases/tag/v1.0) [![8 core rules](assets/badges/core-rules.svg)](spec/core.en.md) [![8 languages](assets/badges/languages.svg)](docs/languages.md) [![License Apache-2.0](assets/badges/license.svg)](LICENSE)

An **AI Worker** is an AI work entity with an identity that persists across tasks, defined responsibilities, an authorization boundary, and deliverables that can be checked. In Teloa, the product name is **AI employee**; AI Worker is the underlying standard concept. Grok Bot can also be described using this concept, subject to the mapping checks below.

## 1. Core concepts

| Concept | What it describes | What makes it useful |
| --- | --- | --- |
| AI Worker | An identifiable work entity with responsibilities, work contracts and delivery checks | Work remains attributable across tasks, models and temporary agents |
| AI Team | Members working toward a shared result with explicit roles, handoffs and acceptance | Collaboration has a checkable delivery contract |
| AI Organization | A composition of workers and teams with organizational accountability, resource boundaries and change governance | Teams can work together under explicit governance |

Persistent identity does not require a dedicated machine, a permanent process or long-term memory. Accountability here means attribution and traceability of work, not legal personality. A chat group or member list alone is not a Team contract.

## 2. Scope and format

| Item | Version 1.0 |
| --- | --- |
| Core objects | `agent` and `organization` |
| Required work profile | `urn:teloa:profile:ai-work`, version `1.0` |
| Worker / Team / Organization | Worker: core + work profile; Team: add collaboration; Organization: add governance |
| Exchange | JSON; optional YAML authoring under the restricted conversion rules |
| Core checks | Eight common rules, R01–R08; six core exchange cases, T01–T06 |
| Authority | Simplified Chinese core and work profile; full reading translations in seven other languages |

The standard defines identities, immutable revisions, references, responsibilities, work interfaces, collaboration and governance. Hosts implement model execution, scheduling, connections and authorization. Importing a definition does not install tools, connect accounts, start work or enable routines. A six-field core `agent` document alone is not a complete AI Worker definition.

## 3. Read the standard

| Document | Purpose |
| --- | --- |
| [Guide with diagrams and searchable text](docs/guide.en.md) | Composition, architecture, delivery workflow and a worked example |
| [Full core specification](spec/core.en.md) | Fields, fixed revisions, references, common rules and exchange cases |
| [Full work and collaboration profile](spec/work-profile.en.md) | Work contracts, resources, handoffs, routines, instance bindings and governance |
| [Examples](examples/README.md) | Complete Worker JSON/YAML, composition fixture and sample input/output |
| [Product and specification mappings](docs/mappings.md) | Teloa, Grok Bot, OASF and Oracle Agent Spec |
| [Conformance](CONFORMANCE.md) · [Governance](GOVERNANCE.md) | Evidence boundaries, contribution and maintenance |
| [All eight languages](docs/languages.md) | Complete reading editions and translation policy |

Each guide explains the diagrams in text. Full translations retain field names, rule identifiers and JSON examples. Translations are reading editions; they have not undergone complete review by native-language domain experts. The Chinese normative documents resolve interpretation differences. Shared mapping, example and governance documents are in English and/or Chinese as marked in the language index.

## 4. Start with one complete Worker

1. Read the [composition and delivery guide](docs/guide.en.md).
2. Inspect [document-worker.json](examples/document-worker.json), or its equivalent [YAML](examples/document-worker.yaml). It accepts `material` and returns `summary` and `openQuestions`.
3. Check its responsibilities, input/output schemas, required resources and acceptance contract; compare the [sample input](examples/document-input.json) and [sample output](examples/document-output.json).
4. To execute it, bind the definition to a real instance, supported resources and effective authorization in a host. Record output, acceptance and business adoption separately.

The examples are contract examples, not a deployment package or a conformance validator. The composition fixture includes unresolved bindings and is not ready for live execution.

## 5. Product names and external specifications

Teloa’s **AI employee** remains the user-facing product concept. Its Worker definition describes the role and work contract; a Worker instance identifies a specific AI employee. An AI team uses a Team contract to organize collaboration and delivery.

Grok’s **Bot** can map to AI Worker. A **Team Bot** shared by people is initially a shared Worker, not automatically a team of multiple Workers. Conformance depends on definitions and host behavior, not the name. See the [mapping guide](docs/mappings.md) for sources and checks.

OASF informs discovery metadata and taxonomies. Oracle Agent Spec informs component references, interfaces and composition. This reuse does not establish complete format or runtime compatibility.

## 6. Status, contribution and citation

This is a **Teloa-maintained open specification**, not an adopted international standard or a certification scheme. No endorsement by xAI, AGNTCY, Oracle or a standards organization is claimed. Independent interoperability requires reproducible evidence from independent implementations.

Submit concrete ambiguities, counterexamples and versioned implementation reports through [Issues](https://github.com/teloa-ai/ai-worker-standard/issues). See [GOVERNANCE.md](GOVERNANCE.md) for changes and [CONFORMANCE.md](CONFORMANCE.md) for claims.

Suggested citation: **Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** Machine-readable citation: [CITATION.cff](CITATION.cff).

## 7. License

Original material is licensed under [Apache-2.0](LICENSE). Product names and linked third-party specifications retain their respective rights. Reading or implementing this standard requires no Teloa account or service.

The Teloa name and squirrel artwork follow [separate brand-use terms](assets/brand/README.md).
