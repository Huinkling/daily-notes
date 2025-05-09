# 🌱 模型训练日志 / Model Training Log  
📅 **日期 / Date**: 2025-05-08  
👤 **负责人 / Author**: Ink  
📂 **项目 / Project**: 玉米病害多任务识别模型（位置 + 等级） / Corn Disease Multi-Task Recognition (Position + Severity)

---

## 🎯 当前训练思路 / Current Training Strategy

- 使用 **ResNet-Plus** 架构进行多任务学习：
  - **任务 1 / Task 1**: 病害位置分类（多类别分类）
  - **任务 2 / Task 2**: 病害等级预测（0–25回归）
- 多任务加权损失函数：
- Total Loss = Position Loss × 0.7 + Grade Loss × 0.3

  - 输入图像为遥感数据，标签来自 14l / 14m / 14t 三个数据源

---

## 🎯 当前阶段目标 / Current Phase Objectives

> **主攻位置任务的泛化能力，维持等级任务的稳定性能**  
> Focus on improving position classification generalization while maintaining stable grade prediction.

---

## 📊 使用训练指标 / Training Metrics

| 指标 / Metric             | 类型 / Type | 含义 / Meaning                                      | 当前表现 / Current Values       | 评估 / Evaluation |
|---------------------------|--------------|------------------------------------------------------|----------------------------------|--------------------|
| **位置准确率 / Accuracy**   | 分类 / Class | 正确预测位置的比例 / Percentage of correct classes | Train: 69.4%, Val: 43.8%         | 👍 较好 / Fair      |
| **位置 F1 分数 / F1 Score**| 分类 / Class | 精度+召回综合指标 / Balanced view of class bias     | Val: 0.40                        | ✅ 稳定提升 / Improving |
| **等级 MAE**              | 回归 / Reg   | 平均预测偏差 / Mean absolute error (0–25)          | Train: 0.47, Val: 1.13           | ✅ 良好 / Good       |
| **±2容忍率 / ±2 Tolerance**| 回归 / Reg   | 预测在 ±2 范围内视为正确 / Within-grade tolerance | 100%                             | 🌟 极佳 / Excellent  |
| **总损失 / Total Loss**    | 综合 / Joint | 多任务加权损失 / Combined task loss                | 持续下降中 / Decreasing steadily | ✅ 正常 / Normal     |

---

## 📌 数据规模与局限 / Dataset Size & Limitations

| 项目 / Item               | 当前状态 / Status          | 说明 / Notes                                      |
|---------------------------|-----------------------------|---------------------------------------------------|
| 训练集规模 / Training Set | 62 张图像 / 62 images       | 样本小，等级任务仍能稳定收敛 / Low, but grade task stable |
| 验证集规模 / Validation   | 16 张图像 / 16 images       | 指标易波动 / Evaluation may fluctuate             |
| 位置标签分布 / Class Balance | 可能不均 / Possibly imbalanced | 部分类未被预测 / Some classes never predicted     |
| 等级标签结构 / Grade Labels | 连续整数 0–25               | 已用回归建模顺序关系 / Modeled as regression task |

---

## ✅ 总结与建议 / Summary & Recommendations

- ✅ 等级任务已完成预期表现，可继续保持；  
- ⚠️ 位置任务正逐步突破过拟合，建议继续监控 F1；
- 🔜 下一阶段建议：
- 可视化混淆矩阵 / Confusion matrix visualization
- 数据增强提升小样本类别表现 / Augmentation for rare classes
- 启用 EarlyStopping + 自适应学习率 / Early stopping + LR scheduler

---
