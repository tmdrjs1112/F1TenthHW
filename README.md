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

---
