# Phase 1: 基础工具与环境

## 阶段学习目标

1. **Linux 命令行 & 系统设置：** 熟练使用常用命令进行文件系统操作，理解用户、权限 (`sudo`)、环境变量，掌握软件包管理 (`apt`)，**配置国内镜像源**，了解**串口设备权限 (`dialout`, `udev`规则)**，并能编写简单的 Shell 脚本。

2. **Git 版本控制：** 掌握 Git 的基本工作流，能使用 Git 进行个人项目管理和基础协作。

3. **C++ 核心：** 理解 C++ 基本语法、面向对象编程、STL 核心容器，能编写、编译、调试分离编译的 C++ 程序。

4. **CMake 构建系统：** 理解 CMake 作用，能编写 `CMakeLists.txt` 构建 C++ 项目。

---

## 具体任务与学习方法

### 任务 1.1: Linux 命令行基础

**要求：**
- 练习文件系统导航 (`cd`, `ls`, `pwd`)
- 文件/目录操作 (`mkdir`, `rm`, `cp`, `mv`)
- 文本编辑 (`nano` 或 VS Code)
- 环境变量查看和临时设置 (`echo $PATH`, `export`)

**学习方法：**
- 打开终端，逐个尝试命令，理解参数和输出
- 创建、移动、复制、删除文件和目录
- 编辑 `.bashrc` 添加永久环境变量（例如 `export MY_VAR="hello"`）
- 使用 `source ~/.bashrc` 后验证

**参考资料：**
---

### 任务 1.2: Linux 系统设置 (中国区)

**要求：**
学习并实践以下内容：
1. 更换 Ubuntu 的 `apt` 软件包源为国内镜像源（如清华、阿里、中科大等）
2. 理解 `sudo` 的作用和基本用法
3. 了解 Linux 如何管理串口设备 (如 `/dev/ttyUSB0`, `/dev/ttyACM0`)
4. 学习为何需要将用户添加到 `dialout` 组才能访问串口
5. **（可选）** 学习 `udev` 规则的基本概念及其在固定设备名称或设置权限中的作用

**学习方法：**

**1. 更换国内镜像源：**
- 搜索 "Ubuntu 更换国内镜像源" 并按照教程修改 `/etc/apt/sources.list` 文件
- **注意：修改前务必备份原文件！** 
  ```bash
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
  ```
- 修改后运行 `sudo apt update`

**2. 学习 sudo：**
- 学习 `sudo` 命令，尝试执行需要管理员权限的命令（如 `sudo apt update`）
- 理解 `/etc/sudoers` 文件的基本作用（**不要直接编辑此文件！**）

**3. 串口设备权限：**
- 学习 `groups` 命令查看当前用户所属的用户组
- 使用以下命令将当前用户添加到 `dialout` 组（需要重新登录生效）：
  ```bash
  sudo usermod -aG dialout $USER
  ```
- 连接一个 USB 转串口设备（如果手头有），使用 `ls /dev/tty*` 查看设备名

**4. （可选）udev 规则：**
- 阅读 `udev` 教程，了解规则文件（通常在 `/etc/udev/rules.d/`）的基本语法
- 特别关注 `SUBSYSTEM`, `ATTRS{idVendor}`, `ATTRS{idProduct}`, `MODE`, `GROUP`, `SYMLINK` 等关键字
- 查看 `sl_sensor` 提供的 `udev_rules` 目录下的示例规则文件

**参考资料：**
- Ubuntu 镜像源更换教程（搜索清华/阿里/中科大 Ubuntu 镜像站帮助文档）
- `sudo` 命令教程（搜索 "linux sudo tutorial"）
- Ubuntu 添加用户到用户组（搜索 "ubuntu add user to group dialout"）
- `udev` 教程（搜索 "linux udev tutorial" 或 "writing udev rules"）

---

### 任务 1.3: Linux Shell 脚本 (`setup_project.sh`)

**要求：**

- 编写一个 Shell 脚本，自动创建 `my_sl_project` 目录结构及空文件

**学习方法：**
- 学习 Shell 脚本基本语法（变量、条件、循环）
- 编写脚本，使用 `echo` 进行调试
- 运行脚本并检查结果

**参考资料：**
- Shell 脚本教程: https://www.runoob.com/linux/linux-shell.html

**提示：**
脚本应该能够创建如下结构：
```
my_sl_project/
├── src/
├── include/
├── build/
└── README.md
```

---

### 任务 1.4: Git 版本控制与 GitHub 协作

**要求：**
- 为 `my_sl_project` 创建 GitHub 仓库
- 完成初始化、提交、分支、推送等操作

**学习方法：**
- 阅读 Pro Git 前三章（**必读**）
- 在本地和 GitHub 上实践以下命令：
  - `git init`
  - `git add`
  - `git commit`
  - `git remote add`
  - `git push`
  - `git branch`
  - `git checkout`

**参考资料：**

- Pro Git (中文版) - **必读前三章**: https://git-scm.com/book/zh/v2
- GitHub 官方快速入门: https://docs.github.com/en/get-started/quickstart

**实践步骤：**
1. 在 GitHub 上创建新仓库 `my_sl_project`
2. 在本地初始化 Git 仓库
3. 创建至少 3 次有意义的提交
4. 创建并切换到一个新分支
5. 将代码推送到 GitHub

---

### 任务 1.5: C++ 基础编程 (`ProjectorControl` 类)

**要求：**

- 编写 `ProjectorControl` 类（分离为 `.h` 和 `.cpp` 文件）
- 包含 `displayPattern` 方法
- 在 `main` 函数中实例化并调用
- 使用 `g++` 命令行编译

**学习方法：**
- 通过 LearnCpp.com 学习：
  - C++ 类的定义和实现
  - 头文件/源文件分离
  - `std::vector` 的使用
- 使用 `g++` 命令行编译，理解 `-I` 和 `-o` 选项

**参考资料：**
- LearnCpp.com - **强烈推荐**: https://www.learncpp.com/
- C++ 标准库参考: https://en.cppreference.com/w/

**示例编译命令：**
```bash
g++ -c src/ProjectorControl.cpp -I include -o ProjectorControl.o
g++ -c src/main.cpp -I include -o main.o
g++ ProjectorControl.o main.o -o my_program
./my_program
```

---

### 任务 1.6: CMake 构建系统

**要求：**
- 为任务 1.5 的项目编写 `CMakeLists.txt`
- 使用 `cmake` 和 `make` 编译项目

**学习方法：**
- 阅读 CMake 官方教程 Step 1 & 2（**必做**）
- 实践 `cmake .. && make` 构建流程
- 理解 out-of-source build 的概念

**参考资料：**
- CMake Tutorial - **必做 Step 1 & 2**: https://cmake.org/cmake/help/latest/guide/tutorial/index.html

**实践步骤：**
```bash
mkdir build
cd build
cmake ..
make
./my_program
```

---

## 学习建议

1. **循序渐进：** 按照任务顺序学习，不要跳过基础内容
2. **动手实践：** 每个命令、每行代码都要亲自尝试
3. **记录日志：** 在 `LearningLog.md` 中记录学习过程和遇到的问题
4. **善用 AI：** 遇到问题时可以使用 AI 助手，但要理解其提供的解决方案
5. **查阅文档：** 培养查阅官方文档的习惯

---

## 预期时间

- 建议用 **2-3 周** 完成本阶段所有任务

---

## 验收标准

完成本阶段后，你应该能够：
- ✅ 熟练使用 Linux 命令行进行日常操作
- ✅ 独立配置 Linux 开发环境（镜像源、权限等）
- ✅ 使用 Git 管理个人项目
- ✅ 编写、编译和运行分离编译的 C++ 程序
- ✅ 使用 CMake 构建 C++ 项目

