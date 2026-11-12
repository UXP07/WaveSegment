# CLAUDE.md — 深度学习工作区项目准则

本工作区为多Agent协作的深度学习研发环境，完整设计蓝图见 `docs/深度学习团队架构设计.md`。本文件是蓝图第一章"协作与编码准则"的执行载体，由 WikiAgent 维护，任何修改须经人类确认。

## 1. 环境约定（硬性）
- **深度学习编码任务**（数据处理、训练、评估、推理、移植等）：一律在 conda `Segment` 环境执行，如 `conda run -n Segment python xxx.py`。
- **PPT与文档整理**（DocAgent / WikiAgent 产物）：使用 conda `base` 环境。

## 2. 协作与编码准则（最高优先级）
1. **先逻辑后行动**：写入、修改、创建任何代码与文件前，先说明改动逻辑与依据、列出修改清单，经人类确认后执行。
2. **有据则谏**：有逻辑、有依据时应主动提出建议乃至反驳，不盲目顺从；最终裁量权归人类。
3. **角色化Skill**：按场景为各Agent沉淀skill至 `.claude/skills/`（WikiAgent维护）。
4. **代码风格**：简洁、高效、易于理解；用更少的代码解决问题，不为一次性需求添加抽象与灵活性。
5. **必要注释**：注释说明意图与依据（为什么这么做），不复述代码字面（做了什么）。

## 3. 目录与git策略
| 目录 | 用途 | 入git |
| :--- | :--- | :--- |
| `configs/` | YAML配置中心（default + 实验覆盖） | ✅ |
| `data/` | raw / interim / processed / external 数据 | ❌ |
| `docs/` | 项目文档（含架构蓝图） | ✅ |
| `models/` | networks / losses 定义；pretrained 权重 | 定义✅ 权重❌ |
| `notebooks/` | Jupyter 探索性分析 | ✅ |
| `papers/` | 论文PDF（pdfs/）与参考代码库（repos/），只读参考，移植时改写进 src/ | ❌ |
| `outputs/` | logs / checkpoints / predictions / metrics / reports，自动生成 | ❌ |
| `scripts/` | CLI入口（train / evaluate / infer / export_model） | ✅ |
| `src/` | 核心模块（data / features / models / training / inference / visualization / utils） | ✅ |
| `tests/` | 测试代码 | ✅ |
| `.claude/` | agents / skills 定义 | ✅（修改须人类确认） |

## 4. Agent团队（`.claude/agents/`）
| 文件 | 角色 | 触发方式 | 环境 |
| :--- | :--- | :--- | :--- |
| `orchestrator.md` | 总调度（编排职能由主会话承担） | 主会话常驻 | — |
| `data-prep.md` | DataPrepAgent 数据准备 | 编排调用 | Segment |
| `model-builder.md` | ModelBuilderAgent 模型构建 | 编排调用 | Segment |
| `training.md` | TrainingAgent 训练执行 | 编排调用 | Segment |
| `evaluation.md` | EvaluationAgent 评估推理 | 编排调用 | Segment |
| `visualization.md` | VisualizationAgent 可视化 | 编排调用 | Segment |
| `experiment-tracker.md` | ExperimentTrackerAgent 实验跟踪 | 编排调用 | Segment |
| `doc.md` | DocAgent 对外PPT/文档 → `outputs/reports/` | 按需（人类/编排） | base |
| `wiki.md` | WikiAgent 对内文档与配置维护 | 按需（人类为主） | base |

## 5. 元文件修改门槛
`CLAUDE.md`、`.claude/agents/*`、`.claude/skills/*` 直接约束所有Agent的行为——任何修改必须先列出改动清单，**人类确认后**由 WikiAgent 执行。

## 6. 可复现性基线
- 配置固定 `seed`；实验配置经 `configs/` 继承覆盖（YAML，OmegaConf/Hydra 优先）。
- 代码与配置入git；每次实验的超参、环境、代码版本与指标由 experiment-tracker 记录。
