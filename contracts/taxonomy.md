# 物料类目契约 v1.0.0

**正本数据**：[`taxonomy/taxonomy.json`](taxonomy/taxonomy.json)
**变更记录**：[`taxonomy/CHANGELOG.md`](taxonomy/CHANGELOG.md)
**消费者**：[词修 CiXiu](https://github.com/chinese-ci/cixiu)、[词合 CiHe](https://github.com/chinese-ci/cihe)、验收、`chinese-dict`。

本契约定义纯词库物料的类目树、文件、slug、顺序与 `planned` 标记。来源类目体系不进入本契约；来源层自行把来源库映射为这里的类目候选。

## 结构

```text
groups[]
├── name: 下里巴人 | 阳春白雪
└── categories[]
    ├── name        # 类目名，同时是目录名
    ├── slug        # 文件名中的类目标识，如 daily_spoken
    └── files[]
        ├── name    # 文件名（不含 .txt）
        ├── planned # true 表示 v1 必须为空
        └── standard（可选）
```

- 物料路径为 `<group>/<category>/<file>.txt`。
- 类目顺序与文件顺序即契约顺序；`其它` 固定最后。
- v1 固定 33 个类目、73 个文件。
- `slug` 用于[词合 CiHe](https://github.com/chinese-ci/cihe) 的 `wubi86.ci.<slug>.dict.yaml` 与主表 `import_tables` 顺序。

## 依据

| 标准/来源 | 用途 | 未采用部分 |
| --- | --- | --- |
| [GB/T 20093-2022](https://openstd.samr.gov.cn/bzgk/std/newGbInfo?hcno=88537A39BE1C3D1B5C6294F70838096E) | 阳春白雪 24 个一级类目及顺序 | 367 个二级类目未在 v1 展开 |
| [GB/T 2260](https://openstd.samr.gov.cn/bzgk/std/newGbInfo?hcno=C9C488FD717AFDCD52157F41C3302C6D) | 地名/行政区划 | — |
| [GB/T 2659](https://openstd.samr.gov.cn/bzgk/std/newGbInfo?hcno=FBCE39BE204B54F6F944092C209121F2) | 地名/国外地名 | — |
| 《[通用规范汉字表](https://www.gov.cn/zhengce/zhengceku/2013-08/19/content_1289.htm)》 | 单字与字形边界，8,105 字 | — |
| [项目调研](https://github.com/chinese-ci/cizhu/tree/main/research) | 下里巴人 8 类与子类目提案 | 来源平台类目体系不进入本契约 |

详细调研见[词铸 CiZhu research](https://github.com/chinese-ci/cizhu/tree/main/research)；标准原文不进入仓库。

## 消费规则

- 工具仓库 vendor 本目录的 `taxonomy.json`，测试校验版本与 SHA-256。
- [词修 CiXiu](https://github.com/chinese-ci/cixiu) 用它校验语料中的物料类目候选、生成物料文件。
- [词合 CiHe](https://github.com/chinese-ci/cihe) 用它读取 slug 与类目顺序生成 Ci 词表，不硬编码；完整包的 import 顺序由 [rime-wubi](https://github.com/rime-wubi/rime-wubi) 基础项目维护。
- 验收用它检查 73 个文件与类目词表一一对应。

## 变更

- 新增类目/文件为次版本；删除、改名、改路径、改顺序或改语义为主版本。
- 变更流程：[词铸 CiZhu](https://github.com/chinese-ci/cizhu) RFC/ADR → 更新本契约与 CHANGELOG → 工具 vendor 升级 → 重跑物料与目标产物验收。
