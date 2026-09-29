# Handover：从 HW1P2 到 HW2P2

> 目的：把 HW1P2 的可复用经验交接给 HW2P2。  
> 这不是 HW2P2 的规则、模型或实验方案；开始前必须以 HW2P2 的 assignment、starter code、Piazza staff clarification 和评分标准为准。

## 1. HW1P2 已完成状态（仅作背景）

- HW1P2 的 Kaggle 最终提交为 **85.189%**。
- HW1P2 的 Gradescope code submission 已获 **100/100**。
- HW1P2 的最终模型是一个用于 phoneme frame classification 的 flattened MLP；其输入形状、模型、超参数和结论**不能直接迁移**到 HW2P2。
- 完整归档复盘见：[HW1P2_最终复盘.md](HW1P2_最终复盘.md)。

## 2. 开始 HW2P2 前必须重新确认的事项

不要沿用 HW1P2 的记忆或文件名来猜测。先读并记录：

1. HW2P2 的任务目标、输入/标签格式、评价指标与 leaderboard 指标；
2. 允许与禁止的内容：预训练模型、外部数据、公开 checkpoint、模型参数量、推理时间、提交次数、合作边界；
3. checkpoint / early / final deadline，以及必须提交到哪里（Kaggle、Gradescope、Canvas 等）；
4. starter notebook 或 repository 的唯一可编辑范围、数据路径和运行平台；
5. 最新的 Piazza **staff** clarification。学生帖子可以提供思路，但不是课程许可。

任何模型结构是否符合规则，只要存在歧义，就在实现前询问 TA / Piazza。

## 3. 第一目标：尽早得到可提交的端到端 baseline

在探索高分模型之前，先让 baseline 完成一次很小的 end-to-end smoke test：

```text
读取少量数据
→ 前向与反向传播
→ 保存 checkpoint
→ 重新加载 checkpoint
→ inference
→ 生成符合格式的 submission 文件
```

这样可以最早暴露数据路径、模型保存、kernel 状态、submission 格式等工程问题。不要等到长训练结束才首次验证这些步骤。

## 4. 用“模型假设”而非只调超参数来规划探索

每个候选模型先填写下面 8 行；写不清楚就先不花完整训练预算。

```text
数据事实：输入/标签有什么结构？（时间、空间、文本顺序、图关系、类别不平衡……）
基线局限：现有模型忽略或难以表达哪一种结构？
候选设计：我准备改变什么？（例如 CNN、sequence mixer、attention、augmentation）
作用机制：这个改变为什么可能补足基线局限？
合规与成本：参数/外部数据/预训练限制是什么？训练时间、显存是多少？
最小实验：怎样低成本排除明显不可行方案？
晋级规则：什么指标、稳定性或效率结果才值得跑 full confirmation？
停止规则：什么结果说明不再继续投入预算？
```

核心问题是：

> 数据真正的结构是什么，而模型有没有直接利用它？

HW1P2 的教训是：D10/D20/D30 是对同一类 flattened MLP 的局部 regularization 搜索；它建立了可靠提交，但没有足够早地探索把时间结构显式纳入模型的不同模型家族。HW2P2 应保留一条稳定 baseline 轨道，同时留出一条探索“与数据结构相匹配的模型”的轨道。

## 5. 两条实验轨道

| 轨道 | 目标 | 动作 |
| --- | --- | --- |
| 稳定轨道 | 始终保留可提交版本 | baseline、checkpoint、submission pipeline、常规调参 |
| 探索轨道 | 证伪或支持新假设 | 小规模 feasibility test、吞吐/显存测试、full-data confirmation |

短小 screen 适合排除明显失败、超限或训练不稳定的方案；它不能自动否定训练较慢起效的深模型。比较候选时，不能只看 epoch 数，还要记录优化 step 数、有效 batch size、看到的样本量与 wall-clock 时间。

## 6. 每一次实验的最小记录

每个 run 至少记录：

- 假设和唯一主要改动；
- 代码版本 / notebook 版本、完整 config、seed；
- train/dev split、epoch、batch、实际 step 数；
- 最佳 validation 指标、对应 checkpoint 与是否已提交；
- 训练耗时、吞吐、显存或设备限制；
- 结论：保留、淘汰，或需要什么补充验证。

结果要区分层级：本地 smoke、短 screen、full validation、Kaggle 结果、W&B 记录、最终 submission。没有实际运行的步骤不能写成“已完成”。

## 7. Notebook 与提交卫生

- 只保留一个权威配置入口，例如 `config` 或单个 preset；run name、seed、数据分割和 checkpoint 路径都从这里派生。
- 尽量自上而下运行 notebook；若必须跳 cell，立刻写下本次 run 已执行的 cell、当前变量来源和使用的 checkpoint。
- 为训练和提交分别保留清楚路径：训练路径创建/保存模型；提交路径明确加载已选 checkpoint 并产生 submission。
- 在 deadline 前预留至少 20% 时间用于训练完成、下载/上传、leaderboard 等待、Gradescope zip 和自动评分核验。
- 已提交的材料应当保留原样；如果做复现实验，使用新 run name 和新文件夹，避免覆盖证据。

## 8. 对协作方式的约定

### Cathy

- 每次准备启动完整训练前，先给出当前 config、预计耗时、目标问题，以及是否已经通过 smoke test。
- 截图请尽量包含 run name、epoch、metric、checkpoint 路径或报错全文，便于准确判断。
- 遇到规则或模型许可不明确时，先问 staff；不要根据高分同学的方案默认可用。

### Codex

- 先读 HW2P2 题目与评分要求，再建议模型；不把 HW1P2 的架构或超参数当作默认正确答案。
- 每次建议实验时明确说明：它验证什么、不能验证什么、成本是多少、晋级/停止门槛是什么。
- 修改前先检查当前 notebook/source；给出准确的 cell、修改内容、原因和重跑顺序。
- 先建立最小 end-to-end 验证；不把未运行的结果、学生自述或短 screen 说成最终证据。
- 外部高分方案只作为研究线索；单独核对课程许可、资源和可复现性。

## 9. HW2P2 的开局清单

- [ ] 读完 assignment、rubric、starter code 与最新 staff post
- [ ] 写下数据 shape、metric、限制与 deadline
- [ ] 跑通最小 end-to-end baseline smoke test
- [ ] 建立实验记录表/日志目录
- [ ] 写出 2–3 个模型假设，其中至少 1 个不是 baseline 的局部调参
- [ ] 确认所有候选结构符合课程规则
- [ ] 为稳定轨道和探索轨道分配训练时间预算
- [ ] 在提交前实际验证 checkpoint、submission 文件和提交平台结果

## 10. 参考材料

- [HW1P2 最终复盘](HW1P2_最终复盘.md)
- [Piazza @369 原帖存档](../references/piazza_369_2026-09-22/Piazza_369_Original.md)
- [Piazza @369 详细对比与下一次竞赛方案](../references/piazza_369_2026-09-22/复盘与下一次竞赛方案.md)

