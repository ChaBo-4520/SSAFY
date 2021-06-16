# Windows에서 Torch 설치

명세서에서 `pip install nets`를 하면 torch에 의존성이 있기 때문에 오류가 뜰 것이다.

찾아보니, Windows에서는 torch를 설치하는 명령어가 다르다고 한다!!

`pip install nets`를 하기 전에 torch를 먼저 설치해주면 되는데, Windows에서 torch를 설치하고 싶으면 아래 명령어를 입력하면 된다.

```bash
$ pip install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f <https://download.pytorch.org/whl/torch_stable.html>
```

<br>

## Reference

https://pytorch.org/