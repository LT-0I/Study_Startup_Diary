# 共享学习资源

本目录用于存放各位同学共享的学习资料、参考链接、文档等。

---

## 官方文档与教程

### Linux & Shell
- [Shell 脚本教程 - 菜鸟教程](https://www.runoob.com/linux/linux-shell.html)

### Git
- [Pro Git 中文版](https://git-scm.com/book/zh/v2) - **强烈推荐前三章**
- [GitHub 官方文档](https://docs.github.com/en/get-started/quickstart)

### C++
- [LearnCpp.com](https://www.learncpp.com/) - **系统化的 C++ 学习资源**
- [C++ 标准库参考](https://en.cppreference.com/w/)

### CMake
- [CMake Tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

### ROS
- [ROS Wiki Tutorials](http://wiki.ros.org/ROS/Tutorials) - Beginner Level
- [ROS TF2 Tutorials](http://wiki.ros.org/tf2/Tutorials)

### MAVROS & PX4
- [MAVROS Wiki](http://wiki.ros.org/mavros)
- [PX4 SITL Simulation](https://docs.px4.io/main/zh/simulation/)

### OpenCV
- [OpenCV Tutorials](https://docs.opencv.org/4.x/)
- [cv_bridge Tutorial](http://wiki.ros.org/cv_bridge/Tutorials/UsingCvBridgeToConvertBetweenROSImagesAndOpenCVImages)

### PCL
- [PCL Documentation](https://pcl.readthedocs.io/en/latest/)
- [pcl_conversions](http://wiki.ros.org/pcl_conversions)

### YAML-cpp
- [yaml-cpp Tutorial](https://github.com/jbeder/yaml-cpp/wiki/Tutorial)

---

## 镜像源配置（中国区）

### Ubuntu APT 镜像源
- [清华大学 Ubuntu 镜像](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
- [阿里云 Ubuntu 镜像](https://developer.aliyun.com/mirror/ubuntu)
- [中科大 Ubuntu 镜像](https://mirrors.ustc.edu.cn/help/ubuntu.html)

### ROS 镜像源
- [清华大学 ROS 镜像](https://mirrors.tuna.tsinghua.edu.cn/help/ros/)

---

## 视频教程（可选）

### ROS 系列
- Bilibili 搜索："ROS机器人开发实践"
- Bilibili 搜索："古月居 ROS 入门21讲"
- 搜索：[Fishros](https://fishros.org.cn/forum/topic/20/%E5%B0%8F%E9%B1%BC%E7%9A%84%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E7%B3%BB%E5%88%97?lang=en-US)一键安装ROS

### C++ 系列
- Bilibili 搜索："C++ 程序设计（北大）"

---

## 常用工具

### 调试工具
- **GDB** - C++ 调试器
  - [GDB Tutorial](https://www.gdbtutorial.com/)
- **rqt_console** - ROS 日志查看工具
  - [rqt_console Wiki](http://wiki.ros.org/rqt_console)
- **RViz** - ROS 3D 可视化工具
  - [RViz Wiki](http://wiki.ros.org/rviz)

### 命令行工具
```bash
# ROS 常用命令
rosnode list              # 查看所有节点
rostopic list             # 查看所有话题
rostopic echo /topic      # 查看话题内容
rostopic hz /topic        # 查看话题频率
rostopic bw /topic        # 查看话题带宽
rosservice list           # 查看所有服务
rosservice call /service  # 调用服务
rqt_graph                 # 可视化节点图

# Git 常用命令
git status                # 查看状态
git add .                 # 添加所有修改
git commit -m "message"   # 提交
git push                  # 推送
git pull                  # 拉取
git log --oneline         # 查看提交历史

# Linux 常用命令
htop                      # 查看系统资源
df -h                     # 查看磁盘使用
du -sh *                  # 查看当前目录各文件大小
find . -name "*.cpp"      # 查找文件
grep -r "keyword" .       # 搜索内容
```

---

## 问题排查指南

### 常见问题

**1. ROS 包找不到**
```bash
# 确保已经 source 工作空间
source ~/catkin_ws/devel/setup.bash
# 可以将上述命令添加到 ~/.bashrc 中
```

**2. MAVROS 连接不上 PX4**
- 检查 PX4 SITL 是否正在运行
- 检查 MAVROS 的 `fcu_url` 参数是否正确
- 使用 `rostopic echo /mavros/state` 查看连接状态

**3. 编译错误**
- 确保所有依赖都已安装：`rosdep install --from-paths src --ignore-src -r -y`
- 清除编译缓存：`catkin_make clean` 或 `rm -rf build devel`
- 检查 CMakeLists.txt 和 package.xml 是否正确

**4. Git 推送失败**
- 先拉取远程代码：`git pull`
- 解决冲突后再推送：`git push`

---

## 学习资料（待添加）

*各位同学可以在这里添加找到的优秀学习资料、博客文章、GitHub 项目等*

### 推荐博客
- 

### 推荐 GitHub 项目
- 

### 推荐论文
- 

---

## 问题讨论区

*遇到共同的问题可以在这里记录和讨论，每次评论要加时间*

### 问题 1：
- 

---

*最后更新时间：2025-10-21*

