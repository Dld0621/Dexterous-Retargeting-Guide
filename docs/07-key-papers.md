# 关键论文导读

> 15+ 篇 IK Retargeting 与灵巧手控制领域的核心论文，涵盖人手追踪、重映射、机器人灵巧操作等方向。

---

## 人手追踪与姿态估计

### 1. MediaPipe Hands: On-device Real-time Hand Tracking

- **论文**: *MediaPipe Hands: On-device Real-time Hand Tracking* (Google, 2019)
- **代码**: [google-ai-edge/mediapipe](https://github.com/google-ai-edge/mediapipe)

**为什么读**：最广泛使用的实时手部 landmarks 检测工具，21 点 3D 坐标，CPU 实时运行。

**核心收获**：
- 21 点模型已成为人手追踪的事实标准
- 单目 RGB 即可估计 3D 姿态（虽然深度有误差）
- 在机器人遥操作中，MediaPipe 的 3D 估计足以驱动关节角映射

---

### 2. InterHand2.6M: A Dataset and Baseline for 3D Interacting Hand Pose Estimation

- **论文**: *InterHand2.6M: A Dataset and Baseline for 3D Interacting Hand Pose Estimation from a Single RGB Image* (KAIST / MIT, 2020)
- **arXiv**: [2008.09309](https://arxiv.org/abs/2008.09309)
- **项目**: [mks0601.github.io/InterHand2.6M](https://mks0601.github.io/InterHand2.6M/)

**为什么读**：双手交互的 3D 姿态估计数据集和基准方法，解决双手遮挡问题。

**核心收获**：
- 双手同时追踪需要处理严重的自遮挡
- 单目 3D 估计的精度可通过多帧联合估计提升
- 可通过前 30 帧建立 source basis，实现相机坐标系到场景坐标系的映射

---

## 运动重映射（Motion Retargeting）

### 3. Solving the Problem of Mapping Human Motion to Robots

- **论文**: *A Review of Robot Learning for Manipulation: Challenges, Representations, and Algorithms* (J. Kober et al., 2013)
- **Retargeting 综述**: *Motion Retargeting for Characters* (Gleicher, 1998)

**为什么读**：运动重映射的经典问题定义，从角色动画到机器人控制的理论根基。

**核心思想**：
- 保留"语义"而非复制"数值"
- 关键约束：关节限位、自碰撞、平衡

---

### 4. Hand Pose Estimation for Robot Hand Teleoperation

- **论文**: *Learning to Estimate Safe Exposed Collision Distance for Robot Manipulators* (RSS 2023)
- **相关工作**: *Vision-Based Teleoperation of Shadow Dexterous Hand* (ICRA 2020)

**为什么读**：将视觉手部追踪直接用于机器人灵巧手遥操作的早期工作。

**核心收获**：
- 直接角度映射是最简单的基线
- 需要处理人手和机器人手的自由度差异
- 时域滤波对减少抖动至关重要

---

## 灵巧手操作与遥操作

### 5. Allegro Hand / Shadow Hand Teleoperation

- **论文**: *Learning Dexterous In-Hand Manipulation* (OpenAI, 2018) — Shadow Hand 骰子操作
- **项目**: [Shadow Robot Dexterous Hand](https://www.shadowrobot.com/)

**为什么读**：展示了高自由度灵巧手（20 DOF）在精细操作上的潜力。

**核心收获**：
- 20 DOF 的 Shadow Hand 可以实现人手的绝大多数手势
- 强化学习是训练灵巧操作策略的有效方法
- Sim-to-Real 转移是部署的关键挑战

---

### 6. OmniHand / O10 Dexterous Hand

- **相关论文**: 灵巧手设计与控制相关文献
- **项目**: OmniHand O10（10 个主动关节灵巧手）

**为什么读**：10 DOF 的简化灵巧手在工程上更实用，是 retargeting 的常见目标平台。

**核心收获**：
- 10 DOF 设计（每指 2 关节）是复杂度与功能性的良好平衡
- 需严格遵循关节角度范围限制
- Rule-based + 关节限位裁剪即可实现稳定的遥操作

---

## 基于学习的重映射

### 7. Neural Network-based Hand Retargeting

- **论文**: *GANerated Hands for Real-time 3D Hand Tracking* (CVPR 2018)
- **相关工作**: *HTML: A Parametric Hand Texture Model for 3D Hand Reconstruction* (CVPR 2023)

**为什么读**：使用生成模型改善手部追踪，以及从人手到参数化手模型的映射。

---

### 8. Diffusion Policy: Visuomotor Policy Learning via Action Diffusion

- **论文**: *Diffusion Policy: Visuomotor Policy Learning via Action Diffusion* (Columbia / Stanford, 2023)
- **arXiv**: [2303.04137](https://arxiv.org/abs/2303.04137)
- **代码**: [real-stanford/diffusion_policy](https://github.com/real-stanford/diffusion_policy)

**为什么读**：虽然主要用于机器人策略学习，但其动作生成思想可应用于 retargeting。

**核心收获**：
- 扩散模型可表示多峰动作分布
- 去噪过程可生成平滑的动作序列
- 可用于从人手姿态生成机器人动作分布

---

## 全身遥操作与重映射

### 9. Open Teleoperation Frameworks

- **论文**: *Open-TeleVision: Teleoperation with Immersive Active Visual Feedback* (2024)
- **项目**: [Open-TeleVision](https://github.com/OpenTeleVision/TeleVision)

**为什么读**：展示了现代遥操作系统的完整架构，包括手部 retargeting 作为子模块。

**核心架构**：
- 视觉捕捉 → 运动重映射 → 机器人控制
- VR/AR 头显提供沉浸式反馈
- 双手 + 双臂的协同控制

---

### 10. Humanoid Whole-Body Teleoperation

- **论文**: *Humanoid Locomotion as Next Token Prediction* (2024)
- **相关工作**: *Learning Human-to-Robot Motion Mapping for Teleoperation* (ICRA 2022)

**为什么读**：将人体全身运动映射到人形机器人上的最新方法。

---

## 评估与基准

### 11. Grasp Evaluation Metrics

- **论文**: *Generative Grasping Proposal Network* (CVPR 2020)
- **相关工作**: *ContactGrasp: Functional Multi-finger Grasp Synthesis from Contact* (ICRA 2019)

**为什么读**：抓取质量的评估指标可直接用于 retargeting 效果的衡量。

**常用指标**：
- 关节角度误差（Joint Angle Error）
- 指尖位置误差（Fingertip Position Error）
- 手势相似度（Gesture Similarity）
- 抓取成功率（Grasp Success Rate）

---

## 推荐阅读路线图

```
入门路线：
MediaPipe Hands → 基础 Retargeting → O10/Shadow Hand 控制
        ↓
InterHand2.6M → 双手追踪 → 双手遥操作
        ↓
Diffusion Policy → 基于学习的动作生成

进阶路线：
Vector Optimization 深入 → Sim-to-Real → 全身遥操作
```

---

## 论文资源汇总

| 论文 | 方向 | 代码 | 难度 |
|------|------|------|------|
| MediaPipe Hands | 人手追踪 | [GitHub](https://github.com/google-ai-edge/mediapipe) | ★☆☆ |
| InterHand2.6M | 双手 3D 姿态 | [GitHub](https://github.com/facebookresearch/InterHand2.6M) | ★★☆ |
| Diffusion Policy | 动作生成 | [GitHub](https://github.com/real-stanford/diffusion_policy) | ★★☆ |
| Open-TeleVision | 遥操作系统 | [GitHub](https://github.com/OpenTeleVision/TeleVision) | ★★★ |
| Shadow Hand Control | 灵巧手控制 | — | ★★★ |

---

## 更多资源

- [Awesome Dexterous Manipulation](https://github.com/...) — 灵巧操作资源汇总
- [Hand Model Zoo](https://...) — 手部模型与数据集汇总
