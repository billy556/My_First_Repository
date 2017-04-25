## ROS安装


<font color=darkred size=3 face="黑体">要求：Ubuntu 14.04 LTS + ROS Indigo</font>


详细过程可以参考[Ubuntu install of ROS Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu)

<br>
### 1.配置我的Ubuntu的库

使计算机可以接受来自packages.ros.org的软件。

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'



### 2.设置密钥

    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116



### 2.安装
    
升级 

    sudo apt-get update
    
安装所有完整系统

    sudo apt-get install ros-indigo-desktop-full 


安装好后检查是否安装成功

    apt-cache search ros-indigo
    
    
### 3.初始化rosdep

在使用ROS之前，您需要初始化rosdep。rosdep使您能够轻松地为要编译的源安装系统依赖关系，并且需要在ROS中运行一些核心组件。

    sudo rosdep init
    rosdep update
    
    
### 4.环境设置
如果ROS环境变量每次启动新的shell时都会自动添加到bash会话中，这很方便：

    echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
    source ~/.bashrc



### 5.获取rosinstall

rosinstall是ROS中经常使用的命令行工具，它分开分发。它使您能够通过一个命令轻松下载许多ROS包的源代码树。

要在Ubuntu上安装此工具，请运行：

    sudo apt-get install python-rosinstall



<br>

<br>


<br>


