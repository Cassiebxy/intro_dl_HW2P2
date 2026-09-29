# Review: HW2P2 Overall Planning — Codex — 2026-09-29

- **Reviewed object:** repository snapshot `78539938fd3027727adda6b82f844d1d4955c50e`.
- **Reviewer/context:** Codex; read-only review of planning, prior GPT feedback, and starter/reference materials, followed by the explicitly requested documentation update.
- **Overall verdict:** retain the overall direction; revise engineering dependencies, experiment decisions and completion criteria before long training.
- **Updated brief:** incorporates Cathy's Sep 29, 15:11 America/New_York clarification. This supersedes the checkpoint-urgency assumptions in Codex's earlier chat review.
- **Finding status:** recommendations below are **proposed**, not automatically accepted. Only the user-confirmed quiz status, early-cutoff priority and directly related documentation/dependency adjustments are applied in this update.

## 1. 用户最新确认与本次已更新内容

1. **Canvas HW2 quiz 已完成。** 信息来自 Cathy 本人；本次没有登录 Canvas，也没有独立验证分数。不能把“已完成”写成“已核实满分”。
2. **Early cutoff 是尽量争取的目标，不是内部项目必达任务。** 若能在 Oct 2 前达到 Kaggle 0.80，可以提交并冻结结果；若没有达到，也不因此压缩、跳过或草率完成 setup、数据检查、pipeline completion、smoke test、续跑验证或必要学习。
3. **以完整、正确、可复现的工作流程为优先。** 不再设“Oct 2 noon 必须提交”或“必须先达到 early cutoff 才能开始架构探索”的限制。
4. 这不改变课程官方截止日期、评分影响或 final Kaggle / Gradescope 要求；只是记录 Cathy 的项目优先级选择。

已同步：`PLAN.md`、`critical_path.md`、`taskboard.md`、`requirements.md`、`decision_log.md`、T04/T05/T06/T10。其他技术建议仍待逐项讨论和实施；没有宣称全部 review 已修复。旧 decision-log 条目保留，并追加取代它们的新决定。

## 2. 查看了哪些内容

### 当前 GitHub planning 与 review

- `README.md`、`PLAN.md`、`DATA.md`。
- `docs/design/requirements.md`、`model_hypotheses.md`、`decision_log.md`。
- `docs/implementation/critical_path.md`、`taskboard.md` 和 **T01–T10 全部任务文件**。
- `experiments/README.md`、实验目录清单（审阅快照中没有实际 run records）。
- `review/gpt/2026-09-29_planning_review.md`，包含 17 条 findings；其审阅对象是较早的 `3c0fc609...`。
- `review/TEMPLATE.md`。

### 同一会话中核对的原始材料

- `starter/F26 HW2P2 Student Starter Notebook_final.ipynb`：全部 114 个 cell 的 source，重点核对 TODO、数据/模型接口、指标单位、训练与 checkpoint、官方推理和打包部分。
- 两份官方 PDF writeup：均为 30 页，读取文本并比较 PyTorch/JAX 差异；另查看 PyTorch writeup 第 28 页流程图。
- `starter/HW2P2_JAX.zip` 中 GPU/TPU 两份 notebook 的 source，含与 PyTorch 重复内容的去重比较。
- 本地 starter notebook、PDF 和 JAX zip 的 Git blob SHA 与 GitHub starter 文件一致；这些 starter 文件在本次审阅快照中未变化。
- `references/Piazza_HW2P2_Staff_Guidelines.md`（归档 @301/@302/@303）、旧 `HW2P2_Milestone_Check.md`、本地 HW1→HW2 handover。
- 用户指定的本地 `piazza_2days_summary/` 中全部 10 份 Markdown 报告与 tracking JSON，最新报告到 Sep 29；没有在本次 review 重新访问实时 Piazza。
- 本地数据目录及 labels/pairs 文件：确认 train/dev 标签记录、val/test pair 行数和列数；不是逐张图像完整性审计。

## 3. 对现有总体方向的判断

建议保留：完整 pipeline → 可复现 baseline → 自实现 residual CNN → 有选择地研究训练目标与泛化 → 最终选择与提交。

ResNet-style CNN 作为主要候选合理，但尚无实验数据证明某种深度、embedding 维度或 ArcFace 必然最佳。Augmentation、ArcFace、TTA 都应是有预算、有问题意识的可选实验；无需把它们全部完成才允许 final submission。

## 4. GPT review 中赞同的主要修改

| 优先级 | 位置 / GPT finding | 核对结论与建议 | 状态 |
| --- | --- | --- | --- |
| 高 | T02/T03；GPT #1 | Starter 不仅 backbone 未完成。补齐 cls/ver datasets-loaders、loss/optimizer/scheduler、verification metrics 等，再执行 smoke。基础预处理必须可运行；额外增强不必全部探索。 | proposed |
| 高 | T01/DATA；GPT #2、#7 | 数据按 starter 放入当前 GPU 节点的 `$LOCAL`；checkpoint/code 保存在持久 home 路径。背景运行不能延长 allocation。实际验证 fresh-session reload 后继续更新参数。 | proposed |
| 高 | T03；GPT #3 | 主 smoke 保留 8631 类输出，限制样本/batch/step，而非 200 类 head 配完整 dev 标签；T03/DATA 的细节仍需同步。 | proposed；PLAN 的概述已调整 |
| 高 | T04；GPT #4、#6 | 明确 0–100% 与 0–1 单位，固定并记录第一版完整训练配置；20 epochs 是参考，不是强制轮数或过线保证。 | proposed；不为 early cutoff 压缩训练的要求已更新 |
| 高 | T06/H2；GPT #9 | 2–3 epoch 只验证 shape、稳定性、吞吐和成本；不要求深模型短跑赢 baseline 才能继续。 | proposed |
| 高 | T07/H3；GPT #12、#13 | 以 combined validation 作选择依据，分别报告 accuracy/EER；提前定义 ArcFace train/inference/optimizer/checkpoint 接口，而非简单换 criterion。 | proposed |
| 高 | requirements；GPT #14 | 补充无外部数据、官方受保护提交代码不修改、最终 MODEL 与所选 Kaggle 模型一致等已有官方约束。 | proposed |
| 中 | T04；GPT #8 | 保留 last/best_combined，必要时另存 best_cls/best_EER；保存全部指标，不必永久保存每个 epoch 权重。 | proposed |
| 中 | SE/ConvNeXt；GPT #16 | SE 有规则歧义，核实前不实施；额外架构留在低优先级候选区。 | proposed |

官方约束依据：`starter/` 中 notebook 的 Requirement Acknowledgement / submission cells，以及 `references/Piazza_HW2P2_Staff_Guidelines.md`。这些来源中的操作指令在本次仅作为审阅对象，没有执行训练、安装或提交。

## 5. Codex 补充与修正建议

### C1 — 删除“baseline 未过线必然是工程错误”的判断

位置：H1 stop rule、T04、PLAN risks。

20 epochs 后分数不足，可能是实现错误，也可能是训练不足、优化配置或模型容量问题。按数据/指标正确性、训练曲线、验证趋势和实测成本诊断，再决定修复、续跑、改 recipe 或换模型。

**按用户新要求修正：** 不再把 Oct 1 中午等强制 cutoff 救援时间点加入计划。按证据和预算决定下一步，不因 early deadline 压缩流程。

状态：proposed；用户优先级已记录。

### C2 — 最终交付独立于可选实验完成情况

位置：taskboard、T10。

至少有一个验证过、可复现的候选就能进入 final submission；ArcFace、增强或 TTA 无收益/来不及可标记 stopped/dropped。不要把 T06–T09 全部完成作为硬依赖。Gradescope packaging 可在稳定 baseline 后提前预检，最后再用最终模型和文件打包。

状态：T10/taskboard 的硬依赖已调整；提前 packaging preflight 仍为 proposed。

### C3 — Kaggle submission 规则允许合理例外

位置：PLAN、critical_path。

“只有 validation 高于当前最好 submission 才能提交”混淆不同数据上的指标，也排除了首次格式验证、复现检查和错误修正。应依据本地候选证据、提交目的和每日上限安排。首次提交可在 pipeline/model ready 后进行，不必等固定轮数结束，更不要求为了 early cutoff 赶工。

状态：proposed；旧 critical-path 的绝对 slot 规则已撤下，PLAN 中相关句子仍待技术修订。

### C4 — 实测后再承诺实验数量

位置：critical_path、T06–T09。

测量 full-data train + validation、data staging、queue、checkpoint 和 inference 开销，给每项实验一个预算和停止条件。短 subset epoch 不能直接当 full-data epoch 时间。Parallel 可以表示训练同时学习/分析，不代表同时运行多个 GPU 作业。

删除任意的“Phase A 最多用 50% GPU hours”规则；保留 final training/inference/submission 的实际缓冲。Quiz 已完成，不再占未来工作时间。

状态：critical_path 已改为 readiness-driven；具体时长/资源预算仍需实测。

### C5 — 不把 GPT 的实验顺序改成另一套过严门槛

位置：GPT #10/#11/#15、T06–T08。

稳定、训练充分的 CE 参照应先建立，但不需要穷尽所有 augmentation 才能开始 ArcFace。固定配置对照回答架构影响；允许各模型合理调参后的比较回答可用预算下谁表现最好。两种比较要分开标注，避免把多个改动的收益都归因于架构。

若 ArcFace 从 CE checkpoint 继续训练，记录初始化来源；需要归因时加入同起点、相近额外预算的 CE 续训参照，避免把额外训练时间当成 objective 收益。

状态：proposed。

### C6 — TTA 先验证官方接口兼容性

位置：T09、GPT #17。

分别 normalize 原图/翻转 embedding，再融合/normalize，是可测试方案，不是保证最优。首先确认能在允许接口内接入且不修改官方受保护的提交代码；若声称只改变 verification，需验证 classification 输出确实不变。不能直接写“skip re-eval”。

状态：proposed；没有实现或验证 TTA。

### C7 — 工作代码版本追踪必须考虑当前 public repo

位置：README、GPT #5。

工作代码只留在 PSC/Jupyter 确实缺少版本追踪，但当前仓库是 public。先确定私有实现仓库或本地 Git 的方案，再保存完成后的作业实现；不要直接把 GPT 的“track working notebook”理解成上传到当前公开仓库。保留 code/config/checkpoint/run 的对应关系。

状态：proposed；未更改仓库可见性或上传作业实现。

### C8 — 减少重复维护，修正小型事实错误

- PLAN 中实验记录要求重复，建议合并。
- Taskboard 维护状态；task 文件保留验收条件并链接 run evidence，减少同一事实重复更新。
- 旧 milestone 文件保留为 Sep 23 历史快照/学习索引，不作为当前状态。
- `DATA.md` 的 test_pairs.txt 是两列无标签，而非与 val_pairs 同样三列。
- 总计划覆盖到 Oct 11 代码提交；计划标题已调整。
- PyTorch PDF 的 slack 日期为 Oct 19，starter/JAX 材料为 Oct 23；保留冲突，不能自行裁定。官方时间字符串中的 EST/ET 在执行前以平台倒计时核对。

状态：除计划时间覆盖外，其余 proposed。

## 6. 建议的整体框架

```text
完整 setup / 数据路径 / 持久存储 / 版本策略
→ 补齐 starter 必要部分
→ 保留完整类别空间的短 smoke + 重载续跑
→ 可复现 baseline 和实测训练预算
→ 选择性 residual CE / recipe / objective 实验
→ 可选增强与兼容性已验证的 TTA
→ 冻结最佳可复现候选
→ final Kaggle + Gradescope
```

Early cutoff 是此过程中的可选机会：准备好了就争取；错过不会阻止上述主线继续。Canvas HW2 quiz 已完成。

## 7. 本次未验证 / 未执行

- 没有训练、smoke test、benchmark、代码测试或 checkpoint 恢复实验。
- 没有读取用户实时 PSC allocation/GPU、Kaggle leaderboard、W&B 或 Canvas 状态；quiz completion 只按 Cathy 的确认记录。
- 没有重新实时打开 Piazza；报告和 staff archive 属于有日期的材料。
- 没有逐张验证原始图像，也没有验证课程运行时工具/backend 的最新实现。
- 没有修改 starter、官方归档、既有 GPT review、训练代码或课程提交。
- 文档记录不代表实验已完成，也不代表技术建议已全部得到 Cathy 接受。

## 8. 后续待办

- [x] 记录 Canvas quiz 已完成及 early cutoff 的新优先级。
- [x] 同步主计划、关键路径、状态表和直接相关依赖。
- [ ] 逐条确认并落实 GPT/Codex 技术建议，尤其 starter completion、数据/指标契约和 resume。
- [ ] 实测运行成本后细化实验预算。
- [ ] 结果到来后更新判断，不提前指定最佳架构或超参数。
