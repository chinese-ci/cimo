# 契约变更记录

## 2026-11-21

- `source-corpus` v1.0.0（未正式发布）：字段收缩为 `source_id`、`order`、`categories`（物料类目候选）、words；删除 `platform`、`title`、`description`、`provenance`；来源类目映射与库级筛选移到来源层。
- `taxonomy` v1.0.0：新增物料类目契约、`taxonomy.json`、33 个类目/73 个文件/slug 与依据表；独立 `CHANGELOG.md`。
- `package` 契约拆为 `targets/adapter` 与 `targets/rime-wubi`；默认 target 为 `rime-wubi`。rime-wubi 基础结构（schema、字表、主表、README/LICENSE）归产物仓库，词合 CiHe 只产出 Ci 类目词表。
- `material` v1.0.0：文件集合与 slug 改为引用 `taxonomy`。
- 工程叙事、规格、ADR、术语、调研、政策、Agent 规则迁至词铸 CiZhu；验收与采集归档迁至私有来源层。
