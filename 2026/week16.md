# 周报
## 一、当前任务
1）项目：基于具身空间的架空输电线路运检关键技术及装备研究

## 二、本周工作
1）问题：数据集缺少销钉以及多样性不足

解决：采集销钉数据集，标注时注意尽量保证框的中心在销钉中心附近，便于后续抓取时的位置调整；

采集远视角下销孔数据集。

2）问题：yolo定位+抓取精度不高
解决：经统计，终点误差在一定的范围内，且数值比较恒定，可引入自定义偏置参数，在获取相机坐标并根据手眼标定矩阵转换到基座标后引入偏执，补正误差，补正后实际效果较好。

3）问题：初版代码较乱，测试代码操作复杂
解决：重构代码目录结构，修改各种脚本的引用导出关系，将常用SDK封装为Nova5功能包，归纳算法模型、资源文件，测试/已完成脚本整理到scripts中。

```
Prj-167-WS/
├── datasets/                         # 标注数据集
│   └── yolo11s/                      #   images/ + labels/（401 对，类别: kong / luo）
├── models/                           # 模型训练 & 推理流水线
│   ├── yolo_sam_pipeline/            #   YOLO + SAM 采集/标注/训练
│   │   ├── config.yaml               #     全局配置
│   │   ├── capture.py                #     RGB-D 图像采集
│   │   ├── annotate.py               #     SAM 辅助标注
│   │   └── train.py                  #     YOLO 训练
│   └── sam2/                         #   Meta SAM2 子模块
├── Nova5/                            # 机械臂 SDK 功能包（Python 包）
│   ├── __init__.py                   #   导出 Nova5Robot, Nova5DobotArm, ...
│   ├── robot.py                      #   Nova5Robot 高层封装
│   ├── nova5_arm.py                  #   Nova5DobotArm（V3 协议）
│   ├── dobot_arm.py                  #   CR5DobotArm 基类
│   └── dobot_api.py                  #   Dobot TCP/IP 原始协议
├── source/                           # 感知模块 & 手眼标定
│   ├── perception/                   #   YOLO 检测 / SAM2 分割 / 3D 几何
│   ├── eye_hand_calibration/         #   手眼标定（Nova5 + Gemini 215/305）
│   ├── handeye_result.json           #   标定结果 T_cam_to_gripper
│   └── CameraParam_*.ini             #   相机内参
├── scripts/                          # 部署 & 调试脚本
│   ├── detect_grasp_nova5.py         #   主线：实时检测 + 抓取
│   ├── nova5_robot.py                #   向后兼容 shim
│   ├── nova5_teach.py                #   使能 + 拖拽示教
│   ├── nova5_enable.py               #   强制使能（应急恢复）
│   └── test/                         #   测试脚本
│       ├── test_gripper.py           #     夹爪测试
│       ├── robot_tools.py            #     位姿/拖拽/HOME/相机
│       ├── nova5_inspire_gripper_control_test.py  # 因时夹爪 Modbus 测试
│       └── gamepad_dobot_6d_gripper_collector_*.py # 手柄遥操作采集
├── weights/                          # 模型权重
│   ├── yolo11s/train400/best.pt      #   YOLO 检测（kong / luo）
│   └── sam2/checkpoints/             #   SAM2 分割
├── runs/                             # 训练输出
├── docs/                             # 文档（夹爪手册等）
├── FRAMEWORK.md                      # 详细框架说明
└── READEME.md
```

4）问题：因Nova5 版本较老，使用SDK v4时，夹爪控制相关类与方法不可用
解决：根据V3版本的SDK，重新封装了一版夹爪控制类，测试结果显示可正常控制夹爪开闭
5）目前实现：机械臂在Home位置->检测到销钉->抓取销钉->返回Home位置->检测销孔位置->移动到小孔前方等待插入

三、下周任务
设计销钉插入任务