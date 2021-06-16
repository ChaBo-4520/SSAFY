# 회전 변환 행렬

- 참고 URL

    [로드리게스 회전](https://jebae.github.io/2019/03/30/rodrigues-rotation/)

    [쿼터니언 회전](https://jebae.github.io/2019/03/30/quaternion-rotation/)

    [모터 제어. RPM](https://m.blog.naver.com/PostView.nhn?blogId=dolmangi&logNo=220530436434&proxyReferer=&proxyReferer=https:%2F%2Fwww.google.co.kr%2F)

    [PID 게인]([https://m.blog.naver.com/PostView.nhn?blogId=dkwltmdgus&logNo=220939484841&proxyReferer=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=dkwltmdgus&logNo=220939484841&proxyReferer=https:%2F%2Fwww.google.com%2F))

## 회전 변환 행렬

2차원 평면에서의 회전 변환 행렬 공식은 다음과 같다

![00](..\img\rotation_matrix\00.jpeg)

3차원에서의 회전 변환 행렬 공식은 다음과 같다

![01](..\img\rotation_matrix\01.png)

![02](..\img\rotation_matrix\02.png)

![03](..\img\rotation_matrix\03.png)

- Rigid 변환 구하기

![04](..\img\rotation_matrix\04.png)

1) p1이 원점이 되도록 A를 평행이동 시킨 후, 2) A'의 방향과 일치되도록 회전시켜서, 3) p1'까지 평행이동시킨다

## 쿼터니언 회전

![05](..\img\rotation_matrix\05.png)

roll, pitch, yaw 를 알고 있을 때 위 식을 통해 qq 를 찾고 qq 에 허수부의 부호를 달리한 q−1q−1 을 찾습니다. 마지막으로 세 사원수의 곱 qvq−1qvq−1 으로 벡터 vv 에 대한 회전을 구한다

## PID

P제어는 앞에 자동차랑 간격이 너무 멀리지면 그 간격에 비례해서 엑셀을 밟아주는거에요!

그런데 자동차가 오르막길을 올라간다고 가정해볼께요. 앞에 자동차랑 같은 간격이 난다고 해도 평평한 길에서랑 같은 세기로 엑셀을 밟으면 점점 차이가 날 수 밖에 없어요

그래서 I제어(적분제어!!)로 오차가 누적해서 점점 더 생기는 걸 더해서 엑셀을 밝아줘요!

그리고 마지막으로 자동차가 내리막길은 간다고 해볼께요! 그러면 앞에 자동차랑 같은 간격이 난다고 해서 같은 세기로 엑셀을 밝으면 부딛칠 수 밖에 없죠. 그래서 오차가 갑자기 줄어으면 그에 대한 보상으로 엑셀을 덜 밟도록 해야 해요. 이게 D제어에요!

시간에 연속적인 함수를 사용해서 미분하고 적분하고 좋겠지만..!

저희는 디지털로 신호을 받잖아요! 그러면 연속적인 값이 아니라 이산적인 값을 받게 되잖아요. 이것을 수치 미분, 수치 적분해야해요!

수치 적분은 정말 말그대로 오차를 더해주기만 하면 되요

그리고 미분은 (오차값 - 이전 오차값)/시간 으로 나눠서 수치미분하시면 되요!