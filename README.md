<h1 align="center">Dexterous Retargeting Guide</h1>

<p align="center">
  <b>IK Retargeting 知识体系 — 从人手视觉捕捉到机器人灵巧手控制的完整指南</b>
</p>

<p align="center">
  <a href="https://github.com/Dld0621/Dexterous-Retargeting-Guide"><img src="https://img.shields.io/github/stars/Dld0621/Dexterous-Retargeting-Guide?style=flat-square" alt="Stars"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License"></a>
  <a href="https://python.org"><img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python" alt="Python"></a>
  <a href="https://pytorch.org"><img src="https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=flat-square&logo=pytorch" alt="PyTorch"></a>
  <a href="docs/07-key-papers.md"><img src="https://img.shields.io/badge/Papers-25+-blueviolet?style=flat-square" alt="Papers"></a>
  <a href="examples/evaluation_framework.py"><img src="https://img.shields.io/badge/Benchmark-5_methods-orange?style=flat-square" alt="Benchmark"></a>
</p>

---

## 这是什么？

**IK Retargeting** 是将源运动（如人手）映射到目标运动（如机器人灵巧手）的技术，是具身智能中人-机遥操作的核心环节。本项目系统整理 IK Retargeting 的完整知识体系：

<table align="center">
<tr><td colspan="3" align="center"><b>IK Retargeting Pipeline</b></td></tr>
<tr>
<td align="center" width="30%">
<b>人手视觉捕捉</b><br>
21点坐标<br>
弯曲/外展角<br>
局部坐标系
</td>
<td align="center" width="30%">
<b>Retargeting</b><br>
Rule-based<br>
Optimization<br>
Learning-based
</td>
<td align="center" width="30%">
<b>机器人控制</b><br>
关节角度<br>
末端位姿<br>
力矩控制
</td>
</tr>
<tr><td colspan="3" align="center">
<b>基础知识</b>: 正/逆运动学 (FK/IK) · 动作表示 · 映射方法 · 评估指标
</td></tr>
</table>

---

## 学完能达到什么水平？

- **理解** 人手 21 点 landmarks 与机器人关节空间的映射原理
- **掌握** Rule-based、Vector Optimization、Learning-based 三种 retargeting 方法
- **实现** 从 MediaPipe/InterHand 视觉捕捉到 O10/Shadow Hand 灵巧手控制的全流程
- **处理** 左右手镜像、关节限位、任务空间失真等工程问题
- **评估** retargeting 精度并选择适合场景的映射策略

---

## 知识地图

| 主题 | 子方向 | 关键技术 | 学习资源 |
|------|--------|---------|---------|
| **基础** | 正运动学 | DH 参数、变换矩阵、关节链 | [FK/IK 教程](tutorials/01-fk-ik-basics/README.md) |
| | 逆运动学 | 解析法、数值法、Jacobian、阻尼最小二乘 | [FK/IK 教程](tutorials/01-fk-ik-basics/README.md) |
| | 动作表示 | 关节角、末端位姿、delta、action chunking | [Retargeting 概念](docs/01-what-is-ik-retargeting.md) |
| **人手捕捉** | 视觉检测 | MediaPipe、InterHand、YOLO-pose | [Landmark Pipeline](tutorials/04-landmark-pipeline/README.md) |
| | 关键点定义 | 21 点模型、MCP/PIP/DIP、弯曲角/外展角 | [人手→机器人手映射](docs/03-human-hand-to-robot-hand.md) |
| | 坐标系处理 | 手腕原点、局部坐标、左右手镜像 | [人手→机器人手映射](docs/03-human-hand-to-robot-hand.md) |
| **Retargeting** | Rule-based | 角度映射、分段线性、关节限位 | [Rule-based 教程](tutorials/02-rule-based-retargeting/README.md) |
| | Optimization | scipy least_squares、向量优化、任务空间 IK | [Vector Optimization 教程](tutorials/03-vector-optimization/README.md) |
| | Learning-based | NN 映射、VAE、Diffusion Policy | [Retargeting 方法分类](docs/02-retargeting-taxonomy.md) |
| **评估** | 精度指标 | 关节误差、指尖误差、手势相似度 | [方法分类体系](docs/02-retargeting-taxonomy.md) |
| | 工程问题 | 振荡抑制、插值失真、Sim-to-Real | [人手→机器人手映射](docs/03-human-hand-to-robot-hand.md) |

---

## 学习路径

| 阶段 | 主题 | 目标 | 可运行代码 | 预计时间 |
|------|------|------|-----------|---------|
| **Stage 1** | FK/IK 基础 | 理解关节链、正逆运动学、Jacobian | `fk_ik_demo.py` | 0.5-1 天 |
| **Stage 2** | Rule-based Retargeting | 角度映射、分段线性、关节限位 | `landmark_to_joint.py` | 1 天 |
| **Stage 3** | Vector Optimization | scipy 优化、任务空间 IK、拇指校准 | `minimal_retargeting.py` | 1-2 天 |
| **Stage 4** | 完整 Pipeline | 视觉捕捉 → 坐标转换 → Retargeting → 仿真 | `landmark_to_joint.py` + `finger_chain_3d.py` | 2-3 天 |
| **Stage 5** | 评估与对比 | 定量评估框架、多种方法基准对比 | `evaluation_framework.py` | 1 天 |
| **进阶** | Learning-based | NN 映射、Diffusion Policy、端到端学习 | `docs/05-learning-based-methods.md` | 2-3 天 |

---

## 仓库结构

```
Dexterous-Retargeting-Guide/
├── docs/                              # 7 本核心文档
│   ├── 01-what-is-ik-retargeting.md   # Retargeting 核心概念与问题定义
│   ├── 02-retargeting-taxonomy.md     # 方法分类体系（Rule / Opt / Learning）
│   ├── 03-human-hand-to-robot-hand.md # 人手→机器人手映射详解
│   ├── 04-optimization-methods.md     # 优化方法深入（Jacobian、阻尼 LS）
│   ├── 05-learning-based-methods.md   # 基于学习的方法（NN、Diffusion）
│   ├── 06-evaluation-metrics.md       # 评估指标与基准
│   └── 07-key-papers.md               # 25+ 篇关键论文导读
├── tutorials/                         # 4 阶段教程
│   ├── 01-fk-ik-basics/              # 正逆运动学基础
│   ├── 02-rule-based-retargeting/    # Rule-based 方法实践
│   ├── 03-vector-optimization/       # 向量优化方法
│   └── 04-landmark-pipeline/         # 21 点 landmark 完整流程
├── examples/                          # 5 个可运行示例
│   ├── fk_ik_demo.py                 # 2D 正逆运动学动画
│   ├── finger_chain_3d.py            # 3D 手指链 FK/IK（DH 参数）
│   ├── landmark_to_joint.py          # 21 点 → 关节角 Rule-based 映射
│   ├── minimal_retargeting.py        # 三种 retargeting 方法对比
│   └── evaluation_framework.py       # 综合评估框架 + 基准对比
├── setup/environment.yml             # Conda 环境
└── resources/README.md               # 数据集/工具/机器人模型索引
```

---

## 快速开始

```bash
git clone https://github.com/Dld0621/Dexterous-Retargeting-Guide.git
cd Dexterous-Retargeting-Guide
conda env create -f setup/environment.yml && conda activate retargeting

# Stage 1: FK/IK 基础（无需 GPU）
python examples/fk_ik_demo.py --mode fk
python examples/fk_ik_demo.py --mode ik
python examples/finger_chain_3d.py --mode fk   # 3D 手指链

# Stage 2: Rule-based retargeting
python examples/landmark_to_joint.py --hand right --gesture open
python examples/landmark_to_joint.py --hand left --gesture fist

# Stage 3: 三种方法对比
python examples/minimal_retargeting.py --method compare

# Stage 4: 3D IK 求解
python examples/finger_chain_3d.py --mode ik

# Stage 5: 综合评估 + 基准对比
python examples/evaluation_framework.py --method all --n_samples 100
```

---

## 核心概念速查

| 概念 | 一句话 |
|------|--------|
| **Retargeting** | 将源运动（人手）映射到目标运动（机器人手） |
| **FK** | 已知关节角 → 计算末端位置（正向） |
| **IK** | 已知末端位置 → 求解关节角（逆向） |
| **21 点模型** | 人手 21 个关键点（手腕 + 5 指 × 4 关节） |
| **弯曲角** | 相邻关键点向量夹角，反映手指卷曲程度 |
| **外展角** | 相邻手指根部方向向量夹角，反映手指张开程度 |
| **Rule-based** | 直接角度映射，简单稳定但泛化性差 |
| **Vector Opt** | 优化 landmark 位置匹配，补偿尺寸差异 |
| **Learning-based** | 神经网络端到端映射，数据驱动 |
| **Action Chunking** | 一次预测多步动作，减少推理延迟 |

---

## 推荐工具与模型

| 名称 | 类型 | 说明 |
|------|------|------|
| MediaPipe Hands | 视觉检测 | Google 开源 21 点手部检测，实时 |
| InterHand2.6M | 数据集 | 双手交互 3D 姿态数据集 |
| MuJoCo | 仿真 | 物理仿真，支持灵巧手模型 |
| O10/OmniHand | 机器人 | 10-DOF 主动关节灵巧手 |
| Shadow Hand | 机器人 | 20-DOF 类人灵巧手 |
| Leap Motion | 传感器 | 深度手部追踪设备 |

---

## 相关项目

- [Embodied-AI-Paper-Analysis](https://github.com/Dld0621/Embodied-AI-Paper-Analysis) — 具身智能论文体系化梳理
- [VLA-Zero-to-Hero](https://github.com/Dld0621/VLA-Zero-to-Hero) — VLA / RL / 世界模型入门指南
- [RobotDev-Setup-Guide](https://github.com/Dld0621/RobotDev-Setup-Guide) — 机器人开发环境搭建

---

## 贡献指南

欢迎提交 Issue 和 PR！支持方向：

- 补充新的 retargeting 方法（Diffusion Policy、强化学习等）
- 添加更多机器人手模型（Leap Hand、Allegro 等）
- 完善评估基准和对比实验
- 修正文档错误、分享学习笔记

---

## 许可证

[MIT License](LICENSE)

---

## Acknowledgments

- [MediaPipe](https://mediapipe-studio.webapps.google.com/demo/hand_landmarker) — 实时手部 landmarks 检测
- [InterHand2.6M](https://mks0601.github.io/InterHand2.6M/) — 双手 3D 姿态数据集
- [MuJoCo Menagerie](https://github.com/google-deepmind/mujoco_menagerie) — 机器人模型库
- [Open X-Embodiment](https://robotics-transformer-x.github.io/) — 大规模机器人数据集
