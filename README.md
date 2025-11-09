# UGV ROS 2 示例工作区

## 项目简介
本仓库是一个标准的 ROS 2 工作区（`colcon` 工作流），目前包含官方 `ros2/examples` 中的 `rclcpp`、`rclpy` 与 `launch_testing` 示例，方便在本地快速验证通信链路、Launch 配置以及端到端测试流程。你可以把它当作 UGV（无人地面车辆）项目的脚手架：
- 直接复用或裁剪示例节点，加速原型开发。
- 参考 Launch、测试与包结构，保持工程一致性。
- 在同一工作区逐步补充自研感知、控制或仿真包。

## 目录结构
```
ugv_homework/
├── build/          # colcon 构建输出（忽略提交）
├── install/        # 安装空间（忽略提交）
├── log/            # 构建/运行日志（忽略提交）
└── src/
    └── examples/   # ROS 2 官方示例（rclcpp、rclpy、launch_testing）
```
根据需求，你可以继续在 `src/` 下新增自定义包，或将 `examples` 拆分为独立子模块。

## 环境依赖
- Ubuntu 24.04 + ROS 2 Jazzy Geochelone
- Python 3.12.3（Jazzy 默认）
- Gazebo Harmonic（`gz-sim` 系列）
- colcon、rosdep、vcstool 等基础工具

快速安装示例（以 Jazzy 为例）：
```bash
sudo apt update && sudo apt install \
  ros-jazzy-desktop \
  ros-jazzy-gazebo-ros-pkgs \
  python3-colcon-common-extensions \
  python3-rosdep \
  python3-vcstool \
  gz-harmonic
sudo rosdep init || true
rosdep update
```

## 快速开始
1. **初始化工作区**
   ```bash
   mkdir -p ~/ws && cd ~/ws
   # 将本仓库克隆到工作区根目录
   git clone git@github.com:<your-name>/ugv_homework.git src/ugv_homework
   cd src/ugv_homework
   rosdep install --from-paths src --ignore-src -r -y
   ```
2. **构建**
   ```bash
   cd ~/ws
   colcon build --symlink-install
   source install/setup.bash
   ```
3. **运行示例节点（示例）**
   ```bash
   # C++ Minimal Publisher / Subscriber
   ros2 run examples_rclcpp_minimal_publisher publisher_member_function
   ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function

   # Python Minimal Publisher / Subscriber
   ros2 run examples_rclpy_minimal_publisher publisher_member_function
   ros2 run examples_rclpy_minimal_subscriber subscriber_member_function

   # launch_testing 样例
   ros2 test src/examples/launch_testing/examples/py_launch_example.test.py
   ```

## 常见任务
- **代码风格**：C++ 参考 `ament_uncrustify`，Python 参考 `ament_flake8` / `ament_black`。
- **测试**：`colcon test --packages-select <pkg>` 然后 `colcon test-result --verbose`。
- **拓展包**：使用 `ros2 pkg create --build-type ament_cmake my_pkg` 或 `ament_python` 快速生成模板。

## 许可证与致谢
`src/examples/` 目录继承 ROS 2 官方示例的许可（Apache 2.0）。若在该基础上二次开发，请在新包内保留原有版权声明，并在根仓库补充适合你项目的 LICENSE。

欢迎把实际 UGV 功能包、仿真环境和 Launch 场景逐步搬进来，配合本 README 与 `.gitignore`，即可直接推送到 GitHub 进行协作。
