# 目标适配器契约 v1.0.0

**消费者**：[词合 CiHe](https://github.com/chinese-ci/cihe) 与各 target 适配器。

目标适配器把纯词库物料转换为某一种输入法可用的 Ci/内容片段。适配器可以调用外部编码器；[词合 CiHe](https://github.com/chinese-ci/cihe) 只负责调度、报告与通用校验。输入法项目的基础结构（schema、字表、主表、发布文件）由产物仓库维护，适配器不生成这些文件。

## 接口

```text
encode(material, taxonomy, char_table?, options) -> encoded data
emit(encoded, output_dir) -> artifact files
verify(artifact, options) -> report
```

- `target` 名称唯一、稳定，如 `rime-wubi`。
- 默认 target 由[词合 CiHe](https://github.com/chinese-ci/cihe) 固定为 `rime-wubi`；其它 target 必须显式指定。
- 适配器不得读取来源语料、来源标识或来源元数据。
- 适配器不得在产物中写入来源名、频次、权重或原始字形。

## 输入

- 纯词库物料，符合 `material` 契约。
- 物料类目数据，符合 `taxonomy` 契约。
- 可选字表与 target 选项。

## 输出

- 产物目录；文件集合由具体 target 契约规定。
- `build-report.json` / `build-report.md`：编码、跳过、阈值与产物指纹；不含来源名与权重。
- 验证报告：可被验收读取的 JSON。

## 确定性与版本

- 同一物料、字表、编码器与 target 版本必须得到逐字节一致的产物。
- 适配器版本独立于[词合 CiHe](https://github.com/chinese-ci/cihe)；词合 CiHe 报告记录 target 名称与版本。
- 新增 target 不需要修改 material/taxonomy/source-corpus 契约。

## 扩展

- 适配器放 `CiHe/src/cihe/targets/<name>/`，附契约测试与最小示例。
- 具体 target 的规范性细节写在该 target 的契约中，不写进本通用契约。
- v1 只验收 `rime-wubi`；其它 target 只要求通过本适配器接口测试。
