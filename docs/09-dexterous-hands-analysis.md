# 开源灵巧手对比分析

> 面向具身 AI / 灵巧操作研究的灵巧机器人手全面对比，包含开源模型下载链接、DOF 结构、价格和代表性论文。

## 总览对比

| 手名称 | DOF | 开源硬件 | URDF | MJCF | 仿真支持 | 价格 (USD) | 许可证 |
|--------|-----|---------|------|------|---------|-----------|--------|
| **LEAP Hand** | 16 | **是** | 有 | 有 | Isaac Gym/Lab, MuJoCo | $200-$2,000 | MIT (代码) / CC BY-NC-SA (CAD) |
| **ORCA Hand** | 17 | **是** | 有 | 有 | Isaac Sim | $800-$1,000 | MIT |
| **RAPID Hand** | 待确认 | **是** | 待确认 | 待确认 | 待确认 | 低成本 | — |
| **Allegro Hand** | 16 | 否 | 有 | 无 | Gazebo, Isaac | ~$19,000 | Apache 2.0 (ROS) |
| **Shadow Hand** | 24 | 否 | 有 | 有 | MuJoCo, Gazebo, robomimic | ~$100,000+ | Apache 2.0 (ROS) |
| **DEX-EE** | 12 | 否 | 有 | 有 | ROS, MuJoCo | 未公开 | — |
| **Robotiq 3F** | 4 | 否 | 有 | 无 | Gazebo | $3,000-$5,000 | BSD (ROS) |
| **Schunk SVH** | 9 | 否 | 有 | 无 | Gazebo | $50,000-$70,000 | Apache 2.0 (ROS) |
| **AR10** | 10 | 否 | 有 | 无 | MoveIt | $5,000-$8,000 | BSD |

---

## 1. LEAP Hand（CMU / RSS 2023）

**最成熟的完全开源灵巧手方案，社区活跃，仿真支持最完善。**

| 属性 | 信息 |
|------|------|
| **机构** | Columbia University |
| **论文** | LEAP Hand: Low-Cost, Efficient, and Anthropomorphic Hand for Robot Learning (RSS 2023) |
| **DOF** | 16（4 指 x 4 自由度：拇指 + 食指 + 中指 + 无名指） |
| **驱动方式** | 直驱电机 |
| **GitHub** | [https://github.com/leap-hand](https://github.com/leap-hand) |
| **仿真仓库** | [LEAP_Hand_Sim](https://github.com/leap-hand/LEAP_Hand_Sim)（Isaac Gym + URDF） |
| **Isaac Lab** | [LEAP_Hand_Isaac_Lab](https://github.com/leap-hand/LEAP_Hand_Isaac_Lab) |
| **API 仓库** | [LEAP_Hand_API](https://github.com/leap-hand/LEAP_Hand_API)（ROS/ROS2/Python/C++） |
| **URDF 路径** | `LEAP_Hand_Sim/assets/` |
| **MJCF** | MuJoCo Playground 提供 |
| **价格** | V1 ~$2,000（BOM）；V2 ~$200-$300（简化版） |
| **许可证** | MIT（代码），CC BY-NC-SA（CAD 文件） |

**关键特性**：
- 完全开源硬件（3D CAD、STL、组装指南），4 小时可组装
- V2 版本（2025）含腱驱动版本，成本降至 $200-$300
- 创新的通用外展/内收机构
- 配套遥操作：BiDex 搭配 Manus 手套

**代表性论文**：
- "Dexterous Functional Grasping" (DexFunc)
- "VideoDex: Learning Dexterity from Internet Videos"
- "Robotic Telekinesis"（从无标定人视频学习）

**模型下载**：
```bash
# 仿真环境 + URDF
git clone https://github.com/leap-hand/LEAP_Hand_Sim.git
cd LEAP_Hand_Sim && ls assets/  # URDF 文件

# Isaac Lab 版本
git clone https://github.com/leap-hand/LEAP_Hand_Isaac_Lab.git

# 控制接口
git clone https://github.com/leap-hand/LEAP_Hand_API.git
```

---

## 2. ORCA Hand（ETH Zurich）

**完全开源，同时提供 URDF + MJCF，硬件成本极低。**

| 属性 | 信息 |
|------|------|
| **机构** | ETH Zurich |
| **DOF** | 17（腱驱动，5 指） |
| **驱动方式** | 腱驱动 |
| **核心仓库** | [orca_core](https://github.com/orcahand/orca_core)（554 stars） |
| **模型文件** | [orcahand_description](https://github.com/orcahand/orcahand_description)（MJCF + URDF，382 stars） |
| **硬件文件** | [orcahand_hardware](https://github.com/orcahand/orcahand_hardware)（STL/3MF，128 stars） |
| **仿真仓库** | [orca_sim](https://github.com/orcahand/orca_sim)（55 stars） |
| **URDF/MJCF** | `orcahand_description/` 下直接提供 |
| **价格** | ~$800-$1,000（BOM 成本），淘宝约 829 RMB |
| **许可证** | MIT |

**模型下载**：
```bash
git clone https://github.com/orcahand/orcahand_description.git
ls orcahand_description/  # URDF + MJCF + 配置文件
```

---

## 3. Shadow Hand（Shadow Robot Company）

**最经典的仿人灵巧手，24-DOF，大量顶会论文使用，但硬件昂贵。**

| 属性 | 信息 |
|------|------|
| **机构** | Shadow Robot Company (UK) |
| **DOF** | 24（20 个电机驱动 + 4 个欠驱动，5 指） |
| **驱动方式** | 腱驱动 |
| **ROS 仓库** | [shadow-robot](https://github.com/shadow-robot)（URDF + Gazebo） |
| **URDF** | `sr_common/sr_description/` |
| **MuJoCo 模型** | [MuJoCo Menagerie](https://github.com/google-deepmind/mujoco_menagerie/tree/master/shadow_hand) |
| **价格** | ~$100,000+ |
| **许可证** | 代码 Apache 2.0，硬件商业闭源 |

**关键特性**：
- 129 个传感器（位置、力、触觉）
- 力矩控制环 5kHz，位置控制环 1kHz
- OpenAI Dactyl（解魔方）使用的就是 Shadow Hand

**模型下载**（已在 `pretrained/urdf/mujoco_menagerie/shadow_hand/`）：
```bash
# 已下载到本项目
ls pretrained/urdf/mujoco_menagerie/shadow_hand/
```

**DEX-EE（2024 新版）**：
- 12-DOF（3 指），与 Google DeepMind 合作
- 指尖光学触觉传感器（数百个 taxel）
- 仓库：[dx_system](https://github.com/shadow-robot/dx_system)、[dx_common](https://github.com/shadow-robot/dx_common)

---

## 4. Allegro Hand（Wonik Robotics / 韩国）

**商业产品中性价比最高的灵巧手，ROS 生态完善。**

| 属性 | 信息 |
|------|------|
| **DOF** | 16（4 指 x 4 自由度，独立电流控制） |
| **驱动方式** | 直驱电机 |
| **ROS V4 仓库** | [allegro_hand_ros_v4](https://github.com/simlabrobotics/allegro_hand_ros_v4) |
| **URDF 路径** | `allegro_hand_ros_v4/src/allegro_hand_description/`（xacro 格式） |
| **MuJoCo 模型** | [MuJoCo Menagerie](https://github.com/google-deepmind/mujoco_menagerie/tree/master/allegro_hand) |
| **价格** | ~$19,000（V4），V5 ~$20,000+（新增指尖触觉） |
| **许可证** | ROS 驱动 Apache 2.0 |

**模型下载**（已在 `pretrained/urdf/mujoco_menagerie/allegro_hand/`）：
```bash
ls pretrained/urdf/mujoco_menagerie/allegro_hand/
```

---

## 5. Robotiq 3-Finger Gripper

**自适应欠驱动夹爪，更适合工业抓取而非灵巧操作。**

| 属性 | 信息 |
|------|------|
| **DOF** | 4 个驱动关节（3 指，自适应欠驱动） |
| **ROS 仓库** | [ros-industrial/robotiq](https://github.com/ros-industrial/robotiq) |
| **URDF** | `robotiq/robotiq_3f_gripper_visualization/urdf/` |
| **价格** | $3,000-$5,000 |
| **说明** | 偏自适应夹爪，不适合精细灵巧操作研究 |

---

## 6. Schunk SVH

**工业级五指灵巧手，可靠性高但价格昂贵。**

| 属性 | 信息 |
|------|------|
| **DOF** | 9（5 指，9 个独立驱动关节） |
| **ROS 驱动** | [ipa320/schunk_svh_driver](https://github.com/ipa320/schunk_svh_driver) |
| **通信** | EtherCAT |
| **价格** | $50,000-$70,000+ |

---

## 7. RAPID Hand（中山大学 / NeurIPS 2025）

**最新低成本灵巧手，面向通用机器人自主性。**

| 属性 | 信息 |
|------|------|
| **论文** | arXiv:2506.07490 |
| **GitHub** | [SYSU-RoboticsLab/RAPID-Hand](https://github.com/SYSU-RoboticsLab/RAPID-Hand) |
| **特性** | 低成本、感知集成、灵巧操作平台 |

---

## 与 Retargeting 的适配性分析

| 手名称 | DOF | Retargeting 难度 | 适合本项目的方法 | 说明 |
|--------|-----|-----------------|----------------|------|
| **LEAP Hand** | 16 | 低 | Rule-based + Vector Opt | 每指 4 DOF（含外展），比 O10 多 1 个自由度/指 |
| **ORCA Hand** | 17 | 中 | Vector Opt（腱驱动非线性） | 腱驱动有非线性映射，IK 需考虑腱耦合 |
| **Allegro Hand** | 16 | 低 | Rule-based + Vector Opt | 直驱，与 O10 类似结构 |
| **Shadow Hand** | 24 | 高 | Vector Opt | 自由度多但结构复杂，拇指多 1 DOF |
| **O10** | 10 | 最低 | Rule-based + Vector Opt | 本项目默认目标，最简单 |
| **Robotiq 3F** | 4 | 不适用 | — | 不适合灵巧 retargeting |