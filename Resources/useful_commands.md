# 常用命令速查表

本文档整理了学习过程中常用的命令，方便快速查阅。

---

## Linux 基础命令

### 文件系统操作
```bash
# 导航
cd /path/to/directory    # 切换目录
cd ~                     # 回到家目录
cd -                     # 回到上一次所在目录
pwd                      # 显示当前目录

# 查看文件
ls                       # 列出文件
ls -la                   # 列出所有文件（包括隐藏文件）的详细信息
tree                     # 树状显示目录结构

# 文件/目录操作
mkdir dirname            # 创建目录
mkdir -p dir1/dir2/dir3  # 递归创建目录
rm filename              # 删除文件
rm -r dirname            # 递归删除目录
rm -rf dirname           # 强制递归删除（危险！）
cp source dest           # 复制文件
cp -r source dest        # 递归复制目录
mv source dest           # 移动/重命名文件或目录

# 文件内容查看
cat filename             # 显示文件内容
less filename            # 分页查看文件
head -n 10 filename      # 查看前 10 行
tail -n 10 filename      # 查看后 10 行
tail -f filename         # 实时查看文件更新
```

### 文本处理
```bash
# 搜索
grep "pattern" filename           # 在文件中搜索
grep -r "pattern" directory       # 递归搜索目录
grep -i "pattern" filename        # 不区分大小写搜索

# 查找文件
find . -name "*.cpp"              # 按名称查找
find . -type f -mtime -7          # 查找最近 7 天修改的文件

# 文本编辑
nano filename                     # 使用 nano 编辑器
vim filename                      # 使用 vim 编辑器
code filename                     # 使用 VS Code 编辑器
```

### 系统管理
```bash
# 权限
sudo command                      # 以管理员权限运行
chmod +x filename                 # 添加可执行权限
chmod 755 filename                # 设置权限为 rwxr-xr-x
chown user:group filename         # 更改文件所有者

# 用户和组
whoami                            # 显示当前用户
groups                            # 显示当前用户所属组
sudo usermod -aG groupname user   # 将用户添加到组

# 软件包管理
sudo apt update                   # 更新软件包列表
sudo apt upgrade                  # 升级所有软件包
sudo apt install package          # 安装软件包
sudo apt remove package           # 删除软件包

# 进程管理
ps aux                            # 查看所有进程
ps aux | grep process_name        # 查找特定进程
kill PID                          # 终止进程
killall process_name              # 终止所有同名进程
htop                              # 交互式进程查看器
```

### 环境变量
```bash
# 查看
echo $PATH                        # 查看 PATH 变量
env                               # 查看所有环境变量

# 设置（临时）
export VAR_NAME="value"           # 设置环境变量

# 设置（永久）
# 编辑 ~/.bashrc 文件，添加：
# export VAR_NAME="value"
source ~/.bashrc                  # 重新加载配置
```

---

## Git 命令

### 基础操作
```bash
# 初始化
git init                          # 初始化仓库
git clone <url>                   # 克隆远程仓库

# 状态和历史
git status                        # 查看状态
git log                           # 查看提交历史
git log --oneline                 # 简洁查看历史
git log --graph --all             # 图形化查看分支历史

# 添加和提交
git add file                      # 添加文件到暂存区
git add .                         # 添加所有修改
git commit -m "message"           # 提交
git commit --amend                # 修改最后一次提交
```

### 分支操作
```bash
# 查看分支
git branch                        # 查看本地分支
git branch -a                     # 查看所有分支（包括远程）

# 创建和切换
git branch branch_name            # 创建分支
git checkout branch_name          # 切换分支
git checkout -b branch_name       # 创建并切换到新分支
git switch branch_name            # 切换分支（新语法）
git switch -c branch_name         # 创建并切换（新语法）

# 合并和删除
git merge branch_name             # 合并分支
git branch -d branch_name         # 删除已合并的分支
git branch -D branch_name         # 强制删除分支
```

### 远程仓库
```bash
# 远程仓库管理
git remote -v                     # 查看远程仓库
git remote add origin <url>       # 添加远程仓库

# 推送和拉取
git push origin main              # 推送到远程
git push -u origin main           # 推送并设置上游分支
git pull                          # 拉取并合并
git fetch                         # 仅拉取不合并
```

### 撤销和回退
```bash
# 撤销修改
git checkout -- file              # 撤销工作区修改
git restore file                  # 撤销工作区修改（新语法）
git reset HEAD file               # 取消暂存
git restore --staged file         # 取消暂存（新语法）

# 回退版本
git reset --soft HEAD~1           # 回退一次提交，保留修改
git reset --hard HEAD~1           # 回退一次提交，丢弃修改
git revert commit_id              # 创建新提交来撤销某次提交
```

---

## ROS 命令

### 核心命令
```bash
# 启动
roscore                           # 启动 ROS Master

# 运行节点
rosrun package_name node_name     # 运行节点
roslaunch package_name file.launch # 运行 launch 文件
```

### 节点管理
```bash
rosnode list                      # 列出所有节点
rosnode info /node_name           # 查看节点信息
rosnode kill /node_name           # 终止节点
```

### 话题（Topic）
```bash
rostopic list                     # 列出所有话题
rostopic echo /topic_name         # 查看话题内容
rostopic hz /topic_name           # 查看话题发布频率
rostopic bw /topic_name           # 查看话题带宽
rostopic info /topic_name         # 查看话题信息
rostopic pub /topic_name type data # 发布话题
```

### 服务（Service）
```bash
rosservice list                   # 列出所有服务
rosservice info /service_name     # 查看服务信息
rosservice call /service_name args # 调用服务
rosservice type /service_name     # 查看服务类型
```

### 参数（Parameter）
```bash
rosparam list                     # 列出所有参数
rosparam get /param_name          # 获取参数值
rosparam set /param_name value    # 设置参数值
rosparam dump file.yaml           # 导出参数到文件
rosparam load file.yaml           # 从文件加载参数
```

### 消息和包
```bash
rosmsg show message_type          # 查看消息定义
rosmsg list                       # 列出所有消息类型
rossrv show service_type          # 查看服务定义

rospack find package_name         # 查找包路径
rospack list                      # 列出所有包
roscd package_name                # 切换到包目录
```

### TF 坐标变换
```bash
rosrun tf tf_echo frame1 frame2   # 查看两个坐标系间的变换
rosrun tf view_frames             # 生成 TF 树 PDF
rosrun rqt_tf_tree rqt_tf_tree    # 可视化 TF 树
```

### 可视化和调试
```bash
rqt_graph                         # 节点图可视化
rqt_console                       # 日志查看器
rqt_image_view                    # 图像查看器
rviz                              # 3D 可视化工具
```

### rosbag（数据记录）
```bash
rosbag record -a                  # 记录所有话题
rosbag record /topic1 /topic2     # 记录指定话题
rosbag play bagfile.bag           # 回放数据
rosbag info bagfile.bag           # 查看 bag 文件信息
```

---

## CMake & Catkin

### Catkin（ROS 构建系统）
```bash
# 创建工作空间
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make

# 创建包
cd ~/catkin_ws/src
catkin_create_pkg package_name dependencies

# 编译
cd ~/catkin_ws
catkin_make                       # 编译所有包
catkin_make --pkg package_name    # 编译指定包

# 清理
catkin_make clean                 # 清理编译文件

# Source 工作空间
source ~/catkin_ws/devel/setup.bash
```

### CMake
```bash
# Out-of-source build
mkdir build
cd build
cmake ..
make
make install                      # 安装

# 指定编译类型
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake -DCMAKE_BUILD_TYPE=Release ..
```

---

## MAVROS 相关

### 常用话题
```bash
# 状态
rostopic echo /mavros/state

# 位置
rostopic echo /mavros/local_position/pose
rostopic echo /mavros/global_position/global

# 姿态
rostopic echo /mavros/imu/data
```

### 常用服务
```bash
# 解锁/上锁
rosservice call /mavros/cmd/arming "value: true"
rosservice call /mavros/cmd/arming "value: false"

# 模式切换
rosservice call /mavros/set_mode "custom_mode: 'OFFBOARD'"
rosservice call /mavros/set_mode "custom_mode: 'STABILIZED'"

# 起飞/降落
rosservice call /mavros/cmd/takeoff "{latitude: 0, longitude: 0, altitude: 2}"
rosservice call /mavros/cmd/land "{latitude: 0, longitude: 0, altitude: 0}"
```

---

## GDB 调试

```bash
# 启动调试
gdb ./program                     # 调试程序
gdb --args ./program arg1 arg2    # 带参数调试

# 在 GDB 中
(gdb) break main                  # 在 main 函数设置断点
(gdb) break file.cpp:42           # 在文件第 42 行设置断点
(gdb) run                         # 运行程序
(gdb) continue                    # 继续执行
(gdb) next                        # 单步执行（不进入函数）
(gdb) step                        # 单步执行（进入函数）
(gdb) print variable              # 打印变量值
(gdb) backtrace                   # 查看调用栈
(gdb) info breakpoints            # 查看所有断点
(gdb) delete 1                    # 删除断点 1
(gdb) quit                        # 退出

# ROS 节点调试
rosrun --prefix 'gdb -ex run --args' package node
```

---

## 其他常用工具

### 压缩和解压
```bash
# tar
tar -czf archive.tar.gz files/    # 压缩
tar -xzf archive.tar.gz           # 解压

# zip
zip -r archive.zip files/         # 压缩
unzip archive.zip                 # 解压
```

### 网络
```bash
ping host                         # 测试连通性
ifconfig                          # 查看网络接口
ip addr                           # 查看 IP 地址
netstat -tulpn                    # 查看端口占用
```

### 系统信息
```bash
uname -a                          # 查看系统信息
lsb_release -a                    # 查看 Ubuntu 版本
df -h                             # 查看磁盘使用
du -sh directory                  # 查看目录大小
free -h                           # 查看内存使用
```

---

*持续更新中...*

