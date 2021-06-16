# SUB1

ROS 통신 프로토콜 이해 및 활용

<br>

## Agenda

#### # Req 1. 스켈레톤 프로젝트 개발 환경 구축

- [ROS2](#ros2)

- 시뮬레이터

- [Visual Studio Code 설치](#vs-code)

- [workspace 만들기 및 스켈레톤 코드 다운](#build)

<br>

#### # Req 2. ROS2 통신 프로토콜

- 로봇 메시지 송신 및 수신

- IoT 메시지 송신 및 수신

- 환경 메시지 수신

<br>

#### # Req 3. 영상 데이터 수신 및 처리결과 전송

- 이미지 parsing 및 데이터 처리

<br>

<br>

---

<br>

<br>

## Req1. 스켈레톤 프로젝트 개발환경 구축



### ROS2

1. Microsoft Store에서 Windows Terminal이라는 프로그램을 다운로드 받는다.
   편리하게 터미널 창을 관리할 수 있다.

2. powershell을 관리자 권한으로 실행하고, [여기]에서 커맨드를 복사해 choco를 설치한다.

   ```bash
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

   설치 후 `choco`를 입력해서 버전이 나오면 정상적으로 설치된 것이다.

3. choco install 명령어를 이용해 Python 3.7.5 버전과 c++ redistributables를 설치한다.

   ```bash
   $ choco install -y python --version 3.7.5
   $ choco install -y vcredist2013 vcredist140
   ```

4. Visual Studio 2019 Community 버전을 [여기](https://visualstudio.microsoft.com/downloads)에서 설치한다. 설치할 때 C++을 사용한 데스크톱 개발 패키지를 같이 설치한다.

5. [OpenSSL 설치 사이트](https://slproweb.com/products/Win32OpenSSL.html)에서 win64.1.0.2u 버전을 다운로드 한 뒤, 아래 명령어를 입력한다.

   ```bash
   $ setx -m OPENSSL_CONF 'C:\OpenSSL-Win64\bin\openssl.cfg'
   ```

   환경 변수 `Path`에 `C:\OpenSSL-Win64\bin\`도 등록해준다.

6. ROS2의 [OpenCV Archive](https://github.com/ros2/ros2/releases/tag/opencv-archives)에서 3.4.6 버전의 압축 파일을 다운로드하고, C:\에 압축 해제한 후 `OpenCV_DIR`이라는 환경 변수로 경로를 지정해준다. 또는 cmd 창에

   ```bash
   $ setx -m OpenCV_DIR C:\opencv
   ```

   로 설정해준다.

7. 종속 패키지에 대한 설치를 진행한다.

   ```bash
   $ choco install -y cmake
   ```

   설치 후, 환경변수 `Path`에 `C:\Program Files\CMake\bin`을 추가한다.
   [여기](https://github.com/ros2/choco-packages/releases/tag/2020-02-24)에서 `.nupkg` 확장자의 파일들을 전부 다운로드 하고, 다운로드 받은 폴더로 이동한다. 해당 패키지와 Python 종속 패키지를 설치한다.

   ```bash
   $ choco install -y -s .asio cunit eigen tinyxml-usestl tinyxml2 log4cxx bullet
   ```

   ```bash
   $ python -m pip install -U pydot PyQt5 pyqtgraph catkin_pkg cryptography empy ifcfg lark-parser lxml netifaces numpy opencv-python pyparsing pyyaml setuptools
   ```

8. ROS2와 opensplice를 설치한다. C:\에 `dev`라는 폴더를 만든다.
   [여기](https://github.com/ros2/ros2/releases/tag/release-eloquent-20200124)에서 ROS2 Eloquent 버전을 다운 받아 `dev` 폴더 아래 `ros2-eloquent`라는 이름의 폴더를 만들어 압축을 푼다. 그 뒤, [여기](https://github.com/ADLINK-IST/opensplice/releases/tag/OSPL_V6_9_190403OSS_RELEASE)에서 `dev` 폴더 아래 `opensplice`라는 이름의 폴더를 만들어 압축을 푼다.

9. RTI Connex를 [여기](https://www.rti.com/free-trial/dds-files-5.3.1)를 통해 다운로드 받고, `.exe`의 설치 파일로 설치를 진행한다.

10. 터미널 창을 2개 열고, 각각 ROS2의 데모를 실행시킨다. talker에서 publishing 된 메세지와 listener에서 받은 메세지가 보인다면 설치가 완료된 것이다.

    ```bash
    $ call C:\dev\ros2_eloquent\setup.bat
    $ ros2 run demo_nodes_cpp talker
    ```

    ```bash
    $ call C:\dev\ros2_eloquent\setup.bat
    $ ros2 run demo_nodes_py listener
    ```



> 10번의 과정을 진행할 때, 계속 아래와 같은 오류가 났다. 
>
> ```bash
> $ ros2 run demo_nodes_py listener
> ...
> RuntimeError: Failed to initialize init_options: failed to load shared library of rmw implementation. Exception: Cannot load library: C:\dev\ros2_eloquent\bin/rmw_fastrtps_cpp.dll
> ...
> ```
>
> opensplice 문제 같아서 `OSPL_HOME` 환경 변수 설정도 해주고, 삭제하고 재설치하고 한참 헤맸는데 같은 문제로 고군분투 중이다가 해결한 팀원의 도움으로 해결할 수 있었다. 
> 아래 명령어로 앞서 설치한 dependency들을 삭제하고 재설치한 뒤, cmd 창을 껐다 켜니까 해결이 됐다.
>
> ```bash
> $ choco uninstall -y -s . asio cunit eigen tinyxml-usestl tinyxml2 log4cxx bullet --force
> ```
>
> ```bash
> $ choco install -y -s . asio cunit eigen tinyxml-usestl tinyxml2 log4cxx bullet --force
> ```

<br>

### VS Code

1. Extension 창에 들어가서 python과 ROS를 install을 눌러 설치한다.
2. ctrl + shift + P로 Python: Select Interpreter 옵션을 선택하면 뜨는 path 창에서 Python 3.7을 누르면 python interpreter를 사용할 수 있다.

<br>

### Build

1. `catkin_ws`라는 workspace 폴더를 만든다. 그리고 그 안에 `src` 폴더를 만든다. (명세서에서는 바탕화면에 만들라고 했지만 C드라이브가 편해서 해당 위치에 만들었다.)

2. Colcon 빌드 확장 패키지를 다운받는다.

   ```bash
   $ pip install -U colcon-common-extensions
   ```

3. VS의 c++ 컴파일러를 사용하기 때문에 `x86 native tools command prompt for VS 2019`를 사용한다. 매번 환경변수는 수동으로 입력해주어야 한다.

   ```bash
   $ call C:\dev\ros2_eloquent\setup.bat
   ```

4. `catkin_ws`의 `src` 폴더 안에 소스 코드 파일 압축을 풀고, 스켈레톤 코드에 필요한 파이썬 패키지를 다운로드 받는다.

   ```bash
   $ pip install squaternion matplotlib python-socketio python-socketio-client
   ```

   catkin_ws 폴더 위치에서 아래와 같은 명령어를 이용해 빌드한다.

   ```bash
   $ colcon build
   ```

   이 때, 특정 패키지만 빌드하려면 `--packages-select` 옵션을 추가하면 된다. 이미 빌드를 한 적이 있으면 `--merge-install`을 붙여준다.

5. `catkin_ws`의 환경 변수를 등록한다.

   ```bash
   $ call C:\catkin_ws\intall\local_setup.bat
   ```

