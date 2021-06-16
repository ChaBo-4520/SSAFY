# Google Cartographer

![https://cdn2.hubspot.net/hubfs/5054152/demo_2d.gif](https://cdn2.hubspot.net/hubfs/5054152/demo_2d.gif)

Cartographer는 여러 플랫폼 및 센서를 통해 2D 및 3D로 실시간 SLAM을 제공하는 시스템이다. 2016년 10월 구글 오픈소스 프로젝트의 일환으로써 발표되었다. 기본적으로 C++ library로 제공되지만, ROS 시스템에도 사용할 수 있는 [cartographer_ros repository](https://github.com/cartographer-project/cartographer_ros)도 포함되어 있다.

> [참고]
>
> - **SLAM?** Simultaneous Localization And Mapping의 약자로써, 한국어로 번역해보면 ’동시적 위치 추정과 지도 작성’이라는 뜻이다. 즉, 임의 공간을 이동하면서 주변을 탐색할 수 있는 로봇에서 얻은 센서 데이터(라이다, 카메라, IMU 등)로 그 공간의 지도 및 현재 위치를 추정하는 것이다.
> - **Lidar?** Light Detection and Ranging의 약자로써 레이저 펄스를 쏘고 반사되어 돌아오는 시간을 측정하여 반사체의 위치좌표를 측정하는 레이다 시스템이다. 스피드 건, 자율이동로봇, 자율주행 자동차 등에 활용된다.
> - **IMU?** Inertial Measurement Unit의 약자로써 관성 측정 장치. 자이로스코프 / 가속도계 / 지자기센서로 구성되어있으며, 항공기, 인공위성, 우주선 등에도 쓰인다.

<br>

## System Requirements

- 64-bit, modern CPU (e.g. 3rd generation i7)
- 16 GB RAM
- Ubuntu 16.04 (Xenial), 18.04 (Bionic), 20.04 (Focal)
- gcc version 4.8.4, 5.4.0, 7.5.0, 9.3.0
- ROS Kinetic, Melodic (if ROS)

<br>

## Building & Installation

wstool, resdep, ninja를 이용함으로써 설치를 빠르게 할 수 있다.

```bash
$ sudo apt-get update$ sudo apt-get install -y python-wstool python-rosdep ninja-build
```

workspace인 'catkin_ws'라는 폴더를 만들고 코드를 받는다.

```bash
$ mkdir catkin_ws$ cd catkin_ws$ wstool init src$ wstool merge -t src https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall$ wstool update -t src
```

cartographer_ros의 dependencies인 proto3과 deb 패키지를 설치한다.

```bash
$ src/cartographer/scripts/install_proto3.sh$ sudo rosdep init$ rosdep update$ rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
```

빌드 후 설치한다.

```bash
$ catkin_make_isolated --install --use-ninja
```

<br>

## Demo

아래 유튜브 주소는 아래 명령어로 demo 파일을 실행한 영상이다.

```bash
# Download the landmarks example bag.
$ wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/mir/landmarks_demo_uncalibrated.bag
# Launch the landmarks demo.
$ roslaunch cartographer_mir offline_mir_100_rviz.launch bag_filename:=${HOME}/Downloads/landmarks_demo_uncalibrated.bag
```

<iframe src="https://www.youtube.com/embed/E2-OD-ycivc" width="640px" height="480px">

<br>

## ROS API

ROS API는 다음과 같이 구성되어 있다.

![https://google-cartographer-ros.readthedocs.io/en/latest/_images/nodes_graph_demo_2d.jpg](https://google-cartographer-ros.readthedocs.io/en/latest/_images/nodes_graph_demo_2d.jpg)

### [ Cartographer Node ]

### # Subscribed Topics

아래 토픽들은 상호 배제적(mutually exclusive)이다. 아래 데이터 중 최소 하나는 필요하다.

- scan ([sensor_msgs/LaserScan](http://docs.ros.org/api/sensor_msgs/html/msg/LaserScan.html))
- echoes ([sensor_msgs/MultiEchoLaserScan](http://docs.ros.org/api/sensor_msgs/html/msg/MultiEchoLaserScan.html))
- points2 ([sensor_msgs/PointCloud2](http://docs.ros.org/api/sensor_msgs/html/msg/PointCloud2.html))

필요하다면 아래의 추가 데이터를 제공할 수도 있다.

- imu ([sensor_msgs/Imu](http://docs.ros.org/api/sensor_msgs/html/msg/Imu.html))
- odom ([nav_msgs/Odometry](http://docs.ros.org/api/nav_msgs/html/msg/Odometry.html))

### # Published Topics

- scan_matched_points2 ([sensor_msgs/PointCloud2](http://docs.ros.org/api/sensor_msgs/html/msg/PointCloud2.html)) : Scan-to-submap 매칭을 위해서 사용한 포인트 클라우드
- submap_list ([cartographer_ros_msgs/SubmapList](https://github.com/cartographer-project/cartographer_ros/blob/master/cartographer_ros_msgs/msg/SubmapList.msg)) : 서브맵의 목록

### [ Occupancy grid Node ]

SLAM에 의해서 publish된 서브맵들을 listen하여 ROS occupancy_grid를 build하고 publish한다. 맵을 생성하는 것은 비용이 크고 느리기 때문에 수 초가 걸린다.

### # Subscribed Topics

- submap_list ([cartographer_ros_msgs/SubmapList](https://github.com/cartographer-project/cartographer_ros/blob/master/cartographer_ros_msgs/msg/SubmapList.msg))

### # Published Topics

- map ([nav_msgs/OccupancyGrid](http://docs.ros.org/api/nav_msgs/html/msg/OccupancyGrid.html))



> [참고]
>
> - **tf?** tf는 프레임을 추적할 수 있게 해주는 ROS 라이브러리이다. tf를 이용해서 두 프레임간의 변환 행렬을 얻거나 서로 다른 프레임의 정보를 Rviz에 보여주기 쉽게 하기도 한다. 또한 시간이 지나도 과거의 프레임을 추적할 수 있게 해준다.
> - **(Occupancy) Grid Map?** 공간을 cell로 나누어 표현하고, 각 cell은 occupied space와 free space로 구분된다. 두 영역은 수학적 계산을 통해서 나뉘어지는데, 자세한 사항은 [여기](http://jinyongjeong.github.io/2017/02/21/lec10_Grid_map)를 참고.
>
> ![img](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSQtLSk8z5xwoQ-NpvalAC3BnJfNsbLaEBr8w&usqp=CAU)

<br>

## Reference

[Google Open Source - Introducing Cartographer](https://opensource.googleblog.com/2016/10/introducing-cartographer.html)

[ICRA 2016 - Real-time Loop Closure in 2D LIDAR SLAM](https://research.google/pubs/pub45466/)

Cartographer ROS [Documentation](https://google-cartographer-ros.readthedocs.io/en/latest/) & [Github](https://github.com/cartographer-project/cartographer_ros)