# Examples / 示例

These are definition and reading examples, not executable product packages. Importing a file does not authorize work or configure a host.

| File | What it shows |
| --- | --- |
| [document-worker.json](document-worker.json) | Complete minimal Worker: responsibilities, one Work contract, no external resources, human acceptance |
| [document-worker.yaml](document-worker.yaml) | Optional YAML authoring form with the same JSON value |
| [document-input.json](document-input.json) | Synthetic input matching the summary Work schema |
| [document-output.json](document-output.json) | Hand-written illustrative output; not a recorded model response |
| [composition-fixture.json](composition-fixture.json) | Two Workers, two Teams and an Organization, with shared-instance and host-mapping illustrations |

`composition-fixture.json` is a test fixture. Each object in `definitions` is a definition document. Its outer `fixtureFormat`, `bindingExamples`, `compositionMappingExamples`, resource documents and expectations are explanatory fixture data, **not fields in the standard exchange document**. There is no standard bundle importer implied by this wrapper.

The composition fixture's application and handler bindings are intentionally unresolved. Its `not-ready` status is expected. Before execution, a host needs actual actors, authorized resources, input distribution, fixed output provenance, a completeness checker and acceptance. A repeated definition reference means template reuse; only a host binding establishes that two slots use the same Worker instance.

The fixture uses generic responsibilities to exercise structure. Its two teams reuse both the analyst and reviewer definitions; the Organization's sample Work delegates to team A, leaving team B unused for that Work. This is not the detection-engineering organization illustrated in the guide and is not a set of production-ready SOC job descriptions.

## 中文使用说明

先读材料摘要 Worker，再读输入与示意输出。例子刻意不配置技能、记忆和例行，说明这些资源按工作需要选择。`authority/owner` 是人工验收位置，部署时必须绑定可信本人，不能由模型自报身份。

组合示例同时展示“已有材料可分析”和“在线告警应用未配置”。前者仍须满足整体条件和自身依赖，后者不得被标为就绪。完整性检查者、实际授权及固定成果尚未绑定，不能把示例当成已运行的团队。
