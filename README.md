# Startup Study Diary - ROS 与无人机学习项目

## 项目目标 (Project Goal)

本项目旨在理解和复现 `sl_sensor` 项目，并掌握基础的 ROS 无人机控制技能。通过系统化的学习路径，实现：

- 掌握 Linux 基础、Git 版本控制、C++、CMake 等基础技能
- 理解 ROS 核心概念并能进行 ROS 开发
- 掌握 MAVROS (MAVLink↔ROS) 和无人机仿真
- 熟悉 OpenCV、PCL 等核心库的使用
- **最终目标：** 理解和复现 `sl_sensor` ROS 项目（软件部分），并掌握使用 ROS/MAVROS 控制开源无人机飞控（如 PX4/ArduPilot）进行基本飞行的技能，为未来将 `sl_sensor` 集成到无人机上打下基础。

## 学习计划概览 (Learning Plan Overview)

本项目分为五个渐进式学习阶段：

### 阶段 1: 基础工具与环境
- Linux 命令行与系统设置（含镜像源配置、串口权限）
- Git 版本控制与 GitHub 协作
- C++ 核心编程
- CMake 构建系统

### 阶段 2: ROS 核心与无人机控制基础
- ROS 核心概念（节点、话题、服务、消息、参数、TF）
- MAVROS 基础与配置
- PX4 SITL + Gazebo 仿真环境
- 基础飞控交互与 Offboard 编程

### 阶段 3: 核心依赖库
- OpenCV 图像处理与 `cv_bridge`
- PCL 点云处理
- YAML-cpp 配置文件读取

### 阶段 4: 理解 `sl_sensor` 与集成思路
- `sl_sensor` 项目结构分析
- 核心流程与参数配置
- 与无人机集成方案设计

### 阶段 5: 复现与仿真集成
- 模拟数据测试
- 调试工具使用（GDB、rqt_console）
- 仿真环境中的传感器数据融合与可视化

## 仓库结构 (Repository Structure)

```
Startup_Study_Diary/
├── .gitignore          # ROS、C++、Python 等忽略文件配置
├── README.md           # 本文件 - 项目总体说明
├── Assignments/        # 各阶段任务要求
│   ├── Phase1_Basics.md
│   ├── Phase2_ROS_MAVROS.md
│   ├── Phase3_Libraries.md
│   ├── Phase4_Codebase_Integration.md
│   └── Phase5_Replication_Simulation.md
├── Peach/           # Peach的工作区
│   ├── README.md
│   ├── LearningLog.md  # 学习日志（使用提供的模板）
│   ├── Phase1/Code/
│   ├── Phase2/Code/
│   ├── Phase3/Code/
│   ├── Phase4/Code/
│   └── Phase5/Code/
├── Sheep/           # Sheep 的工作区（结构同 StudentA）
│   ├── README.md
│   ├── LearningLog.md
│   ├── Phase1/Code/
│   ├── Phase2/Code/
│   ├── Phase3/Code/
│   ├── Phase4/Code/
│   └── Phase5/Code/
└── Resources/          # 共享学习资料、链接等
```

## 仓库使用指南 (Repository Usage)

### 如何查看任务
- 进入 `Assignments/` 目录，查看对应阶段的任务文件（如 `Phase1_Basics.md`）
- 每个阶段文件都包含详细的学习目标、具体任务、学习方法和参考资料

### 如何提交代码和日志
1. **代码提交：**
   - 在各自目录下的对应阶段 `Code/` 文件夹中完成任务代码
   - 例如：Peach 完成 Phase1 任务 1.3，代码应放在 `Peach/Phase1/Code/` 目录下

2. **日志更新：**
   - 在各自目录下的 `LearningLog.md` 中记录学习过程
   - **要求每周至少更新一次**，使用提供的模板格式

3. **Commit Message 规范：**
   - 格式：`[Peach][PhaseY] 简短描述`
   - 示例：
     - `[Peach][Phase1] Complete Linux command practice`
     - `[Sheep][Phase2] Add simple_offboard_control node`
     - `[Peach][Log] Week 3 learning summary`

### Git 工作流程
```bash
# 1. 拉取最新代码
git pull origin main

# 2. 添加修改的文件
git add .

# 3. 提交修改（遵循 Commit Message 规范）
git commit -m "[Peach/Sheep][Phase1] Complete task 1.3"

# 4. 推送到远程仓库
git push origin main
```

## 学习日志规范 (Learning Log Guidelines)

各位同学需要在自己目录下的 `LearningLog.md` 中记录学习过程，要求：

### 更新频率
- **每周至少更新一次**（没学啥就写没学啥，没关系的，从0到1往往是困难的）
- 重要进展或问题可随时更新

### 必须包含的内容
1. **日期/周次** (Date/Week)
2. **学习内容** (Learning Content)
3. **关键收获** (Key Takeaways)
4. **实践与代码** (Practice & Code) - 包含 Git Commit 链接
5. **遇到的问题与解决方案** (Problems Encountered & Solutions)
6. **AI 助手使用情况** (AI Assistant Usage) - **重点记录**
   - 使用的工具 (Tool Used)
   - 解决的问题 (Problem Addressed)
   - 关键提示词 (Key Prompt)
   - 效果与反思 (Effectiveness & Reflection)
7. **心得与下一步计划** (Reflections & Next Steps)

### AI 助手使用说明
**鼓励合理使用 AI 编程助手**（如 Cursor、GitHub Copilot、ChatGPT 等），但要求：

- **必须在学习日志中记录每次使用情况**
- 反思 AI 工具的帮助效果和局限性
- 理解 AI 生成的代码，而不是盲目复制
- 培养独立思考和问题解决能力

## 开始学习 (Getting Started)

1. **Clone 本仓库：**
   
   ```bash
   git clone <repository-url>
   cd Startup_Study_Diary
   ```
   
2. **阅读第一阶段任务：**
   
   ```bash
   cat Assignments/Phase1_Basics.md
   ```
   
3. **创建第一个日志条目：**
   - 打开你的 `LearningLog.md` 文件
   - 使用提供的模板记录第一周的学习内容

4. **开始编码并提交：**
   
   - 完成任务代码
   - 遵循 Commit Message 规范提交
   - 更新学习日志

## 联系与协作

- 各位同学之间要互相学习和讨论
- 遇到问题时，优先自己查阅网络教程，文档和资料，**外网优先！！！**
- 善用 AI 助手，但要理解其提供的解决方案
- 保持学习日志的及时更新

---

**祝学习顺利！ Happy Learning! 🚀**


20251012 Tiger@NUAA
