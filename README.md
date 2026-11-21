# 词模 CiMo

[词铸 CiZhu](https://github.com/chinese-ci/cizhu) 的契约仓库。只放跨工具、规范性的文本与数据模型；不放工具代码、工程叙事、验收记录或来源细节。工程全貌见词铸 CiZhu。

## 契约

| 契约 | 内容 | 生产方 → 消费方 |
| --- | --- | --- |
| [`source-corpus`](contracts/source-corpus.md) | 来源语料：`source_id`、`order`、物料类目候选、文本 | 来源层 / BYO 生产者 → [词修 CiXiu](https://github.com/chinese-ci/cixiu) |
| [`material`](contracts/material.md) | 纯词库物料的文件、行格式与确定性 | [词修 CiXiu](https://github.com/chinese-ci/cixiu) → [词合 CiHe](https://github.com/chinese-ci/cihe)、`chinese-dict` |
| [`taxonomy`](contracts/taxonomy.md) | 物料类目树、文件、slug、顺序、依据 | 类目维护者 → [词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe)、验收 |
| [`targets/adapter`](contracts/targets/adapter.md) | 目标适配器接口：encode/emit/verify | [词合 CiHe](https://github.com/chinese-ci/cihe) → 各 target |
| [`targets/rime-wubi`](contracts/targets/rime-wubi.md) | 默认 target 的 Ci 词表与完整包契约 | [词合 CiHe](https://github.com/chinese-ci/cihe)（Ci 词表）、`rime-wubi` 基础项目 → `rime-wubi` |
| [`versioning`](contracts/versioning.md) | 版本、vendoring 与兼容性规则 | 所有工具 |

## 规则

1. 契约正本在词模 CiMo；工具仓库 vendor 副本并校验版本与 SHA-256。
2. 工具之间不在运行时依赖本仓库，也不互相 import 私有结构。
3. 契约变更先走[词铸 CiZhu](https://github.com/chinese-ci/cizhu) 的 RFC/ADR；版本与变更记入 [`CHANGELOG.md`](CHANGELOG.md)。
4. 本仓库不含来源名称、地址、平台类目或原始文件。

## 状态

第一阶段（v1），尚未正式发布；`source-corpus` 保持 `v1.0.0`，在第一阶段内允许调整，正式发布后按语义化版本冻结。

## 许可

契约与文档采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)（见 `LICENSE`）。
