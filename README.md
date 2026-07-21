<h1 align="center">Dexterous Retargeting Guide</h1>

<p align="center">
  <b>IK Retargeting 完整知识体系 — 从大一新生到研究者的灵巧手控制指南</b>
</p>

<p align="center">
  <a href="https://github.com/Dld0621/Dexterous-Retargeting-Guide"><img src="https://img.shields.io/github/stars/Dld0621/Dexterous-Retargeting-Guide?style=flat-square" alt="Stars"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License"></a>
  <a href="https://python.org"><img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python" alt="Python"></a>
  <a href="https://pytorch.org"><img src="https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=flat-square&logo=pytorch" alt="PyTorch"></a>
  <a href="docs/07-key-papers.md"><img src="https://img.shields.io/badge/Papers-20-blueviolet?style=flat-square" alt="Papers"></a>
  <a href="examples/evaluation_framework.py"><img src="https://img.shields.io/badge/Benchmark-5_methods-orange?style=flat-square" alt="Benchmark"></a>
</p>

---

## 这是什么？

**IK Retargeting** 是将人手运动映射到机器人灵巧手的核心技术，是具身智能、遥操作、模仿学习的关键环节。

本项目系统整理了从**基础概念**到**前沿算法**的完整知识体系，并提供**可运行的代码**，让你：

- **零基础上手** — 无需任何机器人背景，跟着文档就能跑通
- **自包含运行** — 核心算法全部内嵌，无需下载额外开源仓库
- **精度最高** — 复现 DexMV (ECCV 2022) 的位置优化方法，论文精度 < 10 mm
- **多模型支持** — Shadow Hand / Allegro Hand / LEAP Hand / O10 灵巧手

---

## 30 秒快速开始

```bash
git clone https://github.com/Dld0621/Dexterous-Retargeting-Guide.git
cd Dexterous-Retargeting-Guide
pip install numpy scipy mujoco matplotlib

# 大一新生 0→1：人手仿真 → 坐标输入 → 机器人重定向
cd examples
python freshman_zero_to_one.py --gesture open --model shadow
```

**输出**：
```
[Step 1/6] 获取人手 21 点坐标
[Step 5/6] DexMV Retargeting (SLSQP + Huber Loss)
  重定向耗时: 0.003s (2.5 ms/帧)
[Step 6/6] 评估重定向精度
  平均 FPE: 61.02 mm
```

> **完成！** 你已经成功将人手坐标重定向到了 Shadow Hand 的 24 个关节。详见 [`docs/12-freshman-zero-to-one.md`](docs/12-freshman-zero-to-one.md)。

---

## 谁适合本项目？

| 人群 | 起点 | 推荐阅读 |
|------|------|---------|
| **大一新生 / 零基础** | 只学过 Python 基础 | [`docs/12-freshman-zero-to-one.md`](docs/12-freshman-zero-to-one.md) |
| **研究生 / 转方向** | 有机器学习/控制基础 | [`docs/11-dexmv-research-guide.md`](docs/11-dexmv-research-guide.md) |
| **工程师 / 快速落地** | 需要直接可用的代码 | [`examples/dexmv_style_retargeting/`](examples/dexmv_style_retargeting/) |
| **研究者 / 深入理解** | 想理解算法原理与对比 | [`docs/02-retargeting-taxonomy.md`](docs/02-retargeting-taxonomy.md) |

---

## 完整学习路径

```
Stage 0: 基础概念 ──────────────────────► 0.5 天
  ├─ 关节/关节角/ctrl 区别        docs/00-joint-concepts.md
  └─ 全概念百科（40+ 概念）         docs/00-concepts-encyclopedia.md

Stage 1: 零到一跑通 ────────────────────► 1 小时
  └─ 人手仿真 + DexMV 重定向        docs/12-freshman-zero-to-one.md

Stage 2: FK/IK 原理 ────────────────────► 0.5 天
  └─ 正逆运动学、Jacobian          tutorials/01-fk-ik-basics/

Stage 3: Rule-based 方法 ───────────────► 1 天
  └─ 角度映射、关节限位             tutorials/02-rule-based-retargeting/

Stage 4: Vector Optimization ───────────► 1-2 天
  └─ scipy 优化、任务空间 IK        tutorials/03-vector-optimization/

Stage 5: 完整 Pipeline ─────────────────► 2-3 天
  └─ 视觉捕捉 → 坐标转换 → 重定向    tutorials/05-complete-pipeline/

Stage 6: 高精度 DexMV ──────────────────► 2 天
  └─ SLSQP + Huber Loss + 时序平滑   docs/11-dexmv-research-guide.md

Stage 7: 评估与对比 ────────────────────► 1 天
  └─ 定量评估框架、方法基准对比      examples/evaluation_framework.py

Stage 8: VLA 视觉-语言-动作 ───────────► 1 天
  └─ SmolVLA 推理 + 四大 VLA 对比    docs/13-vla-zero-to-one.md

进阶: Learning-based ───────────────────► 2-3 天
  └─ NN 映射、Diffusion Policy      docs/05-learning-based-methods.md

实战: 开源项目复现 ─────────────────────► 持续
  └─ AnyTeleop、HaMeR、DexPilot 等   docs/08-open-source-projects.md

硬件: 灵巧手选型 ───────────────────────► 1 天
  └─ LEAP/ORCA/Shadow/Allegro 对比   docs/09-dexterous-hands-analysis.md
```

---

## 项目结构

```
Dexterous-Retargeting-Guide/
├── docs/                              # 15 本核心文档
│   ├── 00-concepts-encyclopedia.md    # 全概念百科（控制理论/硬件/Sim-to-Real）
│   ├── 00-joint-concepts.md           # 关节、关节角、Ctrl 核心概念
│   ├── 01-what-is-ik-retargeting.md   # Retargeting 核心概念与问题定义
│   ├── 02-retargeting-taxonomy.md     # 方法分类体系（Rule / Opt / Learning）
│   ├── 03-human-hand-to-robot-hand.md # 人手→机器人手映射详解
│   ├── 04-optimization-methods.md     # 优化方法深入（Jacobian、阻尼 LS）
│   ├── 05-learning-based-methods.md   # 基于学习的方法（NN、Diffusion）
│   ├── 06-evaluation-metrics.md       # 评估指标与基准
│   ├── 07-key-papers.md               # 20 篇关键论文导读
│   ├── 08-open-source-projects.md     # 8 个优质开源项目复现指南
│   ├── 09-dexterous-hands-analysis.md # 开源灵巧手对比（LEAP/ORCA/Shadow/Allegro）
│   ├── 10-manipulation-datasets.md    # 灵巧操作数据集
│   ├── 11-dexmv-research-guide.md     # DexMV 高精度 IK Retargeting 研究指南
│   ├── 12-freshman-zero-to-one.md     # 大一新生 0→1 实战指南
│   └── 13-vla-zero-to-one.md          # VLA 视觉-语言-动作模型实战
│
├── examples/                          # 9 个可运行示例（全部自包含）
│   ├── freshman_zero_to_one.py        # 大一新生 0→1：人手仿真 + DexMV 重定向
│   ├── vla_demo.py                    # VLA 推理演示（SmolVLA/ALOHA）
│   ├── fk_ik_demo.py                  # 2D 正逆运动学动画
│   ├── finger_chain_3d.py             # 3D 手指链 FK/IK（DH 参数）
│   ├── landmark_to_joint.py           # 21 点 → 关节角 Rule-based 映射
│   ├── minimal_retargeting.py         # 三种 retargeting 方法对比
│   ├── evaluation_framework.py        # 综合评估框架 + 基准对比
│   ├── complete_retargeting_pipeline.py   # 完整 0→1 Pipeline
│   └── dexmv_style_retargeting/       # DexMV 高精度 IK Retargeting（ECCV 2022）
│       ├── dexmv_retargeting.py       # 核心算法：SLSQP + Huber Loss
│       ├── run_pipeline.py            # 完整 pipeline
│       └── README.md                  # 算法详解
│
├── tutorials/                         # 5 阶段教程
│   ├── 01-fk-ik-basics/               # 正逆运动学基础
│   ├── 02-rule-based-retargeting/     # Rule-based 方法实践
│   ├── 03-vector-optimization/        # 向量优化方法
│   ├── 04-landmark-pipeline/          # 21 点 landmark 完整流程
│   └── 05-complete-pipeline/          # 0→1 完整 Pipeline
│
├── pretrained/                        # 预训练模型 + URDF 模型
│   ├── README.md                      # 文件清单与下载说明
│   ├── download_models.py             # 模型自动下载（断点续传）
│   ├── hamer/                         # HaMeR (CVPR 2024) 模型
│   ├── anyteleop/                     # AnyTeleop (RSS 2023) 模型
│   └── urdf/                          # Shadow / Allegro / Franka / ORCA / LEAP
│
├── datasets/                          # 数据集下载脚本
│   └── download_datasets.py           # DexGraspNet/robomimic/LIBERO 等
│
├── setup/
│   └── environment.yml                # Conda 环境配置
│
└── resources/
    └── README.md                      # 数据集/工具/机器人模型索引
```

---

## 文档速查表

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **零基础，想先跑通** | [`docs/12-freshman-zero-to-one.md`](docs/12-freshman-zero-to-one.md) | ⭐ |
| **DexMV 算法详解** | [`docs/11-dexmv-research-guide.md`](docs/11-dexmv-research-guide.md) | ⭐⭐⭐ |
| **VLA 视觉-语言-动作** | [`docs/13-vla-zero-to-one.md`](docs/13-vla-zero-to-one.md) | ⭐⭐ |
| **所有相关概念** | [`docs/00-concepts-encyclopedia.md`](docs/00-concepts-encyclopedia.md) | 参考 |
| **Retargeting 是什么** | [`docs/01-what-is-ik-retargeting.md`](docs/01-what-is-ik-retargeting.md) | ⭐⭐ |
| **三种方法对比** | [`docs/02-retargeting-taxonomy.md`](docs/02-retargeting-taxonomy.md) | ⭐⭐ |
| **人手→机器人映射** | [`docs/03-human-hand-to-robot-hand.md`](docs/03-human-hand-to-robot-hand.md) | ⭐⭐ |
| **优化方法深入** | [`docs/04-optimization-methods.md`](docs/04-optimization-methods.md) | ⭐⭐⭐ |
| **学习方法** | [`docs/05-learning-based-methods.md`](docs/05-learning-based-methods.md) | ⭐⭐⭐ |
| **评估指标** | [`docs/06-evaluation-metrics.md`](docs/06-evaluation-metrics.md) | ⭐⭐ |
| **关键论文** | [`docs/07-key-papers.md`](docs/07-key-papers.md) | ⭐⭐⭐ |
| **开源项目** | [`docs/08-open-source-projects.md`](docs/08-open-source-projects.md) | ⭐⭐ |
| **灵巧手对比** | [`docs/09-dexterous-hands-analysis.md`](docs/09-dexterous-hands-analysis.md) | ⭐⭐ |
| **操作数据集** | [`docs/10-manipulation-datasets.md`](docs/10-manipulation-datasets.md) | 参考 |

---

## 核心概念速查

| 概念 | 一句话解释 |
|------|-----------|
| **Joint（关节）** | 连接刚体的运动副，定义"能怎么动" |
| **Joint Angle** | 关节当前状态量，从编码器读取或从 landmarks 计算 |
| **Ctrl（控制量）** | 发给执行器的指令，position actuator 中 ctrl = 目标关节角 |
| **Retargeting** | 将源运动（人手）映射到目标运动（机器人手） |
| **FK** | 已知关节角 → 计算末端位置（正向） |
| **IK** | 已知末端位置 → 求解关节角（逆向） |
| **21 点模型** | 人手 21 个关键点：手腕 + 5 指 × 4 关节 |
| **Huber Loss** | Smooth L1：小误差二次惩罚，大误差线性惩罚（鲁棒） |
| **SLSQP** | 序列最小二乘规划，带约束的数值优化方法 |
| **Jacobian** | 末端位置对关节角的偏导数矩阵 |
| **Warm-start** | 用上一帧优化结果作为下一帧初始值，加速收敛 |
| **FPE** | Fingertip Position Error，重定向精度核心指标 |

---

## 代码速查

```bash
# === 大一新生 0→1（推荐首次运行）===
cd examples
python freshman_zero_to_one.py --gesture open --model shadow
python freshman_zero_to_one.py --gesture open --visualize-human --visualize-robot

# === FK/IK 基础 ===
python examples/fk_ik_demo.py --mode fk
python examples/fk_ik_demo.py --mode ik
python examples/finger_chain_3d.py --mode ik

# === Rule-based Retargeting ===
python examples/landmark_to_joint.py --hand right --gesture open

# === 三种方法对比 ===
python examples/minimal_retargeting.py --method compare

# === DexMV 高精度 Retargeting ===
cd examples/dexmv_style_retargeting
python run_pipeline.py --model shadow --n_frames 60 --visualize

# === 完整 Pipeline ===
python examples/complete_retargeting_pipeline.py --method all --visualize

# === 评估框架 ===
python examples/evaluation_framework.py --method all --n_samples 100

# === VLA 推理演示 ===
python examples/vla_demo.py --mode synthetic --task "pick up the apple"
python examples/vla_demo.py --mode aloha --episode 0  # 需要 GPU
```

---

## 支持的手模型

| 模型 | DOF | 指数量 | 文件 | 状态 |
|------|-----|--------|------|------|
| **Shadow Hand** | 24 | 5 | `pretrained/urdf/mujoco_menagerie/shadow_hand/` | ✅ 完整支持 |
| **Allegro Hand** | 16 | 4 | `pretrained/urdf/allegro_hand_right/` | ✅ 完整支持 |
| **LEAP Hand** | 16 | 4 | `pretrained/urdf/leap_hand_sim/` | ✅ 完整支持 |
| **O10 / OmniHand** | 10 | 5 | 外部硬件 | ⚠️ 需真机 |

---

## 优质开源项目推荐

| 项目 | 方向 | 开源 | 核心学习点 |
|------|------|------|-----------|
| [DexMV](https://github.com/yzqin/dexmv-sim) | IK Retargeting | ✅ | SLSQP + Huber Loss，**精度最高** |
| [AnyTeleop](https://github.com/dexsuite/dex-teleop) | 遥操作框架 | ❌ | MediaPipe → IK → Shadow Hand |
| [HaMeR](https://github.com/geopavlakos/hamer) | 手部 mesh 恢复 | ✅ | 单 RGB → MANO 参数化模型 |
| [LEAP Hand](https://github.com/leap-hand/LEAP_Hand_Sim) | 低成本灵巧手 | ✅ | 16-DOF URDF / Isaac Gym |
| [DexPilot](https://github.com/byte-dance/dexpilot) | 视觉遥操作 | ❌ | 点云直接优化 |
| [RDT-1B](https://github.com/thu-ml/RDT-1B) | 双臂扩散策略 | ✅ | Diffusion Transformer |
| [DexGraspNet](https://github.com/PKU-EPIC/DexGraspNet) | 灵巧抓取数据集 | ✅ | 百万级抓取 + 物理筛选 |

> 完整 8 个项目复现指南见 [`docs/08-open-source-projects.md`](docs/08-open-source-projects.md)

---

## 外部学习资源

### 教材与课程

| 资源 | 类型 | 说明 |
|------|------|------|
| [Modern Robotics (Lynch & Park)](http://hades.mech.northwestern.edu/index.php/Modern_Robotics) | 教材 | 刚体运动学、Jacobian、开链/闭链系统 |
| [MuJoCo Menagerie](https://github.com/google-deepmind/mujoco_menagerie) | 代码 | 预构建机器人模型库（含 Shadow、Allegro） |
| [Diffusion Policy 官方教程](https://diffusion-policy.cs.columbia.edu/) | 教程 | 扩散策略从原理到代码 |

### 相关项目

- [Embodied-AI-Paper-Analysis](https://github.com/Dld0621/Embodied-AI-Paper-Analysis) — 具身智能论文体系化梳理
- [Embodied-AI-Zero-to-Hero](https://github.com/Dld0621/Embodied-AI-Zero-to-Hero) — VLA / RL / 世界模型入门

---

## 贡献指南

欢迎提交 Issue 和 PR！支持方向：

- 补充新的 retargeting 方法（Diffusion Policy、强化学习等）
- 添加更多机器人手模型（Inspire Hand、SVH 等）
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
- [DexMV](https://github.com/yzqin/dexmv-sim) — ECCV 2022 高精度 IK Retargeting
