<h1 align="center">SSAFY 2ํ๊ธฐ ํนํ PJT ๐ค</h1>

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://lab.ssafy.com/s03-iot-sub2/s03p22a207/tree/master)](https://hits.seeyoufarm.com) ![SamSung Badge](https://img.shields.io/badge/-Samsung-blue?style=flat-square&logo=Samsung)

- SSAFY 3๊ธฐ 2ํ๊ธฐ ์์ธ 2๋ฐ 7ํ IoT์ ์ด

- 2020.08.31 ~ 2020.10.09 (์ฝ 7์ฃผ)

- Notion [๋ฐ๋ก๊ฐ๊ธฐ](https://www.notion.so/SSAFY-2-PJT-88cccf0c5ec64822ab4be1f5f9afc67b)

- Front-end : [Mock-up](https://ovenapp.io/view/mA68SiZmDPsl9VarKd1DZmLpHDBzIWrt)

<br>

## Members

๐ด **๊ณฝ์์ ** (@iamkkwak) | ๐  **๋ฐ์งํธ** (@wlgh325) | ๐ก **์์ฃผ์ฐ** (@wndus9382)

๐ข **์ดํฌ์ง** (@university1809) | ๐ต **์ฅํ๋** (@jkwkdz1) | ๐ฃ **์ฐจ๋ณด๋** (@ckqhfka4520)

<br>

## Docs

#### ๐ [ํ์๋ก](https://lab.ssafy.com/s03-iot-sub2/s03p22a207/tree/master/docs/notes) | ๐ [์คํฐ๋](https://lab.ssafy.com/s03-iot-sub2/s03p22a207/tree/master/docs/study) | ๐ก [๊ฟํ](https://lab.ssafy.com/s03-iot-sub2/s03p22a207/tree/master/docs/tips)

<br>

## Project

- SUB1 : ROS2 ํต์  ํ๋กํ ์ฝ ์ดํด ๋ฐ ํ์ฉ -  [๐](docs/project/sub1.md)

- SUB2 : ์ธ์ง, ํ๋จ ๋ฐ ์ ์ด - ์งํ์ค

<br>

### # 1. ํ๋จ/์ ์ด ํ๋ก์ ํธ

#### 1-1 ์ฃผํ๊ธฐ๋ก๊ณ

- ๋ก๋ด์ ์ํ ๋ฉ์์ง(์ ์๋, ๊ฐ์๋) ๋ฐ์์ ์ฃผํ๊ธฐ๋ก ๊ณ์ฐํ๊ธฐ
- ์ ์๋, IMU์ Quaternion์ ์ด์ฉํด ์ฃผํ๊ธฐ๋ก ๊ณ์ฐํ๊ธฐ

#### 1-2 ๊ฒฝ๋ก ๊ธฐ๋ก ๋ฐ ์ฝ์ด์ค๊ธฐ

- ์ฃผํ๊ธฐ๋ก ๊ธฐ๋ฐ ๊ฒฝ๋ก ์ ์ฅํ๊ธฐ
- ๊ฒฝ๋ก Publish ํ๊ธฐ
- ์ ์ฅํ ๊ฒฝ๋ก ์ฝ์ด์ Publish ํ๊ธฐ
- ๋ค์ํ ๊ฒฝ๋ก ์์ฑ

#### 1-3 ๊ฒฝ๋ก ์ถ์ข

- ๋ก๋ด์ ํ์ฌ ์์น, ๊ฒฝ๋ก๋ก ๋ฉ์์ง Subscribe
- ์ ์๋, ๊ฐ์๋ ๊ณ์ฐ
- ์ ์ด ๋ฉ์์ง Publish

![img](docs/img/sub2/map_path.jpg)</p>

### # 2. ์ธ์ง ํ๋ก์ ํธ
#### 2-1 Point Cloud ์ขํ ๋ณํ

- ์๋ฎฌ๋ ์ดํฐ์ LaserScan ๋ฉ์์ง ๋ฐ๊ธฐ
- Range/Angle๋ก ์ด๋ฃจ์ด์ง Point๋ฅผ x, y 2์ฐจ์์ผ๋ก ๋ณํ

![img](docs/img/sub2/local_coordinates.PNG)

#### 2-2 Extrinsic Calibration

- Utils.py ๋ด Lidar2Camera Transfromation matrix ๊ตฌํ
- Utils.py ๋ด project2cam()์ instrinsic matrix ๊ตฌํ
- ์นด๋ฉ๋ผ ์์ผ ๋ฐ์ ๋ชจ๋  2D ๋ผ์ด๋ค ํฌ์ธํธ ์ ๊ฑฐ
- ์นด๋ฉ๋ผ ์ด๋ฏธ์ง์ ๋ผ์ด๋ค ํฌ์ธํธ ๊ทธ๋ฆฌ๊ธฐ

![img](docs/img/sub2/lidar_calibration.png)

#### 2-3 ์นจ์์ ์ธ์ง

- BGR ์ด๋ฏธ์ง Grayscale ์ฑ๋ ๋ณํ
- ๋ณดํ์ ๊ฒ์ถ์ ์ํ HoG Descriptor ์ ์
- ๋ณดํ์ ๊ฒ์ถ์ ์ํ SVM Detector ํ๋ผ๋ฏธํฐ ๋ถ๋ฌ์ค๊ธฐ
- ํํธํ๋ ๋ณดํ์ bounding box ํตํฉ
- ์๋ ฅ ์ด๋ฏธ์ง ์์ ๋ณดํ์ ๊ฒ์ถ ๊ฒฐ๊ณผ์ธ bounding box ๊ทธ๋ฆฌ๊ธฐ

![img](docs/img/sub2/detect_intruder.png)

#### 2-4 ์์งํ ์ธ์ง

- Semantic segmentation image ๋ฐ์ดํฐ ํ์ฑ
- ์ง๊ฐ, ํค, ๋ฐฑํฉ, ๋ฆฌ๋ชจ์ฝ ์ด์งํ
- ๊ฐ ์์งํ์ bounding box ๊ณ์ฐ
- ์๋ ฅ ์ด๋ฏธ์ง ์์ ๋ณดํ์ ๊ฒ์ถ ๊ฒฐ๊ณผ์ธ bounding box ๊ทธ๋ฆฌ๊ธฐ

![img](docs/img/sub2/detect_thing.png)

<br>

### # 3. ๋งต ๊ธฐ๋ฐ ๊ฒฝ๋ก์์ฑ
#### 3-1 A*๋ฅผ ํ์ฉํ ์ ์ญ๊ฒฝ๋ก ์์ฑ

- ๋งต ๋ฐ์ดํฐ๋ฅผ ์ฝ์ด์ค๊ธฐ
- ๋ก๋ด์ ์ ๋ ์์น, ํค๋ฉ ์ ๋ณด๋ฅผ ์์ ํ๊ธฐ
- ๋ก๋ด์ ์์น (x, y)๋ฅผ grid map์ cell๋ก ๋งคํ
- A* ์๊ณ ๋ฆฌ์ฆ์ผ๋ก ์ต๋จ ๊ฒฝ๋ก ํ์
- grid map์ cell์ ์์น (x, y)๋ก ๋งคํ
- ๊ฒฝ๋ก ๋ฐ์ดํฐ publish ํ๊ธฐ

![img](docs/img/sub2/a-star.png)