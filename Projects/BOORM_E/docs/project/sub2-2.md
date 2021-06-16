>  polar_cartesian.py

```python
#!/ C:\\Python37\\python.exe
import numpy as np
import matplotlib.pyplot as plt
import rclpy
from rclpy.node import Node

from sensor_msgs.msg import LaserScan
from math import sin, cos, pi, radians

class PolarCartesian(Node):

    def __init__(self):
        super().__init__(node_name='polar_cartesian')

        self.subs_pc = self.create_subscription(LaserScan, '/scan', self.pc_callback, 10)
        self.pc_msg = None
        
        plt.ion()
        plt.show()
        fig = plt.figure(0)
        fig.canvas.set_window_title("Local cartesian points")

        self.timer_period = 0.05
        self.timer = self.create_timer(self.timer_period, self.timer_callback)

        
    def pc_callback(self, msg):
        
        self.pc_msg = msg

    def timer_callback(self):

        if self.pc_msg is not None:

            x = np.array([])
            y = np.array([])
            for i in range(len(self.pc_msg.ranges)-1, -1, -1):
                x = np.append(x, np.array([-self.pc_msg.ranges[i] * sin(radians(i))]))
                y = np.append(y, np.array([self.pc_msg.ranges[i] * cos(radians(i))]))
            
            plt.clf()  # plot을 매번 지우고 다시 그림
            plt.plot(x, y, 'b.')
            plt.plot(0, 0, 'r^')  # 로봇의 현재 위치
            plt.pause(0.001)

        else:
            pass

def main(args=None):

    rclpy.init(args=args)
    polar_cartesian = PolarCartesian()
    rclpy.spin(polar_cartesian)

if __name__ == '__main__':

    main()
```

![img](../img/sub2/local_coordinates.png)

로봇의 시점(0, 0)에서 나타난 Local 좌표계입니다.

움직여보면 알겠지만 로봇의 위치는 빨간색, 장애물의 위치는 파란색으로 나타납니당