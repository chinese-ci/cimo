# rime-wubi 目标契约 v1.0.0

**消费者**：[rime-wubi](https://github.com/rime-wubi/rime-wubi) 产物仓库、[Rime](https://github.com/rime/rime) 用户。
**生产者**：
- [词合 CiHe](https://github.com/chinese-ci/cihe) 的 `rime-wubi` 适配器：只生成 Ci 类目词表。
- [rime-wubi](https://github.com/rime-wubi/rime-wubi) 基础项目：维护 schema、字表、主表、README 与 LICENSE。

默认目标：与官方 [rime-wubi](https://github.com/rime/rime-wubi) 同名、可 drop-in 的三 schema 方案包。包内不含来源名、词频、权重或采集信息。

## 职责边界

[词合 CiHe](https://github.com/chinese-ci/cihe) 只产出由纯词库物料派生的 Ci 层，不生成、不修改 [rime-wubi](https://github.com/rime-wubi/rime-wubi) 的基础项目结构：

| 文件 | 维护方 |
| --- | --- |
| `wubi86.ci.<slug>.dict.yaml`（33 个） | [词合 CiHe](https://github.com/chinese-ci/cihe) |
| `build-report.json` / `build-report.md`（包外） | [词合 CiHe](https://github.com/chinese-ci/cihe) |
| `wubi86.schema.yaml`、`wubi_pinyin.schema.yaml`、`wubi_trad.schema.yaml` | `rime-wubi` 基础项目 |
| `wubi86.zi.dict.yaml` | `rime-wubi/zi` 构建，`rime-wubi` 基础项目维护 |
| `wubi86.dict.yaml` | `rime-wubi` 基础项目 |
| `README.md`、`LICENSE`、`NOTICE.md` | `rime-wubi` 基础项目 |

发布时由 `rime-wubi` 的流程把基础项目与 Ci 词表组装成完整方案包。

## 词合 CiHe 输出

```text
ci/
└── wubi86.ci.<slug>.dict.yaml    # 33 个类目词表，slug 来自 taxonomy
```

- 每个文件只含 `text` 与 `code` 两列，`sort: original`。
- 文件集合与物料类目一一对应；顺序与文件名来自 taxonomy 契约。
- 不含 schema、字表、主表、README、LICENSE 或 NOTICE。
- 构建报告写在 Ci 目录之外，不进入方案包。

## 完整包文件集合

基础项目与 Ci 词表组装后：

```text
wubi86.schema.yaml
wubi_pinyin.schema.yaml
wubi_trad.schema.yaml
wubi86.dict.yaml
wubi86.zi.dict.yaml
wubi86.ci.<slug>.dict.yaml      # 33 个类目词表
README.md
LICENSE
```

公开产物仓库另外保留 `NOTICE.md`；它不属于包内文件，但属于发布检查项。

## 主表

`wubi86.dict.yaml`：

```yaml
name: wubi86
version: "0.1.0"
sort: original
columns:
  - text
  - code
import_tables:
  - wubi86.zi
  - wubi86.ci.daily_spoken
  # … 其余 32 个类目，按 taxonomy 顺序
```

- `sort: original`，保持物料顺序。
- `import_tables` 第一项固定为字表 `wubi86.zi`，其后固定为 33 个类目词表。
- `columns` 只有 `text` 与 `code`。

## 三个 schema

| schema_id | 说明 |
| --- | --- |
| `wubi86` | 五笔主方案，单字表 + 类目词表 |
| `wubi_pinyin` | 五笔 + 拼音混输，共用主词表，使用 `wubi_pinyin` prism |
| `wubi_trad` | 简入繁出，共用主词表，使用 `wubi_trad` prism 与 [OpenCC](https://github.com/BYVoid/OpenCC) `s2t.json` |

- schema_id 与官方 [rime-wubi](https://github.com/rime/rime-wubi) 同名；部署后遮蔽官方三件套。
- 不提供 `.custom.yaml`。
- `pinyin_simp` 为运行时依赖；OpenCC 仅 `wubi_trad` 使用，均不随包分发。

## 确定性

- 同一物料、字表与编码器版本必须得到相同的 Ci 词表与基础文件。
- 编码只由 [imewlconverter](https://github.com/studyzy/imewlconverter) 生成，不自研编码器。
- 类目词表与物料文件一一对应；顺序来自 taxonomy 契约。

## 校验

1. Ci 文件集合、文件名、表头与排序；
2. 组装后的文件集合与 `import_tables` 顺序；
3. 三个 schema 在真实 Rime 目录编译；
4. 每个 schema 按码查词；
5. `wubi_trad` OpenCC 简繁对照；
6. README 覆盖 drop-in 风险、用户词典、`pinyin_simp`、OpenCC、`import_tables` 与 [LGPL](https://www.gnu.org/licenses/lgpl-3.0.html) 声明。

## 版本

- 当前版本：`1.0.0`。
- [词合 CiHe](https://github.com/chinese-ci/cihe) target 版本与 [rime-wubi](https://github.com/rime-wubi/rime-wubi) 基础项目版本分别维护。
- Ci 文件新增或语义变更按 taxonomy 与 target 契约升级；基础结构变更由 [rime-wubi](https://github.com/rime-wubi/rime-wubi) 基础项目决定。
- 产物发布在 `rime-wubi/rime-wubi`；[词合 CiHe](https://github.com/chinese-ci/cihe) 记录 target 版本与 Ci 词表指纹。
