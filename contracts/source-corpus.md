# 来源语料契约 v1.0.0

**状态**：第一阶段，尚未正式发布；变更记入 [`CHANGELOG.md`](../CHANGELOG.md)。

本契约定义「来源语料」：来源层或 BYO 生产者交给[词修 CiXiu](https://github.com/chinese-ci/cixiu) 的一组文本与最小元数据。词修 CiXiu 只依赖本契约，不解析来源格式，也不知道来源概念。

## 目录布局

```text
corpus/
├── sources.json
└── text/
    └── <source_id>.txt
```

`source_id` 是不透明、稳定、跨运行不变的字符串；推荐 `<producer>-<local>` 形式，但词修 CiXiu 不解析其含义。允许小写字母、数字、`-`、`_`；不得包含 `/`、`..` 或以 `-`/`_` 开头。

## sources.json

```json
{
  "schema_version": 1,
  "sources": [
    {
      "source_id": "producer-a-0001",
      "order": 0,
      "categories": [
        {"path": "下里巴人/日常口语/其它", "priority": [-2, 30, 0]}
      ]
    }
  ]
}
```

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `source_id` | 是 | 全局唯一；决定文本路径 |
| `order` | 是 | 整数；决定来源顺序 |
| `categories[].path` | 是 | 物料类目文件路径，必须来自 `taxonomy` 契约 |
| `categories[].priority` | 是 | 整数元组；同词多候选时按字典序取最小者 |

`categories` 可为空数组；空候选的来源不会贡献物料，词修 CiXiu 在报告中记为未映射。候选的 `priority` 由生产者计算，词修 CiXiu 只比较元组，不解释各分量含义。同一来源的全部词条共享该来源的候选集合。

不包含标题、描述、来源平台、来源类目、URL 或原始文件信息。真实来源上下文由生产者私有保存，用 `source_id` 关联。

## text 文件

- UTF-8，无 BOM；换行统一为 LF。
- 一行一个原始词条，不做清洗、去重、字形归一或排序。
- 保留空行与首尾空白；清洗阶段处理并统计拒收原因。
- 文件必须能完整解码；非法 UTF-8 或 BOM 时该来源无效，词修 CiXiu 报告并跳过。

## 确定性

1. `order` 升序；
2. 同序按 `source_id` 字典序；
3. 同来源内按文本行序；
4. 类目与文件顺序由 `taxonomy` 固定。

## 生产者责任

- 库级筛选、来源类目到物料类目的映射、删除词条与改类目都在生产者侧完成。
- 增量语料默认只追加新词：生产者应在导出前过滤掉基础语料中已存在的词。
- 生产者可私下保存 `source_id → 真实来源` 的映射，用于排障与审计。

## 版本

- `schema_version: 1` 对应契约主版本 1。
- 增加可选字段为次版本；删除字段、改变语义或必填性为主版本，需要新的 `schema_version`。
- 工具仓库以 vendored 副本消费，规则见 `versioning.md`。
