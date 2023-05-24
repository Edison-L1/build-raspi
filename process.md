一、树莓派安装镜像
1、在官网上下载镜像烧录软件并安装。
官网：https://www.raspberrypi.com/software/

 
2、打开软件选择操作系统
 

 
 
这里我们选择64位系统。
 
选择好镜像之后选择相应的SD卡，点击烧录即可。（SD卡要格式化）
注意：这里我们要通过高级设置开启SSH及配置无线连接
	点击右下角设置
 
	开启SSH服务

 
	设置用户名以及密码
 
这里用默认用户名：pi
默认密码：raspberry

	配置wifi（方便查询树莓派ip）
 
二、配置好树莓派连接上位机
1、下载相关软件并安装
putty官网：https://www.putty.be/
vncviewer官网：https://www.realvnc.com/en/connect/download/viewer/
 
2、获取树莓派的ip地址
这里我们可以打开wifi路由器的官网来查询ip地址。
 
3、打开命令窗口
打开putty，输入我们获取的ip地址后点击open。
 
输入默认的用户名和密码。
 
在命令行下，输入sudo raspi-config，打开树莓派配置界面；
 
在 3 Interfacing Options里，设置使能VNC，建议顺便把Camera和SSH也都使能，修改完以后，按tab键退出选项，选择back按钮返回。
 
在Display Options里，VNC Resolution选1280*720，不能选第一个，否则远程桌面会黑屏。
4、登录远程桌面
打开vncviewer，输入账号密码就可以在Windows下登录树莓派的桌面。
 
三、安装pytorch环境
1、配置国内源
科大镜像网址：http://mirrors.ustc.edu.cn/help/raspbian.html

在命令窗口输入sudo nano /etc/apt/sources.list命令
 
编辑文件
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian bullseye main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian bullseye-updates main contrib non-free
deb https://mirrors.ustc.edu.cn/debian-security bullseye-security main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian-security/ bullseye-security main non-free contrib

编辑完文件后输入sudo apt-get update和sudo apt-get upgrade命令
 
 
更新软件索引。
2、上传whl文件
whl文件下载链接：https://github.com/Qengineering/PyTorch-Raspberry-Pi-64-OS
在命令窗口输入python3命令查看当前python版本
 
根据对应的python版本以及pytorch版本下载对应的whl文件

将我们提前下载好的 whl 文件上传到树莓派中。我这里是通过 VNC 软件上传的，你也可以通过其他你熟悉的方式上传。
 
3、开始安装
依次在命令窗口输入以下命令
	sudo apt-get install python3-pip libopenblas-dev libopenmpi-dev libomp-dev
	sudo -H pip3 install --upgrade setuptools
	sudo -H pip3 install Cython
	cd Downloads/
	sudo -H pip3 install torch-1.11.0a0+gitbc2c6ed-cp39-cp39-linux_aarch64.whl
	sudo -H pip3 install torchvision-0.12.0a0+9b5a3fe-cp39-cp39-linux_aarch64.whl

等待完成后在命令窗口输入依次输入
	python3
	import torch as tr
	tr.__version__
	print(tr.rand	(5,3))
 
这样pytorch就算安装成功。
四、USB摄像头配置
这里我们使用 USB 免驱工业摄像头，获取图像数据。（在上述步骤我们已经开启了树莓派的摄像头权限）

1、	设备接入
在插入USB摄像头前后都在命令窗口输入lsusb以便于观察是否有设备接入。
 
可以看到摄像头已经接入。
2、	测试
树莓派自带多个 USB 口，我们可以外接 USB 摄像头。如果驱动支持，默认会在系统的 dev 下，直接拟出来设备(video0、video1…).树莓派 opencv可以直接这个 video0 数据，进行视频显示、处理、录制保存等功能(程序运行过程中，因为程序锁定了video1...)虚拟视频设备，拔插摄像头后，这个序列号可能会改变，比如默认的 video0，会变成 video1) 。

在插入USB摄像头前后都在命令窗口输入ls /dev/video*
 
可以看到我们的摄像头设备为video0和video1。

五、
