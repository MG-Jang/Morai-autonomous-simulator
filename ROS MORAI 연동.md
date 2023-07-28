# ROS 시뮬레이터(MORAI) 연동

ROS 와 시뮬레이터를 연동시 ROS Bridge 라는 개념을 사용한다. 

- ROS 개발 환경 구축

```py
$ cd ~/catkin_ws/src

# simulator ROS message
$ git clone https://github.com/morai-developergroup/morai_msgs.git

#git clone을 사용 할 수 없는 경우
$ sudo apt-get install git

```

## 시뮬레이터와 ROS연동

<br>

ROS는 원래 리눅스 환경에서 작동된다. 하지만 우리의 노트북은 윈도우 환경이기 때문에 VM을 이용해서 리눅스 가상환경을 만들어 준 후 사용한다.

<img src="https://github.com/MG-Jang/img/blob/main/ros-simulator.JPG?raw=tru"  width="500" height="300">

- 모든 터미널 실행후 아래 
```py
# catkin 환경 변수 선언
$ source ~/catkin_ws/devel/setup.bash
#패키지 재구축 
$ rospack profile
```

```py
# Rosbridge 실행 
$ roslaunch rosbridge_server rosbridge_websocket.launch
# 현재 나는 bashrc에 위 내용을 쉘로 추가하여 아래 명령어만 쳐도 동일한 효과
# 만약 bashrc 내용을 알고 싶다면 ROS정리를 확인 해 볼것.
$ rrb
```

- MORAI ROS 네트워크 설정

<img src="https://github.com/MG-Jang/img/blob/main/ros-morai-network-setting.JPG?raw=true"  width="500" height="300">

```py
# 우분투 터미널에서 입력
$ ifconfig
```


```py
# 충돌데이터 입력
$ rostopic echo /CollisionData
```