# Phase 2: ROS 核心与无人机控制基础

## 阶段学习目标

1. **ROS 核心：** 理解 ROS 架构，掌握节点、话题、服务、消息、参数、TF 编程，熟练使用 ROS 命令行工具和 RViz。

2. **MAVROS：** 理解 MAVLink 和 MAVROS 的作用，掌握其基本配置和使用方法。

3. **无人机仿真：** 能够启动并运行 PX4 SITL + Gazebo 仿真环境。

4. **基础飞控交互：** 能够使用 ROS 服务和话题监控无人机状态，并发送基本指令。

5. **Offboard 编程：** 掌握使用 C++ 编写简单的 MAVROS Offboard 控制节点的基本流程。

---

## 具体任务与学习方法

### 任务 2.1: ROS Topics & Messages

**要求：**
- 创建 `simple_communication` ROS 包
- 实现 `status_publisher` 节点，发布自定义消息 `HardwareStatus.msg`
- 实现 `status_subscriber` 节点，接收并打印消息

**学习方法：**
- **严格按照** ROS Wiki C++ Tutorials 学习：
  - 创建工作空间（catkin workspace）
  - 创建 ROS 包
  - 定义自定义消息
  - 编写发布者（Publisher）节点
  - 编写订阅者（Subscriber）节点
- 使用 `catkin_make` 或 `catkin build` 编译
- 使用 `rosrun` 运行节点
- 使用 `rostopic` 工具检查话题

**参考资料：**
- ROS Wiki Tutorials (C++): http://wiki.ros.org/ROS/Tutorials (Beginner Level)

**实践步骤：**
```bash
# 1. 创建工作空间
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make

# 2. 创建包
cd ~/catkin_ws/src
catkin_create_pkg simple_communication std_msgs rospy roscpp

# 3. 定义消息、编写代码、编译
# 4. 运行
roscore
rosrun simple_communication status_publisher
rosrun simple_communication status_subscriber

# 5. 查看话题
rostopic list
rostopic echo /hardware_status
```

---

### 任务 2.2: ROS Services

**要求：**
- 在 `simple_communication` 包中添加 `SetLedStatus.srv` 服务定义
- 实现服务端（Server）节点
- 实现客户端（Client）节点

**学习方法：**
- 继续学习 ROS Tutorials 中服务（Services）的部分
- 实践服务定义、服务端/客户端 C++ 编程
- 学习 `rosservice` 命令行工具

**参考资料：**
- ROS Wiki Tutorials (C++): http://wiki.ros.org/ROS/Tutorials (WritingServiceServerClient)

**实践步骤：**
```bash
# 运行服务端
rosrun simple_communication led_service_server

# 使用命令行调用服务
rosservice call /set_led_status "led_id: 1, status: true"

# 运行客户端
rosrun simple_communication led_service_client
```

---

### 任务 2.3: TF 坐标变换

**要求：**
- 创建 `tf_basics` ROS 包
- 发布静态 TF 变换（Static Transform）
- 发布动态 TF 变换（Dynamic Transform）
- 在 RViz 中可视化 TF 树

**学习方法：**
- 学习 TF2 Tutorials，理解 TF 树的概念
- 实践 `StaticTransformBroadcaster` 编程
- 实践 `TransformBroadcaster` 编程
- 学习在 RViz 中配置 TF 可视化
- 学习使用 `tf_echo` 命令查看变换

**参考资料：**
- ROS Wiki TF2 Tutorials: http://wiki.ros.org/tf2/Tutorials
  - Introduction to tf2
  - Writing a static broadcaster
  - Writing a broadcaster

**实践步骤：**
```bash
# 运行 TF 广播节点
rosrun tf_basics static_tf_broadcaster
rosrun tf_basics dynamic_tf_broadcaster

# 查看 TF 树
rosrun rqt_tf_tree rqt_tf_tree

# 查看特定变换
rosrun tf tf_echo /world /camera_link

# 在 RViz 中可视化
rosrun rviz rviz
# 在 RViz 中添加 TF Display
```

---

### 任务 2.4: MAVROS 与仿真入门

**要求：**
1. 安装并运行 PX4 SITL + Gazebo 仿真环境
2. 编写节点监控无人机状态和位置
3. 使用 `rosservice call` 控制仿真无人机（模式切换、解锁、起飞、降落）

**学习方法：**
1. 按照 PX4 文档安装 SITL 和 Gazebo
2. 学习使用 `roslaunch mavros px4.launch` 启动 MAVROS
3. 使用 ROS 命令行工具探索 MAVROS 接口：
   - `rostopic list` - 查看所有话题
   - `rostopic echo /mavros/state` - 查看飞控状态
   - `rostopic echo /mavros/local_position/pose` - 查看位置
   - `rosservice list` - 查看所有服务
4. 编写 C++ 节点订阅 `/mavros/state` 和 `/mavros/local_position/pose`
5. 使用 `rosservice call` 练习：
   - 模式切换（OFFBOARD、STABILIZED 等）
   - 解锁（Arming）
   - 起飞（Takeoff）
   - 降落（Landing）

**参考资料：**
- PX4 SITL with Gazebo: https://docs.px4.io/main/en/simulation/gazebo.html
- MAVROS Wiki: http://wiki.ros.org/mavros
- PX4 MAVROS Offboard Tutorial: https://docs.px4.io/main/en/ros/mavros_offboard.html

**安装 PX4 SITL：**
```bash
# Clone PX4 Autopilot
cd ~
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
cd PX4-Autopilot
bash ./Tools/setup/ubuntu.sh

# 编译并运行 SITL
make px4_sitl gazebo
```

**启动仿真：**
```bash
# 终端 1: 启动 PX4 SITL + Gazebo
cd ~/PX4-Autopilot
make px4_sitl gazebo

# 终端 2: 启动 MAVROS
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"

# 终端 3: 查看状态
rostopic echo /mavros/state

# 终端 4: 测试服务调用
# 解锁
rosservice call /mavros/cmd/arming "value: true"
# 切换到 OFFBOARD 模式
rosservice call /mavros/set_mode "custom_mode: 'OFFBOARD'"
```

---

### 任务 2.5: MAVROS Offboard 编程基础

**要求：**
- 编写 `simple_offboard_control` 节点
- 在仿真中实现：
  1. 起飞到高度 1 米
  2. 飞到位置 (0, 0, 1)
  3. 保持悬停

**学习方法：**
1. 基于 PX4 Offboard 教程的 C++ 示例进行修改
2. 理解以下关键概念：
   - **状态机：** Offboard 控制流程（发送设定点 → 切换模式 → 解锁 → 起飞）
   - **模式切换：** 使用 `set_mode` 服务
   - **解锁：** 使用 `arming` 服务
   - **设定点发布：** 向 `/mavros/setpoint_position/local` 话题发布目标位置
3. 在仿真中调试代码，观察无人机行为

**参考资料：**
- PX4 MAVROS Offboard Tutorial (C++ Example) - **核心参考**: https://docs.px4.io/main/en/ros/mavros_offboard.html
- `mavros_msgs` 文档: http://docs.ros.org/en/noetic/api/mavros_msgs/html/index-msg.html

**关键代码框架：**
```cpp
#include <ros/ros.h>
#include <geometry_msgs/PoseStamped.h>
#include <mavros_msgs/CommandBool.h>
#include <mavros_msgs/SetMode.h>
#include <mavros_msgs/State.h>

mavros_msgs::State current_state;
void state_cb(const mavros_msgs::State::ConstPtr& msg){
    current_state = *msg;
}

int main(int argc, char **argv)
{
    ros::init(argc, argv, "offb_node");
    ros::NodeHandle nh;

    ros::Subscriber state_sub = nh.subscribe<mavros_msgs::State>
            ("mavros/state", 10, state_cb);
    ros::Publisher local_pos_pub = nh.advertise<geometry_msgs::PoseStamped>
            ("mavros/setpoint_position/local", 10);
    ros::ServiceClient arming_client = nh.serviceClient<mavros_msgs::CommandBool>
            ("mavros/cmd/arming");
    ros::ServiceClient set_mode_client = nh.serviceClient<mavros_msgs::SetMode>
            ("mavros/set_mode");

    // 设置发布频率
    ros::Rate rate(20.0);

    // 等待 FCU 连接
    while(ros::ok() && !current_state.connected){
        ros::spinOnce();
        rate.sleep();
    }

    // 设置目标位置
    geometry_msgs::PoseStamped pose;
    pose.pose.position.x = 0;
    pose.pose.position.y = 0;
    pose.pose.position.z = 1;

    // 在切换到 OFFBOARD 模式之前，必须已经发送了一些设定点
    for(int i = 100; ros::ok() && i > 0; --i){
        local_pos_pub.publish(pose);
        ros::spinOnce();
        rate.sleep();
    }

    // 切换到 OFFBOARD 模式并解锁
    mavros_msgs::SetMode offb_set_mode;
    offb_set_mode.request.custom_mode = "OFFBOARD";

    mavros_msgs::CommandBool arm_cmd;
    arm_cmd.request.value = true;

    ros::Time last_request = ros::Time::now();

    while(ros::ok()){
        if( current_state.mode != "OFFBOARD" &&
            (ros::Time::now() - last_request > ros::Duration(5.0))){
            if( set_mode_client.call(offb_set_mode) &&
                offb_set_mode.response.mode_sent){
                ROS_INFO("Offboard enabled");
            }
            last_request = ros::Time::now();
        } else {
            if( !current_state.armed &&
                (ros::Time::now() - last_request > ros::Duration(5.0))){
                if( arming_client.call(arm_cmd) &&
                    arm_cmd.response.success){
                    ROS_INFO("Vehicle armed");
                }
                last_request = ros::Time::now();
            }
        }

        local_pos_pub.publish(pose);

        ros::spinOnce();
        rate.sleep();
    }

    return 0;
}
```

---

## 学习建议

1. **ROS 基础必须扎实：** 不要急于进入 MAVROS，先把 ROS 核心概念掌握好
2. **多用命令行工具：** `rostopic`, `rosservice`, `rosnode`, `rqt_graph` 等工具非常有用
3. **理解数据流：** 使用 `rqt_graph` 可视化节点和话题的关系
4. **仿真是最好的学习环境：** 在仿真中可以安全地测试各种指令
5. **阅读官方文档：** PX4 和 MAVROS 的官方文档是最权威的学习资料

---

## 预期时间

- 建议用 **3-4 周** 完成本阶段所有任务
- ROS 基础（任务 2.1-2.3）：2 周
- MAVROS 与仿真（任务 2.4-2.5）：1-2 周

---

## 验收标准

完成本阶段后，你应该能够：
- ✅ 独立创建 ROS 包并编写节点
- ✅ 熟练使用 ROS 话题、服务、参数
- ✅ 理解并使用 TF 坐标变换
- ✅ 启动并运行 PX4 SITL + Gazebo 仿真
- ✅ 使用 MAVROS 监控和控制仿真无人机
- ✅ 编写简单的 Offboard 控制节点

