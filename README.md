# ros2-study

ROS 2 (Humble, Ubuntu 22.04) 학습용 colcon 워크스페이스입니다. turtlesim을 대상으로 토픽, 서비스, 액션, 파라미터, 런치 파일, 멀티스레드 실행기를 실습했습니다.

## 구성

```
src/
├── my_first_package/          Python 패키지 (ament_python)
│   ├── my_first_package/      노드 소스
│   ├── launch/                런치 파일
│   └── params/turtlesim.yaml  파라미터 파일
└── my_first_package_msgs/     사용자 정의 인터페이스 (ament_cmake)
    ├── msg/CmdAndPoseVel.msg
    ├── srv/MultiSpawn.srv
    └── action/DistTurtle.action
bags/                          ros2 bag 녹화 예제 (turtle_test*)
```

## 노드

| 실행 이름 | 내용 |
|---|---|
| `my_first_node` | 기본 노드 구조 |
| `my_publisher` | `/turtlesim/turtle1/cmd_vel` 퍼블리셔 |
| `my_subscriber` | `/turtle1/pose` 서브스크라이버 |
| `turtle_cmd_and_pose` | `cmd_vel`과 `pose`를 구독해 `CmdAndPoseVel` 메시지로 `/cmd_and_pose`에 발행 |
| `my_service_server` | `multi_spawn` 서비스(`MultiSpawn`): 요청한 수만큼 거북이를 원형으로 배치해 생성 |
| `dist_turtle_action_server` | `dist_turtle` 액션(`DistTurtle`): 지정 거리만큼 이동하며 남은 거리를 피드백, 파라미터(`quantile_time`, `almost_goal_time`) 사용 |
| `my_multi_thread` | `MultiThreadedExecutor` 예제 |

## 빌드와 실행

```bash
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash

# turtlesim + 퍼블리셔
ros2 launch my_first_package turtlesim_and_teleop.launch.py

# 액션 서버
ros2 launch my_first_package dist_turtle_action.launch.py
ros2 action send_goal /dist_turtle my_first_package_msgs/action/DistTurtle "{linear_x: 2.0, angular_z: 1.0, dist: 2.0}" --feedback
```

녹화된 bag 재생:

```bash
ros2 bag play bags/turtle_test
```
