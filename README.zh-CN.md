# 游戏参与度优先级分析

> **游戏工作室能否用可观察的市场元数据，判断哪些竞品游戏最值得优先人工研究？**<br>
> 一个以证据为核心的产品分析 notebook，用于在分析资源有限时优先筛选高参与度游戏。

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/chrisnch/game-engagement-prioritisation-analytics/blob/main/notebooks/final_notebook.ipynb)

**GitHub 仓库:** https://github.com/chrisnch/game-engagement-prioritisation-analytics<br>
**交互报告:** 使用 GitHub Pages 从仓库根目录发布 `index.html`。<br>
**English version:** [README.md](README.md)

---

## 项目价值

独立游戏和 AA 游戏工作室不可能在做产品、平台和营销决策前深度研究所有竞品。更现实的问题是：有限的分析时间应该先花在哪些游戏上？

本项目模拟这个决策支持场景。它使用一个包含 15,000 款游戏的市场快照，为人工竞品研究生成优先级排序。模型不被解释为自动预测成功的工具，而是一个筛选辅助：帮助分析师先看最值得看的游戏，同时避免把预测关系误解为因果关系、收入结论或用户偏好结论。

**核心问题:** 哪些可观察因素与前 25% 的玩家参与度相关？工作室如何把这些因素作为筛选信号，而不把预测误读成因果？

---

## 关键结果

在固定 **600 款游戏** 的人工审核队列中，随机筛选预计能找到约 **150** 款高参与度游戏。最终机器学习模型找到 **413** 款，比随机多找到 **263** 款，同时把无效审核负担从 **450** 款降到 **187** 款。

<p align="center">
  <img src="figures/decision_simulation_bar_chart.png" width="88%" alt="随机筛选、启发式筛选和最终机器学习筛选的决策模拟对比">
</p>

这个结果支持一个实际工作流：先用模型排列审核优先级，再由分析师对候选游戏进行设计、用户、平台和市场语境判断。

---

## 本项目展示的能力

| 阶段 | 完成内容 |
|---|---|
| **商业问题定义** | 将游戏参与度建模转化为受资源约束的审核优先级问题，而不是普通预测比赛。 |
| **数据审计** | 验证 Kaggle 快照中的 15,000 行、43 个字段，并检查重复键、占位符、缺失值和分析行保留情况。 |
| **目标定义** | 将 `engagement_score` 前 25% 定义为 `high_engagement`，并做阈值敏感性检查。 |
| **泄漏控制** | 移除直接目标泄漏，并确保预处理在训练管道内拟合。 |
| **模型选择** | 用审核队列 lift 比较启发式规则和机器学习模型，而不是只看 ROC-AUC。 |
| **留出集验证** | 在 3,000 款游戏的锁定留出集上报告 ROC-AUC、PR-AUC、precision、recall 和队列结果。 |
| **模型解释** | 使用特征重要性、SHAP 和消融检查，区分可用筛选信号与因果过度解读。 |
| **商业建议** | 将模型输出转化为 human-in-the-loop 的竞品研究优先级流程。 |

### 技术栈

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy` · `SHAP` · `matplotlib` · `seaborn` · `Jupyter`

---

## 结果

### 模型表现

最终选择的模型是 `RandomForest_flexible`。在锁定留出集上的表现为：

| 指标 | 数值 |
|---|---:|
| ROC-AUC | **0.869** |
| PR-AUC | **0.715** |
| Precision | **0.566** |
| Recall | **0.723** |
| Balanced accuracy | **0.769** |
| 留出集行数 | **3,000** |

<p align="center">
  <img src="figures/roc_curve.png" width="46%" alt="最终模型 ROC 曲线">
  <img src="figures/precision_recall_curve.png" width="46%" alt="最终模型 Precision-Recall 曲线">
</p>

### 审核队列提升

最重要的商业结果不是单一分类指标，而是审核队列 lift：

| 审核组 | 审核游戏数 | 找到的高参与度游戏数 | Precision | 相对随机提升 |
|---|---:|---:|---:|---:|
| 预测前 10% | 300 | 256 | 0.853 | 3.41x |
| 预测前 20% | 600 | 413 | 0.688 | 2.75x |
| 预测前 25% | 750 | 469 | 0.625 | 2.50x |

<p align="center">
  <img src="figures/review_queue_outcomes.png" width="88%" alt="按预测优先级分组的审核队列结果">
</p>

### 哪些信号重要？

特征重要性和 SHAP 分析显示，模型使用了发行背景、平台覆盖、系列深度、类型和设计元数据，以及文本派生信号。这些信号适合做筛选，但不应被解释为因果杠杆。

<p align="center">
  <img src="figures/feature_importance.png" width="88%" alt="最终模型特征重要性">
</p>

<p align="center">
  <img src="figures/shap_summary.png" width="78%" alt="最终模型 SHAP 摘要图">
</p>

### 关键限制

本地数据 artefacts 没有记录 `engagement_score` 的具体公式、采集日期或曝光窗口。因此 notebook 将它作为一个不透明的市场参与度代理指标，而不是收入、留存、玩家满意度或因果结果。

---

## 复现方式

### 用 Google Colab 运行

点击 README 顶部的 Colab badge。如果 Colab 没有自动找到仓库，可以打开：

```text
https://colab.research.google.com/github/chrisnch/game-engagement-prioritisation-analytics/blob/main/notebooks/final_notebook.ipynb
```

Notebook 需要提交数据位于：

```text
data/raw/Ultimate_Games_Dataset.csv
```

### 本地运行

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
jupyter lab notebooks/final_notebook.ipynb
```

运行 notebook 会重新生成 `figures/` 和 `outputs/`。提交前应检查这些文件的 diff。

---

## 数据集

本分析使用 Rudra Kumar Gupta 的 Kaggle 数据集 **Ultimate Games Dataset | 15K Games | 43 Features**：

```text
https://www.kaggle.com/datasets/rudrakumargupta/ultimate-games-dataset-15k-games-43-features
```

提交用原始 CSV 存放在：

```text
data/raw/Ultimate_Games_Dataset.csv
```

在公开仓库或重新分发最终 zip 前，请重新确认 Kaggle 数据集 licence 和课程提交规则。

---

## 项目结构

```text
├── README.md                              英文项目说明
├── README.zh-CN.md                       简体中文项目说明
├── index.html                            双语 GitHub Pages 报告
├── notebooks/
│   └── final_notebook.ipynb              标准分析 notebook
├── data/raw/
│   └── Ultimate_Games_Dataset.csv        提交用 Kaggle 原始数据
├── outputs/                              标准 CSV 证据表
├── figures/                              生成的可视化图表
├── report/
│   └── final_business_report.pdf         面向业务读者的 PDF 报告
└── requirements.txt                      运行依赖
```

仓库只保留最终分析 artefacts。草稿 notebook、缓存、处理后数据、外部 enrichment dump 和旧运行结果都不纳入提交。

---

## 输出文件

关键表格：

- `outputs/model_holdout_metrics.csv`
- `outputs/screening_lift_table.csv`
- `outputs/decision_simulation.csv`
- `outputs/target_threshold_sensitivity.csv`
- `outputs/claim_evidence_action_matrix.csv`

关键图表：

- `figures/decision_simulation_bar_chart.png`
- `figures/review_queue_outcomes.png`
- `figures/feature_importance.png`
- `figures/shap_summary.png`
- `figures/roc_curve.png`
- `figures/precision_recall_curve.png`

---

## 项目语境

这是一个以 notebook 为核心的分析提交。最终 notebook 按照数据审计、目标定义、建模、评估、解释和商业建议的顺序展开。

最终建议保持保守：用模型提升人工审核效率，而不是让模型自动决定产品策略。
