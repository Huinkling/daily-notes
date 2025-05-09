# 🌱 模型训练日志  
📅 日期：2025-05-08  
👤 负责人：Ink  
📂 项目：玉米病害多任务识别模型（位置 + 等级）

---

## 🎯 当前训练思路

- 使用 ResNet-Plus 架构进行多任务学习：
  - 任务 1：病害位置分类（多类别 Softmax）
  - 任务 2：病害等级预测（0–25 回归）
- 多任务加权损失函数：
total_loss = 位置损失 × 0.7 + 等级损失 × 0.3
- 输入图像为遥感图像，标签来自 14l/m/t 三个数据源

---

## 🎯 当前阶段目标

**主攻位置任务的泛化能力，维持等级任务的稳定性能**

---

## 📊 使用训练指标

| 指标名称         | 类型     | 含义                               | 当前数据          | 评估         |
|------------------|----------|------------------------------------|-------------------|--------------|
| 位置准确率       | 分类     | 模型预测的位置类别是否正确         | 训练: 69.4%, 验证: 43.8% | 👍 较好       |
| 位置 F1 分数     | 分类     | 综合精度与召回，衡量偏类情况       | 验证: 0.40         | ✅ 提升中     |
| 等级 MAE         | 回归     | 平均预测偏差（0~25）               | 训练: 0.47, 验证: 1.13 | ✅ 良好       |
| 等级 ±2容忍率    | 回归     | 等级误差在 ±2 范围视为正确         | 验证: 100%         | 🌟 极佳       |
| 总损失           | 综合     | 两个子任务损失加权和               | 稳定下降中         | ✅ 正常       |

---

## 📌 数据规模与局限

| 项目              | 当前状态           | 说明                                   |
|-------------------|--------------------|----------------------------------------|
| 训练集规模         | 62 张图像           | 样本较小，等级任务已能稳定收敛         |
| 验证集规模         | 16 张图像           | 样本较小，指标可能波动较大             |
| 位置标签分布       | 可能不均衡          | 有些位置类别从未被预测                 |
| 等级标签结构       | 0–25 整数，0 表示健康 | 顺序性强，已通过回归建模               |

---

## ✅ 总结与建议

- ✅ 等级任务表现稳定，已达到目标；
- ⚠️ 位置任务首次验证 F1 明显上升，值得继续追踪；
- 下一步建议：
- 可视化位置预测混淆矩阵
- 使用数据增强补充位置小类样本
- 启用 EarlyStopping 和自适应学习率策略


  # 🌱 Model Training Log  
📅 Date: 2025-05-08  
👤 Author: Ink  
📂 Project: Corn Disease Multi-Task Recognition (Position + Severity)

---

## 🎯 Current Training Strategy

- Using a ResNet-Plus architecture for multi-task learning:
  - Task 1: Disease position classification (multi-class Softmax)
  - Task 2: Disease severity prediction (regression from 0 to 25)
- Weighted multi-task loss function:
- total_loss = position_loss × 0.7 + grade_loss × 0.3
- - Input: satellite images  
- Labels: from 14l, 14m, 14t datasets

---

## 🎯 Current Phase Objective

**Focus on improving generalization of the position task, while keeping grade task stable**

---

## 📊 Training Metrics

| Metric Name         | Type     | Meaning                                              | Current Values                  | Evaluation     |
|---------------------|----------|------------------------------------------------------|----------------------------------|----------------|
| Position Accuracy   | Classif. | % of correctly predicted position classes            | Train: 69.4%, Val: 43.8%         | 👍 Fair         |
| Position F1 Score   | Classif. | Balance of precision and recall                      | Val: 0.40                        | ✅ Improving    |
| Grade MAE           | Regr.    | Average error in severity grade (0–25)               | Train: 0.47, Val: 1.13           | ✅ Good         |
| Grade ±2 Tolerance  | Regr.    | % of predictions within ±2 of the ground truth label | Val: 100%                        | 🌟 Excellent    |
| Total Loss          | Combined | Weighted sum of position and grade losses            | Decreasing steadily              | ✅ Normal       |

---

## 📌 Dataset Size & Limitations

| Item                  | Current Status      | Notes                                       |
|-----------------------|---------------------|---------------------------------------------|
| Training Set Size     | 62 images           | Small, but grade prediction already stable  |
| Validation Set Size   | 16 images           | Small, metrics may fluctuate                |
| Position Label Balance| Possibly imbalanced | Some position classes never predicted       |
| Grade Label Structure | Integer 0–25, 0 = healthy | Modeled as regression (ordinal meaning) |

---

## ✅ Summary & Recommendations

- ✅ Grade task is well-trained and stable;
- ⚠️ Position task shows promising F1 progress — continue monitoring;
- Next steps:
- Add confusion matrix visualization
- Apply data augmentation to rare position classes
- Use EarlyStopping and learning rate scheduler
