# 契约版本与 vendoring 规则

本规则适用于 `source-corpus`、`material`、`taxonomy`、`targets/adapter`、`targets/rime-wubi`。

## 版本号

- 契约使用语义化版本：`MAJOR.MINOR.PATCH`。
- 新增可选字段、放宽校验为 `MINOR`。
- 删除字段、改变必填性、改变语义、改变文件集合或确定性规则为 `MAJOR`。
- 只修正文档措辞、不改变行为为 `PATCH`。
- `source-corpus` 的 `schema_version` 与契约 `MAJOR` 对齐。

## 正本与副本

1. 正本（规范性文本、schema、示例）在词模 CiMo `contracts/`。
2. 工具仓库不运行时依赖词模 CiMo；按需 vendor 副本：

```text
contracts/vendor/source-corpus-v1/
├── VERSION            # 1.0.0
├── sha256             # 正本文件清单与哈希
├── schema.json        # 机器可读 schema（如有）
└── examples/          # 最小示例
```

3. 工具仓库的测试必须校验 vendored 副本的 `VERSION` 与 `sha256` 和已发布契约一致。
4. 升级契约时，先改词模 CiMo，再在各工具仓库更新副本并跑契约测试；不得只改副本。

## 发布流程

1. 在词模 CiMo 更新契约，并在 `CHANGELOG.md` 记录变更说明。
2. 更新[词铸 CiZhu](https://github.com/chinese-ci/cizhu) `releases/matrix.md` 中受影响的工具版本与验收运行。
3. 工具仓库 vendor 新版本，提交时引用词模 CiMo 契约提交 SHA。
4. 跑该工具的契约测试；跨工具变更再跑完整验收。

## 兼容性矩阵

[词铸 CiZhu](https://github.com/chinese-ci/cizhu) `releases/matrix.md` 是唯一记录「工具版本 + 契约版本 + 验收结果」的地方。每次发布或验收后更新，不把版本矩阵散落在各工具 README。

## 未正式发布

第一阶段契约尚未正式发布；`source-corpus` 保持 `v1.0.0`，允许在第一阶段内调整，但每次调整都记入 `CHANGELOG.md` 并重跑验收。正式发布后按语义化版本冻结。
