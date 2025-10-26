# Phase 3: 核心依赖库

## 学习目标

掌握 `sl_sensor` 项目所依赖的核心库的基本使用：
1. **cv_bridge：** 实现 ROS 图像消息与 OpenCV 图像的转换
2. **OpenCV：** 基本图像处理操作
3. **PCL：** 点云数据的滤波和处理
4. **yaml-cpp：** 配置文件的读取和解析

---

## 具体任务与学习方法

### 任务 3.1: OpenCV & `cv_bridge`

**要求：**
- 创建 `image_processing_node` ROS 节点
- 订阅图像话题（`sensor_msgs/Image`）
- 使用 OpenCV 在图像上画圆
- 发布处理后的图像

**学习方法：**
1. 学习 `cv_bridge` 教程，掌握 ROS 图像消息与 OpenCV 图像的相互转换
2. 查阅 OpenCV 文档，学习 `cv::circle` 函数的使用
3. 使用 `usb_cam` 节点或 rosbag 提供图像源
4. 使用 `rqt_image_view` 查看处理结果

**参考资料：**
- cv_bridge Tutorial: http://wiki.ros.org/cv_bridge/Tutorials/UsingCvBridgeToConvertBetweenROSImagesAndOpenCVImages
- OpenCV Tutorials: https://docs.opencv.org/4.x/

**关键代码框架：**
```cpp
#include <ros/ros.h>
#include <image_transport/image_transport.h>
#include <cv_bridge/cv_bridge.h>
#include <sensor_msgs/image_encodings.h>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/highgui/highgui.hpp>

class ImageProcessor
{
private:
    ros::NodeHandle nh_;
    image_transport::ImageTransport it_;
    image_transport::Subscriber image_sub_;
    image_transport::Publisher image_pub_;

public:
    ImageProcessor() : it_(nh_)
    {
        image_sub_ = it_.subscribe("/camera/image_raw", 1, 
            &ImageProcessor::imageCb, this);
        image_pub_ = it_.advertise("/image_processed", 1);
    }

    void imageCb(const sensor_msgs::ImageConstPtr& msg)
    {
        cv_bridge::CvImagePtr cv_ptr;
        try
        {
            // ROS Image -> OpenCV Image
            cv_ptr = cv_bridge::toCvCopy(msg, sensor_msgs::image_encodings::BGR8);
        }
        catch (cv_bridge::Exception& e)
        {
            ROS_ERROR("cv_bridge exception: %s", e.what());
            return;
        }

        // 在图像中心画圆
        cv::Point center(cv_ptr->image.cols/2, cv_ptr->image.rows/2);
        cv::circle(cv_ptr->image, center, 50, CV_RGB(255,0,0), 3);

        // OpenCV Image -> ROS Image 并发布
        image_pub_.publish(cv_ptr->toImageMsg());
    }
};

int main(int argc, char** argv)
{
    ros::init(argc, argv, "image_processor");
    ImageProcessor ic;
    ros::spin();
    return 0;
}
```

**实践步骤：**
```bash
# 安装 usb_cam（如果需要）
sudo apt-get install ros-noetic-usb-cam

# 启动摄像头节点（或使用 rosbag）
rosrun usb_cam usb_cam_node

# 运行图像处理节点
rosrun image_processing image_processing_node

# 查看结果
rosrun rqt_image_view rqt_image_view
# 选择 /image_processed 话题
```

---

### 任务 3.2: PCL (Point Cloud Library)

**要求：**
- 创建 `pcl_filter_node` ROS 节点
- 订阅点云话题（`sensor_msgs/PointCloud2`）
- 使用 PCL 进行点云滤波（如统计滤波器）
- 发布滤波后的点云

**学习方法：**
1. 学习 `pcl_conversions` 库，掌握 ROS 点云消息与 PCL 点云的转换
2. 阅读 PCL Filtering 教程，理解 `StatisticalOutlierRemoval` 的原理和参数
3. 使用 `pcd_to_pointcloud` 节点发布点云，或使用 rosbag
4. 在 RViz 中对比滤波前后的效果

**参考资料：**
- PCL Tutorials: https://pcl.readthedocs.io/en/latest/ (Filtering section)
- pcl_conversions: http://wiki.ros.org/pcl_conversions
- PCL Filtering Tutorial: https://pcl.readthedocs.io/projects/tutorials/en/latest/statistical_outlier.html

**关键代码框架：**
```cpp
#include <ros/ros.h>
#include <sensor_msgs/PointCloud2.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/point_cloud.h>
#include <pcl/point_types.h>
#include <pcl/filters/statistical_outlier_removal.h>

class PCLFilter
{
private:
    ros::NodeHandle nh_;
    ros::Subscriber cloud_sub_;
    ros::Publisher cloud_pub_;

public:
    PCLFilter()
    {
        cloud_sub_ = nh_.subscribe("/input_cloud", 1, &PCLFilter::cloudCb, this);
        cloud_pub_ = nh_.advertise<sensor_msgs::PointCloud2>("/filtered_cloud", 1);
    }

    void cloudCb(const sensor_msgs::PointCloud2ConstPtr& input)
    {
        // ROS PointCloud2 -> PCL PointCloud
        pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
        pcl::fromROSMsg(*input, *cloud);

        // 统计滤波
        pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_filtered(new pcl::PointCloud<pcl::PointXYZ>);
        pcl::StatisticalOutlierRemoval<pcl::PointXYZ> sor;
        sor.setInputCloud(cloud);
        sor.setMeanK(50);           // 设置邻近点数量
        sor.setStddevMulThresh(1.0); // 设置标准差倍数阈值
        sor.filter(*cloud_filtered);

        // PCL PointCloud -> ROS PointCloud2 并发布
        sensor_msgs::PointCloud2 output;
        pcl::toROSMsg(*cloud_filtered, output);
        output.header = input->header;
        cloud_pub_.publish(output);
    }
};

int main(int argc, char** argv)
{
    ros::init(argc, argv, "pcl_filter_node");
    PCLFilter pf;
    ros::spin();
    return 0;
}
```

**实践步骤：**
```bash
# 下载测试点云数据或使用 rosbag
# 运行滤波节点
rosrun pcl_processing pcl_filter_node

# 在 RViz 中可视化
rosrun rviz rviz
# 添加两个 PointCloud2 Display
# 一个订阅 /input_cloud，一个订阅 /filtered_cloud
# 对比滤波效果
```

---

### 任务 3.3: YAML-cpp

**要求：**
- 编写一个**非 ROS** 的 C++ 程序
- 读取 `settings.yaml` 配置文件
- 解析并打印配置内容

**学习方法：**
1. 阅读 `yaml-cpp` 文档和教程
2. 学习如何加载 YAML 文件
3. 学习如何访问节点（`config["key"]`）
4. 学习如何进行类型转换（`.as<type>()`）
5. 学习如何迭代列表和映射
6. 确保 CMake 正确链接 `yaml-cpp` 库

**参考资料：**
- yaml-cpp Tutorial: https://github.com/jbeder/yaml-cpp/wiki/Tutorial
- yaml-cpp GitHub: https://github.com/jbeder/yaml-cpp

**示例配置文件 `settings.yaml`：**
```yaml
camera:
  width: 640
  height: 480
  fps: 30

sensor:
  name: "sl_sensor"
  enabled: true
  parameters:
    - threshold: 0.5
    - max_distance: 10.0

calibration:
  matrix: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
```

**示例代码：**
```cpp
#include <iostream>
#include <yaml-cpp/yaml.h>

int main(int argc, char** argv)
{
    try {
        // 加载 YAML 文件
        YAML::Node config = YAML::LoadFile("settings.yaml");

        // 访问简单值
        int width = config["camera"]["width"].as<int>();
        int height = config["camera"]["height"].as<int>();
        std::cout << "Camera resolution: " << width << "x" << height << std::endl;

        // 访问布尔值
        bool enabled = config["sensor"]["enabled"].as<bool>();
        std::cout << "Sensor enabled: " << (enabled ? "yes" : "no") << std::endl;

        // 访问字符串
        std::string sensor_name = config["sensor"]["name"].as<std::string>();
        std::cout << "Sensor name: " << sensor_name << std::endl;

        // 迭代列表
        std::cout << "Sensor parameters:" << std::endl;
        if (config["sensor"]["parameters"]) {
            for (const auto& param : config["sensor"]["parameters"]) {
                for (auto it = param.begin(); it != param.end(); ++it) {
                    std::cout << "  " << it->first.as<std::string>() 
                              << ": " << it->second.as<double>() << std::endl;
                }
            }
        }

        // 访问数组
        std::vector<double> matrix = config["calibration"]["matrix"].as<std::vector<double>>();
        std::cout << "Calibration matrix: ";
        for (double val : matrix) {
            std::cout << val << " ";
        }
        std::cout << std::endl;

    } catch (const YAML::Exception& e) {
        std::cerr << "YAML Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

**CMakeLists.txt：**
```cmake
cmake_minimum_required(VERSION 3.0)
project(yaml_reader)

set(CMAKE_CXX_STANDARD 11)

# 查找 yaml-cpp 库
find_package(yaml-cpp REQUIRED)

add_executable(yaml_reader src/main.cpp)
target_link_libraries(yaml_reader ${YAML_CPP_LIBRARIES})
```

**安装 yaml-cpp：**
```bash
sudo apt-get install libyaml-cpp-dev
```

**编译运行：**
```bash
mkdir build && cd build
cmake ..
make
./yaml_reader
```

---

## 学习建议

1. **理解库的作用：** 每个库都解决特定问题，理解其应用场景
2. **查阅官方文档：** 遇到问题时，优先查阅官方文档和示例
3. **从简单示例开始：** 先运行官方示例，再修改成自己的需求
4. **结合 ROS 使用：** 前两个任务需要结合 ROS，第三个任务是纯 C++
5. **可视化验证：** 使用 RViz 和 `rqt_image_view` 验证处理结果

---

## 预期时间

- 建议用 **1-2 周** 完成本阶段所有任务
- 每个任务大约 2-3 天

---

## 验收标准

完成本阶段后，你应该能够：
- ✅ 在 ROS 节点中处理图像数据（使用 OpenCV 和 cv_bridge）
- ✅ 在 ROS 节点中处理点云数据（使用 PCL 和 pcl_conversions）
- ✅ 读取和解析 YAML 配置文件（使用 yaml-cpp）
- ✅ 理解这些库在 `sl_sensor` 项目中的作用

