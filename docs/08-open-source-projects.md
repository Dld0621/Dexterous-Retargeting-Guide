# 优质开源项目：从复现到实战

> 精选可直接复现的开源项目，按“视觉捕捉 → Retargeting → 仿真/真机部署”链路组织。每个项目标注**复现难度**、**核心学习点**和**与 Retargeting 的关联**。

---

## 目录

| 项目 | 方向 | 难度 | 关键标签 |
|------|------|------|---------|
| [AnyTeleop](#anyteleop) | 遥操作框架 | ⭐⭐⭐ | Vision-based teleop, Dexterous manipulation |
| [HaMeR](#hamer) | 手部 mesh 恢复 | ⭐⭐ | Hand mesh, MANO, RGB-only |
| [LEAP Hand](#leap-hand) | 低成本灵巧手 | ⭐⭐ | 16-DOF, URDF, MuJoCo |
| [DexPilot](#dexpilot) | 视觉遥操作 | ⭐⭐⭐⭐ | Vision-based, 无穿戴设备 |
| [RDT-1B](#rdt-1b) | 双臂扩散策略 | ⭐⭐⭐ | Diffusion Transformer, Bimanual |
| [DexGraspNet](#dexgraspnet) | 灵巧抓取数据集 | ⭐⭐ | Grasp generation, Shadow Hand |
| [OpenTeleVision](#opentelevision) | VR 遥操作 | ⭐⭐ | Meta Quest, Stereoscopic vision |
| [MyoSuite / MyoHand](#myosuite) | 肌骨仿真+RL | ⭐⭐⭐ | RL, Hand biomechanics |

---

## AnyTeleop

| 属性 | 信息 |
|------|------|
| **论文** | AnyTeleop: A General Vision-Based Dexterous Robot Arm-Hand Teleoperation System (RSS 2023) |
| **机构** | UC San Diego, NVIDIA |
| **链接** | [https://github.com/dexsuite/dex-teleop](https://github.com/dexsuite/dex-teleop) |
| **难度** | ⭐⭐⭐ |
| **硬件需求** | RGB 摄像头（单目/双目） |

### 为什么值得复现

AnyTeleop 是**无穿戴视觉遥操作**的代表性工作。它用单个 RGB 摄像头捕捉人手 21 点，通过 IK Retargeting 控制 Shadow Hand + Franka 机械臂。复现它能让你完整走通：

```
MediaPipe 21点 → 局部坐标系 → 手掌姿态估计 → 手指 IK → Shadow Hand 控制
```

### 核心学习点

1. **手掌姿态估计**：用手腕 + 3 个 MCP 点构建正交基，估计手掌 R/t
2. **手指链 IK**：每根手指独立做解析/数值 IK，而非直接角度映射
3. **臂-手协同**：手掌位置由机械臂 IK 控制，手指由灵巧手 IK 控制

### 复现步骤

```bash
# 1. 环境（Ubuntu 20.04+, Python 3.8）
git clone https://github.com/dexsuite/dex-teleop.git
cd dex-teleop && pip install -e .

# 2. 启动摄像头 + MediaPipe 检测
python run_webcam.py --hand_model shadow

# 3. 启动 Isaac Gym / MuJoCo 仿真
python run_sim.py --robot shadow --arm franka
```

### 与 Retargeting 的关系

- 直接使用了本项目 Stage 2/3 的**Rule-based + Vector Optimization**混合策略
- 拇指使用 Rule-based（防止 Vector Opt 局部最优），其余手指用 IK
- 手掌姿态估计代码可参考 `tutorials/05-complete-pipeline/README.md` 第 4 节

---

## HaMeR

| 属性 | 信息 |
|------|------|
| **论文** | Reconstructing Hands in 3D with Transformers (CVPR 2024) |
| **机构** | MPI for Intelligent Systems |
| **链接** | [https://github.com/geopavlakos/hamer](https://github.com/geopavlakos/hamer) |
| **难度** | ⭐⭐ |
| **硬件需求** | GPU (>= 8GB VRAM) |

### 为什么值得复现

HaMeR 解决了 MediaPipe 的痛点：**深度估计不准、自遮挡严重**。它从单张 RGB 直接回归 MANO 参数（778 个顶点 + 16 个关节角），输出的是**完整的 3D 手模型**，而非稀疏 21 点。

### 核心学习点

1. **MANO 参数化**：10 维 shape + 48 维 pose (轴角)，理解人手参数化模型
2. **从 mesh 到 landmarks**：MANO 顶点 → 21 点映射，可做更精确的 retargeting
3. **Transformer 编码器**：ViT 提取图像特征，直接回归参数

### 复现步骤

```bash
git clone https://github.com/geopavlakos/hamer.git
cd hamer && pip install -e .

# 下载预训练权重（~400MB）
mkdir _DATA && python fetch_demo_data.py

# 单图推理
python demo.py --img_folder example_data --out_folder output --batch_size 8
```

### 与 Retargeting 的关系

- 输出 MANO 关节角后，可用本项目 `examples/complete_retargeting_pipeline.py` 中的 `RuleBasedRetargeter` 直接映射到 O10
- 比 MediaPipe 更精确的 3D 位置 → 任务空间 IK 精度更高
- 可替换本项目 Stage 1 的 21 点检测模块

---

## LEAP Hand

| 属性 | 信息 |
|------|------|
| **论文** | LEAP Hand: Low-Cost, Efficient, and Anthropomorphic Hand for Robot Learning (CoRL 2023) |
| **机构** | Columbia University |
| **链接** | [https://github.com/leap-hand/LEAP_Hand_Sim](https://github.com/leap-hand/LEAP_Hand_Sim) |
| **难度** | ⭐⭐ |
| **硬件需求** | GPU for Isaac Gym sim |

### 为什么值得复现

LEAP Hand 是**低成本开源灵巧手**的标杆（整机 <$2000）。它的仿真环境基于 Isaac Gym，提供：
- URDF/MJCF 模型
- RL 训练代码（抓取、旋转等任务）
- 与真实 LEAP Hand 的通信接口

### 核心学习点

1. **16-DOF 灵巧手建模**：理解 Allegro/Shadow 之外的另一种 DOF 分配方案
2. **Isaac Gym 仿真**：大规模并行 RL 训练（可同时跑 4096 个环境）
3. **Sim-to-Real**：从仿真策略直接迁移到真实 LEAP Hand

### 复现步骤

```bash
git clone https://github.com/leap-hand/LEAP_Hand_Sim.git
cd LEAP_Hand_Sim && pip install -e .

# 训练抓取策略（Isaac Gym）
python train.py --task leap_hand_grasp --num_envs 2048

# 部署到真实 LEAP Hand（需硬件）
python deploy_real.py --checkpoint checkpoints/grasp.pt
```

### 与 Retargeting 的关系

- LEAP Hand 的 URDF 可直接用于本项目的 `VectorOptimizationRetargeter`（替换 O10 模型）
- 可作为 GeoRT 部署的目标平台（`assets/` 下放 LEAP Hand URDF，`config/` 写 joint_order JSON）

---

## DexPilot

| 属性 | 信息 |
|------|------|
| **论文** | DexPilot: Vision Based Teleoperation of Dexterous Robotic Hand-Arm System (ICRA 2020) |
| **机构** | UC Berkeley |
| **链接** | [https://github.com/byte-dance/dexpilot](https://github.com/byte-dance/dexpilot)（社区镜像） |
| **难度** | ⭐⭐⭐⭐ |
| **硬件需求** | RGB-D 摄像头（RealSense D435 推荐） |

### 为什么值得复现

DexPilot 是**早期 vision-based teleoperation 的经典工作**。它用 RGB-D 点云做手部跟踪，通过**优化问题**直接求解机器人关节角，无需显式的 21 点检测。

### 核心学习点

1. **点云配准**：人手的深度点云与机器人手模型点云对齐
2. **直接优化**：以机器人关节角为变量，最小化点云距离 + 关节正则化
3. **无穿戴**：不需要数据手套或动捕设备

### 复现步骤

```bash
git clone https://github.com/byte-dance/dexpilot.git
cd dexpilot && pip install -r requirements.txt

# 启动 RealSense 并运行跟踪
python run_dexpilot.py --camera realsense --robot shadow
```

### 与 Retargeting 的关系

- DexPilot 本质上是一个**端到端优化 retargeting**：
  ```
  目标函数 = w1 * ||人点云 - 机器人点云|| + w2 * ||关节正则化|| + w3 * 碰撞惩罚
  ```
- 与本项目 `docs/04-optimization-methods.md` 中 SLSQP 约束优化的思想一致
- 适合理解“为什么直接用优化比 Rule-based 更鲁棒”

---

## RDT-1B

| 属性 | 信息 |
|------|------|
| **论文** | RDT-1B: a Diffusion Foundation Model for Bimanual Manipulation (2024) |
| **机构** | 清华大学，上海 AI Lab |
| **链接** | [https://github.com/thu-ml/RDT-1B](https://github.com/thu-ml/RDT-1B) |
| **难度** | ⭐⭐⭐ |
| **硬件需求** | GPU (>= 24GB VRAM for 1B model) |

### 为什么值得复现

RDT-1B 是目前**最大的双臂机器人扩散模型**（1B 参数），支持视觉-语言-动作联合生成。它的 action space 包含双手关节角，retargeting 结果可直接作为训练数据输入。

### 核心学习点

1. **Diffusion Transformer (DiT)**：用扩散模型生成动作序列，而非直接回归
2. **Bimanual Action Space**：双臂 + 双手的联合动作表示
3. **Scaling Law**：1B 参数模型在小样本机器人任务上的涌现能力

### 复现步骤

```bash
git clone https://github.com/thu-ml/RDT-1B.git
cd RDT-1B && pip install -r requirements.txt

# 下载预训练权重（~4GB）
python scripts/download_weights.py --model rdt-1b

# 推理 demo（需提供初始图像和语言指令）
python inference.py --prompt "pick up the red cube with both hands" \
    --initial_image demo/start.png
```

### 与 Retargeting 的关系

- RDT 的输入动作可以是**retargeting 后的关节角**：人做一遍动作 → retargeting → 存入数据集 → 训练 RDT
- 本项目 `examples/complete_retargeting_pipeline.py` 的输出可直接作为 RDT 的 action chunk
- 理解 Diffusion Policy 对 retargeting 精度的要求（低精度动作会导致扩散模型收敛慢）

---

## DexGraspNet

| 属性 | 信息 |
|------|------|
| **论文** | DexGraspNet: A Large-Scale Robotic Dexterous Grasp Dataset for General Objects (NeurIPS 2023) |
| **机构** | PKU, BIGAI |
| **链接** | [https://github.com/PKU-EPIC/DexGraspNet](https://github.com/PKU-EPIC/DexGraspNet) |
| **难度** | ⭐⭐ |
| **硬件需求** | GPU for grasp generation |

### 为什么值得复现

DexGraspNet 提供了 **1.32 million 个 Shadow Hand 抓取姿态**，覆盖 5355 个物体。它是理解“灵巧手能怎么抓”的绝佳数据集。

### 核心学习点

1. **Grasp Generation**：从物体点云生成可行的 Shadow Hand 抓取姿态
2. **物理可解性筛选**：用 Isaac Gym 验证每个抓取是否稳定（不掉落）
3. **Dataset Structure**：理解大规模机器人数据集的组织方式

### 复现步骤

```bash
git clone https://github.com/PKU-EPIC/DexGraspNet.git
cd DexGraspNet && pip install -r requirements.txt

# 下载数据集（~50GB）
python scripts/download_dataset.py

# 可视化抓取姿态
python visualize.py --object mug --grasp_id 42

# 训练 grasp generator（可选）
python train.py --config configs/pointnet_grasp.yaml
```

### 与 Retargeting 的关系

- 可作为 Learning-based retargeting 的**先验数据**：先学 DexGraspNet 中的抓取姿态分布，再微调到人手上
- 理解 Shadow Hand 的关节限位和可达空间，有助于设置 retargeting 的 bounds
- `docs/02-retargeting-taxonomy.md` 中 Learning-based 方法的数据来源参考

---

## OpenTeleVision

| 属性 | 信息 |
|------|------|
| **论文** | Open-TeleVision: Teleoperation with Immersive Active Visual Feedback (2024) |
| **机构** | UC San Diego |
| **链接** | [https://github.com/OpenTeleVision/TeleVision](https://github.com/OpenTeleVision/TeleVision) |
| **难度** | ⭐⭐ |
| **硬件需求** | Meta Quest 3 / Apple Vision Pro |

### 为什么值得复现

OpenTeleVision 用**VR 头显做沉浸式遥操作**，操作者通过双目立体视觉感知机器人视角，用手柄控制机器人双臂。它展示了 retargeting 在**人机交互层面**的应用。

### 核心学习点

1. **Stereo Visual Feedback**：延迟 < 100ms 的双目视频流回传
2. **VR 手柄到机器人末端映射**：手柄位姿 → 机械臂末端位姿（6-DOF tracking）
3. **沉浸式操作**：操作者临场感（telepresence）对任务成功率的影响

### 复现步骤

```bash
git clone https://github.com/OpenTeleVision/TeleVision.git
cd TeleVision && pip install -r requirements.txt

# 启动 VR 服务端
python server/television_server.py --device quest3

# 启动机器人控制端（需配合真实/仿真机器人）
python robot_control/franka_control.py
```

### 与 Retargeting 的关系

- VR 手柄提供的是**末端位姿**（position + quaternion），需要用 IK 转换为关节角 → 与本项目 Stage 3 机械臂 IK 直接相关
- 双手操作时的**左右手镜像问题**与 Dexterous Retargeting 双手系统完全一致
- 可作为人机交互层面的 retargeting 应用场景参考

---

## MyoSuite / MyoHand

| 属性 | 信息 |
|------|------|
| **论文** | MyoSuite: A contact-rich simulation suite for musculoskeletal motor control (NeurIPS 2022) |
| **机构** | Meta AI |
| **链接** | [https://github.com/facebookresearch/myosuite](https://github.com/facebookresearch/myosuite) |
| **难度** | ⭐⭐⭐ |
| **硬件需求** | GPU 可选（MuJoCo CPU 即可） |

### 为什么值得复现

MyoSuite 是**肌骨级别的人手仿真环境**，包含 MyoHand（20 个肌腱驱动的手指关节）。它能让你理解“人手的运动学约束”是什么，从而设计更好的 retargeting 映射。

### 核心学习点

1. **Tendon-driven Actuation**：肌腱驱动 vs 电机直驱对关节控制的影响
2. **Contact-rich Tasks**：钥匙旋转、球抓取等需要精细接触的任务
3. **RL for Hand Control**：PPO/SAC 在精细操作任务上的表现

### 复现步骤

```bash
git clone https://github.com/facebookresearch/myosuite.git
cd myosuite && pip install -e .

# 运行 MyoHand 抓取任务
python -m myosuite.utils.examine_env --env_name myoHandGrabExp-v0

# 训练 RL 策略
python myosuite/agents/train_rl.py --env myoHandPenTwirl-v0 --algorithm PPO
```

### 与 Retargeting 的关系

- MyoHand 的关节结构更接近真实人手（20 DOF），可作为 retargeting 的**参考源模型**
- 理解人手运动的生物力学约束，有助于解释为什么某些 retargeting 映射会“不自然”
- `examples/complete_retargeting_pipeline.py` 中 `SyntheticHandGenerator` 的灵感来源

---

## 项目对比与选型建议

| 你的目标 | 推荐项目 | 原因 |
|---------|---------|------|
| **快速跑通视觉遥操作全流程** | AnyTeleop | 开箱即用，MediaPipe + Shadow Hand |
| **获得更精确的 3D 手模型** | HaMeR | 单 RGB → MANO mesh，替代 MediaPipe |
| **低成本真机实验** | LEAP Hand | <$2000，URDF/MuJoCo/Isaac Gym 齐全 |
| **理解优化式 retargeting** | DexPilot | 点云直接优化，无显式 landmark |
| **生成双臂操作训练数据** | RDT-1B | Diffusion 生成动作，retargeting 输出可作输入 |
| **研究灵巧抓取** | DexGraspNet | 百万级抓取数据，Shadow Hand 物理验证 |
| **沉浸式人机交互** | OpenTeleVision | VR 双目反馈，双臂协同控制 |
| **理解人手生物力学** | MyoSuite | 肌腱驱动，接触丰富任务 |

---

## 复现路线图建议

### 路径 A：快速上手（1-2 周）

```
Week 1: 本项目 Stage 1-3 + LEAP Hand Sim 跑通抓取
Week 2: AnyTeleop + HaMeR 替换 MediaPipe，对比精度
```

### 路径 B：深入研究（1 个月）

```
Week 1-2: DexPilot + 本项目 Vector Optimization 方法对比
Week 3: DexGraspNet 数据分析 → 设计 Learning-based retargeter
Week 4: RDT-1B 微调，用 retargeting 数据训练双臂策略
```

### 路径 C：真机部署（2 周 + 硬件）

```
准备：LEAP Hand / Shadow Hand + RealSense D435
Week 1: 本项目 complete pipeline → LEAP Hand URDF → MuJoCo 仿真
Week 2: GeoRT 部署（URDF → config JSON → ROS2 控制节点）
```

---

## 附录：Retargeting 相关工具库

| 库 | 用途 | 链接 |
|----|------|------|
| **scipy.optimize.least_squares** | 数值 IK / 向量优化 | [SciPy Docs](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.least_squares.html) |
| **PyTorch3D** | 可微分渲染 / 3D 运算 | [GitHub](https://github.com/facebookresearch/pytorch3d) |
| **trimesh** | 3D mesh 处理（碰撞检测） | [GitHub](https://github.com/mikedh/trimesh) |
| **pinocchio** | 刚体动力学 / 解析 Jacobian | [GitHub](https://github.com/stack-of-tasks/pinocchio) |
| **pybullet** | 物理仿真（比 MuJoCo 轻量） | [PyBullet Docs](https://docs.google.com/document/d/10sXEhzFRSnvFcl3XxNGhnD4N2SedqwdAvK3dsihxVUA) |
| **manopth** | MANO 模型 PyTorch 实现 | [GitHub](https://github.com/hassony2/manopth) |
