<h1 align="center">Dexterous Retargeting Guide</h1>

<p align="center">
  <b>灵巧手重定向完整知识体系 — 从大一新生零基础到 2026 前沿研究</b>
</p>

<p align="center">
  <a href="https://github.com/Dld0621/Dexterous-Retargeting-Guide"><img src="https://img.shields.io/github/stars/Dld0621/Dexterous-Retargeting-Guide?style=flat-square" alt="Stars"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License"></a>
  <a href="https://python.org"><img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python" alt="Python"></a>
  <a href="https://pytorch.org"><img src="https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=flat-square&logo=pytorch" alt="PyTorch"></a>
  <a href="docs/07-key-papers.md"><img src="https://img.shields.io/badge/Papers-37-blueviolet?style=flat-square" alt="Papers"></a>
  <a href="#项目结构"><img src="https://img.shields.io/badge/Docs-20-success?style=flat-square" alt="Docs"></a>
  <a href="examples/evaluation_framework.py"><img src="https://img.shields.io/badge/Benchmark-5_methods-orange?style=flat-square" alt="Benchmark"></a>
</p>

<p align="center">
  <b>20 本文档 · 11 个可运行示例 · 37 篇关键论文导读 · 80+ 篇 Arxiv 扫描 · 12+ 篇前沿论文在线链接</b>
</p>

---

## 这是什么？

**IK Retargeting** 是将人手运动映射到机器人灵巧手的核心技术，是具身智能、遥操作、模仿学习的关键环节。

本项目系统整理了从**基础概念**到**2026 前沿研究**的完整知识体系，覆盖四大支柱：

| 支柱 | 内容 | 代表文档 |
|------|------|---------|
| **基础教程** | 关节概念 → FK/IK → 三种重定向方法 → 完整 Pipeline | `docs/00` ~ `docs/06` |
| **可运行代码** | 11 个自包含示例，无需下载额外开源仓库 | `examples/` |
| **前沿论文** | 37 篇关键论文导读 + 80+ 篇 Arxiv 扫描 + 12+ 篇在线链接 | `docs/07`, `docs/16`, `docs/18` |
| **研究定位** | 2026 后几何时代研究趋势分析 + 竞争关系 + 研究路线 | `docs/17` |

**核心特色**：

- **零基础上手** — 无需任何机器人背景，跟着文档就能跑通
- **自包含运行** — 核心算法全部内嵌，无需下载额外开源仓库
- **精度最高** — 复现 DexMV (ECCV 2022) 的位置优化方法，论文精度 < 10 mm
- **前沿覆盖** — 追踪到 2026 年 7 月最新论文，含 TopoRetarget、SPIDER、AnyDexRT 等
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
| **前沿追踪者** | 想了解 2026 最新进展 | [`docs/17-research-trends-and-positioning.md`](docs/17-research-trends-and-positioning.md) |

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

Stage 9: RL 强化学习 ───────────────────► 2 天
  └─ SAC + HER + Shadow Hand 灵巧操作  docs/14-rl-zero-to-one.md

Stage 10: 世界模型 ─────────────────────► 2 天
  └─ DreamerV3 RSSM + 想象力规划    docs/15-world-model-zero-to-one.md

Stage 11: 前沿研究 ─────────────────────► 持续
  ├─ 2026 研究趋势与定位             docs/17-research-trends-and-positioning.md
  ├─ 前沿论文在线链接                 docs/18-frontier-papers-online.md
  ├─ Arxiv 论文全景扫描               docs/16-arxiv-retargeting-scan.md
  └─ 37 篇关键论文导读                docs/07-key-papers.md

进阶: Learning-based ───────────────────► 2-3 天
  └─ NN 映射、Diffusion Policy      docs/05-learning-based-methods.md

实战: 开源项目复现 ─────────────────────► 持续
  └─ SPIDER、DexMachina、AnyTeleop 等  docs/08-open-source-projects.md

硬件: 灵巧手选型 ───────────────────────► 1 天
  └─ LEAP/ORCA/Shadow/Allegro 对比   docs/09-dexterous-hands-analysis.md
```

---

## 2026 前沿研究亮点

> 2026 年灵巧手重定向已进入**"后几何时代"**——从 `human pose → robot pose` 转向 `human intent + morphology + object interaction + physics → robot-feasible action`。

### 六大研究转向

| 转向 | 传统方法 | 2026 新方向 | 代表论文 |
|------|---------|------------|---------|
| 几何 → 物理 | 纯运动学优化 | 物理可行性约束 | [SPIDER](https://arxiv.org/abs/2511.09484) |
| 标定 → 免标定 | 手工规则校准 | 少量示范自适应 | [AnyDexRT](https://arxiv.org/abs/2607.08341) |
| 姿态 → 交互 | 指尖位置匹配 | 手-物接触拓扑保持 | [TopoRetarget](https://arxiv.org/abs/2606.16272) |
| 统一 → 条件化 | 单一映射 | 手势/阶段条件化 | [VTAP Gripper](https://arxiv.org/abs/2607.15448) |
| 在线 → 推理 | 每帧迭代求解 | 学习后前向推理 | [GeoRT](https://arxiv.org/abs/2503.07541) |
| 关节 → 功能 | 复制关节形状 | 功能等效 | [DexMachina](https://arxiv.org/abs/2505.24853) |

### 有开源代码的前沿项目

| 项目 | 代码 | 核心方法 |
|------|------|---------|
| [SPIDER](https://github.com/facebookresearch/spider) | Meta FAIR | 物理感知重定向，运动学→动力学可行轨迹 |
| [DexMachina](https://github.com/MandiZhao/dexmachina) | Stanford+NVIDIA | 功能性重定向，课程学习跨 embodiment |
| [TopoRetarget](https://toporetarget2026.github.io/TopoRetarget/) | 清华 IIIS | 交互保持，距离加权拉普拉斯优化 |
| [EquiDexFlow](https://equidexflow.github.io/) | — | SE(3) 等变生成流，接触+法向+力学联合预测 |

> 完整 12+ 篇前沿论文在线链接见 [`docs/18-frontier-papers-online.md`](docs/18-frontier-papers-online.md)

---

## 项目结构

```
Dexterous-Retargeting-Guide/
├── docs/                              # 20 本核心文档
│   ├── 00-concepts-encyclopedia.md    # 全概念百科（控制理论/硬件/Sim-to-Real）
│   ├── 00-joint-concepts.md           # 关节、关节角、Ctrl 核心概念
│   ├── 01-what-is-ik-retargeting.md   # Retargeting 核心概念与问题定义
│   ├── 02-retargeting-taxonomy.md     # 方法分类体系（Rule / Opt / Learning）
│   ├── 03-human-hand-to-robot-hand.md # 人手→机器人手映射详解
│   ├── 04-optimization-methods.md     # 优化方法深入（Jacobian、阻尼 LS）
│   ├── 05-learning-based-methods.md   # 基于学习的方法（NN、Diffusion）
│   ├── 06-evaluation-metrics.md       # 评估指标与基准
│   ├── 07-key-papers.md               # 37 篇关键论文导读（含 2026 最新进展 + 在线链接）
│   ├── 08-open-source-projects.md     # 8 个优质开源项目复现指南
│   ├── 09-dexterous-hands-analysis.md # 开源灵巧手对比（LEAP/ORCA/Shadow/Allegro）
│   ├── 10-manipulation-datasets.md    # 灵巧操作数据集
│   ├── 11-dexmv-research-guide.md     # DexMV 高精度 IK Retargeting 研究指南
│   ├── 12-freshman-zero-to-one.md     # 大一新生 0→1 实战指南
│   ├── 13-vla-zero-to-one.md          # VLA 视觉-语言-动作模型实战
│   ├── 14-rl-zero-to-one.md           # RL 强化学习：SAC+HER Shadow Hand
│   ├── 15-world-model-zero-to-one.md  # 世界模型：DreamerV3 + 想象力规划
│   ├── 16-arxiv-retargeting-scan.md   # Arxiv Retargeting 论文全景扫描（200篇→80+精选）
│   ├── 17-research-trends-and-positioning.md  # 2026 研究趋势与定位分析（后几何时代）
│   └── 18-frontier-papers-online.md   # 前沿论文在线资源目录（12+ 篇 arXiv/代码/项目页）
│
├── examples/                          # 11 个可运行示例（全部自包含）
│   ├── freshman_zero_to_one.py        # 大一新生 0→1：人手仿真 + DexMV 重定向
│   ├── vla_demo.py                    # VLA 推理演示（SmolVLA/ALOHA）
│   ├── rl_demo.py                     # RL 演示（Q-learning / SAC+HER）
│   ├── world_model_demo.py            # 世界模型演示（线性模型 + MPC）
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

### 入门与基础

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **零基础，想先跑通** | [`docs/12-freshman-zero-to-one.md`](docs/12-freshman-zero-to-one.md) | ⭐ |
| **关节/关节角/Ctrl 区别** | [`docs/00-joint-concepts.md`](docs/00-joint-concepts.md) | ⭐ |
| **所有相关概念百科** | [`docs/00-concepts-encyclopedia.md`](docs/00-concepts-encyclopedia.md) | 参考 |
| **Retargeting 是什么** | [`docs/01-what-is-ik-retargeting.md`](docs/01-what-is-ik-retargeting.md) | ⭐⭐ |
| **三种方法对比** | [`docs/02-retargeting-taxonomy.md`](docs/02-retargeting-taxonomy.md) | ⭐⭐ |
| **人手→机器人映射** | [`docs/03-human-hand-to-robot-hand.md`](docs/03-human-hand-to-robot-hand.md) | ⭐⭐ |

### 算法与实现

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **DexMV 算法详解** | [`docs/11-dexmv-research-guide.md`](docs/11-dexmv-research-guide.md) | ⭐⭐⭐ |
| **优化方法深入** | [`docs/04-optimization-methods.md`](docs/04-optimization-methods.md) | ⭐⭐⭐ |
| **学习方法** | [`docs/05-learning-based-methods.md`](docs/05-learning-based-methods.md) | ⭐⭐⭐ |
| **评估指标** | [`docs/06-evaluation-metrics.md`](docs/06-evaluation-metrics.md) | ⭐⭐ |

### 扩展领域

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **VLA 视觉-语言-动作** | [`docs/13-vla-zero-to-one.md`](docs/13-vla-zero-to-one.md) | ⭐⭐ |
| **RL 强化学习** | [`docs/14-rl-zero-to-one.md`](docs/14-rl-zero-to-one.md) | ⭐⭐⭐ |
| **世界模型** | [`docs/15-world-model-zero-to-one.md`](docs/15-world-model-zero-to-one.md) | ⭐⭐⭐ |

### 前沿研究

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **2026 研究趋势与定位** | [`docs/17-research-trends-and-positioning.md`](docs/17-research-trends-and-positioning.md) | ⭐⭐⭐ |
| **前沿论文在线链接** | [`docs/18-frontier-papers-online.md`](docs/18-frontier-papers-online.md) | 参考 |
| **37 篇关键论文导读** | [`docs/07-key-papers.md`](docs/07-key-papers.md) | ⭐⭐⭐ |
| **Arxiv 论文全景扫描** | [`docs/16-arxiv-retargeting-scan.md`](docs/16-arxiv-retargeting-scan.md) | 参考 |

### 硬件与资源

| 你想了解 | 文档 | 难度 |
|---------|------|------|
| **灵巧手对比** | [`docs/09-dexterous-hands-analysis.md`](docs/09-dexterous-hands-analysis.md) | ⭐⭐ |
| **开源项目复现** | [`docs/08-open-source-projects.md`](docs/08-open-source-projects.md) | ⭐⭐ |
| **操作数据集** | [`docs/10-manipulation-datasets.md`](docs/10-manipulation-datasets.md) | 参考 |

---

## 核心概念速查

### 基础概念

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

### 2026 前沿概念

| 概念 | 一句话解释 | 代表论文 |
|------|-----------|---------|
| **Interaction-Preserving** | 不只匹配指尖位置，更保持手-物接触拓扑关系 | TopoRetarget |
| **Physics-Informed** | 运动学可行 ≠ 物理可行，需纳入碰撞/力/接触约束 | SPIDER |
| **Calibration-Free** | 免标定，少量人工示范即可适配新操作者/新机器人 | AnyDexRT |
| **Functional Retargeting** | 不追求关节形状复制，追求功能等效 | DexMachina |
| **Gesture-Conditioned** | 根据手势/操作阶段切换不同重定向映射 | VTAP Gripper |
| **Morphology Gap** | 人手与机器人手的形态差异，从误差因素升级为对齐变量 | DexGrasp-Zero |
| **Cross-Embodiment** | 跨机器人形态泛化，训练于 A 手迁移到 B 手 | One-Policy-Fits-All |

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

# === RL 强化学习 ===
python examples/rl_demo.py --mode demo     # numpy Q-learning（无需安装）
python examples/rl_demo.py --mode train    # SAC+HER Shadow Hand（需要GPU）

# === 世界模型 ===
python examples/world_model_demo.py --mode concept  # numpy 线性模型 + MPC
python examples/world_model_demo.py --mode dreamer  # DreamerV3 推理（需要GPU）
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

### 经典项目

| 项目 | 方向 | 开源 | 核心学习点 |
|------|------|------|-----------|
| [DexMV](https://github.com/yzqin/dexmv-sim) | IK Retargeting | ✅ | SLSQP + Huber Loss，**精度最高** |
| [AnyTeleop](https://github.com/dexsuite/dex-teleop) | 遥操作框架 | ❌ | MediaPipe → IK → Shadow Hand |
| [HaMeR](https://github.com/geopavlakos/hamer) | 手部 mesh 恢复 | ✅ | 单 RGB → MANO 参数化模型 |
| [LEAP Hand](https://github.com/leap-hand/LEAP_Hand_Sim) | 低成本灵巧手 | ✅ | 16-DOF URDF / Isaac Gym |
| [DexPilot](https://github.com/byte-dance/dexpilot) | 视觉遥操作 | ❌ | 点云直接优化 |
| [RDT-1B](https://github.com/thu-ml/RDT-1B) | 双臂扩散策略 | ✅ | Diffusion Transformer |
| [DexGraspNet](https://github.com/PKU-EPIC/DexGraspNet) | 灵巧抓取数据集 | ✅ | 百万级抓取 + 物理筛选 |

### 2026 前沿项目（有代码/项目页）

| 项目 | 方向 | 代码 | 核心方法 |
|------|------|------|---------|
| [SPIDER](https://github.com/facebookresearch/spider) | 物理感知重定向 | ✅ | Meta FAIR，运动学→动力学可行轨迹 |
| [DexMachina](https://github.com/MandiZhao/dexmachina) | 功能性重定向 | ✅ | Stanford+NVIDIA，课程学习跨 embodiment |
| [TopoRetarget](https://toporetarget2026.github.io/TopoRetarget/) | 交互保持重定向 | 项目页 | 清华 IIIS，距离加权拉普拉斯优化 |
| [EquiDexFlow](https://equidexflow.github.io/) | 接触感知生成流 | 项目页 | SE(3) 等变，接触+法向+力学联合预测 |

> 完整复现指南见 [`docs/08-open-source-projects.md`](docs/08-open-source-projects.md)，前沿论文在线链接见 [`docs/18-frontier-papers-online.md`](docs/18-frontier-papers-online.md)

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
- 补充前沿论文在线链接和代码复现指南
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
- [SPIDER](https://github.com/facebookresearch/spider) — Meta FAIR 物理感知重定向
- [DexMachina](https://github.com/MandiZhao/dexmachina) — Stanford+NVIDIA 功能性重定向
- [TopoRetarget](https://toporetarget2026.github.io/TopoRetarget/) — 清华 IIIS 交互保持重定向
