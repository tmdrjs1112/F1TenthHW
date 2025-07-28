# F1TENTH ROS2 시뮬레이션 환경 구축 및 제어

## 프로젝트 개요
본 프로젝트는 Ubuntu 20.04와 ROS2 Foxy를 기반으로 F1TENTH 자동차 시뮬레이터를 구축하고,  
RViz를 활용해 맵과 차량을 시각화하며, 키보드 조작을 통해 차량을 수동 제어하는 환경을 완성하는 것을 목표로 합니다.

## 주요 내용
- F1TENTH Gym ROS2 통신 브릿지 설치 및 실행  
- nav2_map_server를 활용한 레이싱 맵 로딩 및 퍼블리시  
- TF 좌표 변환 문제 해결을 위한 static_transform_publisher 적용  
- teleop_twist_keyboard 노드를 통한 키보드 텔레오퍼레이션 구현  
- cmdvel_to_ackermann 노드를 이용한 Twist 메시지 Ackermann 메시지 변환  
- RViz에서 차량 및 맵 동시 시각화 구현

## 설치 및 실행 방법

### 1. 환경 세팅

```bash
# ROS2 Foxy 설치 (Ubuntu 20.04 기준)
# F1TENTH Gym 패키지 클론 및 설치
git clone https://github.com/f1tenth/f1tenth_gym_ros
cd ~/f1tenth_ws
rosdep install --from-path src --ignore-src -r -y
colcon build
source install/setup.bash
```


### 2. 시뮬레이터 및 관련 노드 실행
```
# 터미널 1: 시뮬레이터 실행
ros2 launch f1tenth_gym_ros gym_bridge_launch.py

# 터미널 2: 맵 서버 실행
ros2 run nav2_map_server map_server --ros-args -p yaml_filename:=$HOME/f1tenth_ws/src/f1tenth_gym_ros/maps/levine.yaml

# 터미널 3: map_server 상태 변경
ros2 lifecycle set /map_server configure
ros2 lifecycle set /map_server activate

# 터미널 4: TF 고정 변환 브로드캐스트
ros2 run tf2_ros static_transform_publisher 0 0 0 0 0 0 map ego_racecar/base_link

# 터미널 5: 키보드 조작 노드 실행
ros2 run teleop_twist_keyboard teleop_twist_keyboard

# 터미널 6: cmdvel_to_ackermann 노드 실행
ros2 run cmdvel_to_ackermann cmdvel_to_ackermann
```

## 문제 해결 및 참고사항

    setup.py entry_points 문제 해결로 cmdvel_to_ackermann 빌드 오류 해결

    ackermann_msgs 패키지 미설치 문제 해결

    map_server lifecycle transition 실패 문제는 map_server 터미널 로그 확인 필수

    TF 변환 문제 해결을 위해 static_transform_publisher 사용

    RViz Fixed Frame을 map 또는 ego_racecar/base_link로 조절하며 확인

    RobotModel Status Error는 URDF 파라미터 설정 확인 필요

## 향후 계획

    자율 주행 알고리즘 추가 개발 및 센서 데이터 활용

    조이스틱 연동 및 SLAM 연동 시도

    시뮬레이터 및 RViz 커스텀 설정 강화

## 참고 문헌 및 라이선스

    O’Kelly et al., "F1TENTH: An Open-source Evaluation Environment for Continuous Control and Reinforcement Learning," NeurIPS 2019

    본 프로젝트는 MIT 라이선스를 따릅니다.
