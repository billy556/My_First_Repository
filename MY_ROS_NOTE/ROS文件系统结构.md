## ROS文件系统


ROS 系统的架构主要被设计和划分成了三部分，每一部分都代表一个层级的概念:

- 文件系统级(Filesystem level)
- 计算图级(Computation Graph level)
- 开源社区级(Community level)

### 1.文件系统级

与其他操作系统相类似，一个 ROS 程序的不同组件要被放在不同的文件夹下。这些文件夹是根据功能的不同来对文件进行组织的:

<br>

组织关系：

![](http://ww4.sinaimg.cn/large/006tNc79gy1fet7ijlhb6j30jq0ff3z2.jpg)



- <font color=darkgreen size=2.5 face="黑体">*功能包(Package):*</font>  功能包是 ROS 中软件组织的基本形式。一个功能包具有最小的结构和最少的内容，用于创建 ROS 程序。它可以包含 ROS 运行的进程(节点)、配置文件等。
- <font color=darkgreen size=2.5 face="黑体">*功能包清单(Manifest):*</font>  功能包清单提供关于功能包、许可信息、依赖关系、编译标志等的信息。功能包清单是一个 _manifests.xml_ 文件，通过这个文件能够实现对功能包的管理。
- <font color=darkgreen size=2.5 face="黑体">*功能包集(Stack):*</font> 如果你将几个具有某些功能的功能包组织在一起，那么你将会获得一个功能包集。在 ROS 系统中，存在大量的不同用途的功能包集，例如导航功能 包集。
- <font color=darkgreen size=2.5 face="黑体">*功能包集清单(Stack manifest):*</font> 功能包集清单(_stack.xml_)提供一个关于功能包集的清单，包括开源代码的许可证信息、与其他功能包集的依赖关系等。
- <font color=darkgreen size=2.5 face="黑体">*消息类型(Message /msg type):*</font> 消息是一个进程发送到其他进程的信息。ROS系统有很多的标准类型消息。消息类型的说明存储在 my_package/msg/ MyMessageType.msg 中，也就是对应功能包的 msg 文件夹下。
- <font color=darkgreen size=2.5 face="黑体">*服务类型(Service/srv type):*</font> 对服务的类型进行描述说明的文件在 ROS 系统中定义了服务的请 求和响应的数据结构。这些描述说明存储在 my_ package/srv/ MyServiceType.srv 中，也就是对应功能包的 srv 文件夹下。


### 2.ROS计算图级

ROS 会创建一个连接到所有进程的网络。在系统中的任何节点都可以访问此网络，并通过该网络与其他节点交互，获取其他节点发布的信息，并将自身数据发布到网络上。


![](http://ww2.sinaimg.cn/large/006tNc79gy1fet8brioqcj30j8086aay.jpg)


- <font color=darkgreen size=2.5 face="黑体">*节点(Node)：*</font> 节点是主要的计算执行进程。如果你想要有一个可以与其他节点进行交互的进程，那么你需要创建一个节点，并将此节点连接到ROS网络。通常情况下，系统包含能够实现不同功能的多个节点。你最好让每一个节点都具有特定的单一的功能，而不是在系统中创建一个包罗万象的大节点。节点需要使用如 'roscpp' 或 'rospy' 的 ROS 客户端库进行编写。
- <font color=darkgreen size=2.5 face="黑体">*节点管理器(Master)：*</font> 节点管理器用于节点的名称注册和查找等。如果在你的整个 ROS 系统中没有节点管理器，就不会有节点、服务、消息之间的通信。需要注意的 是，由于 ROS 本身就是一个分布式网络系统，你可以在某一台计算机上运行节点管 理器，在其他计算机上运行由该管理器管理的节点。
- <font color=darkgreen size=2.5 face="黑体">*参数服务器(Parameter Server)：*</font> 参数服务器能够使数据通过关键词存储在一个系统的核心位置。通过使用参数，就能够在运行时配置节点或改变节点的工作任务。
- <font color=darkgreen size=2.5 face="黑体">*消息(Message)：*</font> 节点通过消息完成彼此的沟通。消息包含一个节点发送到其他节点的数据信息。ROS 中包含很多种标准类型的消息，同时你也可以基于标准消息开发自定义类型的消息。
- <font color=darkgreen size=2.5 face="黑体">*主题(Topic)：*</font> 主题是由 ROS 网络对消息进行路由和消息管理的数据总线。每一条消息都要发布到相应的主题。当一个节点发送数据时，我们就说该节点正在向主题发布消息。节点可以通过订阅某个主题，接收来自其他节点的消息。一个节点可以订阅一个主题，而并不需要该节点同时发布该主题。这就保证了消息的发布者和订阅者之间相互解耦，完全无需知晓对方的存在。主题的名称必须是独一无二的，否则在同名主题之间的消息路由就会发生错误。

- <font color=darkgreen size=2.5 face="黑体">*服务(Service)：*</font> 在发布主题时，正在发送的数据能够以多对多的方式交互。但当你需要从某个节点获得一个请求或应答时，就不能通过主题来实现了。在这种情况下， 服务能够允许我们直接与某个节点进行交互。此外，服务必须有一个唯一的名称。当一个节点提供某个服务时，所有的节点都可以通过使用 ROS 客户端库所编写的代码 与它通信。
- <font color=darkgreen size=2.5 face="黑体">*消息记录包(Bag)：*</font> 消息记录包是一种用于保存和回放 ROS 消息数据的文件格式。 消息记录包是一种用于存储数据的重要机制。它能够获取并记录各种难以收集的传感器数据。我们可以通过消息记录包反复获取实验数据，进行必要的开发和算法测试。 在使用复杂机器人进行实验工作时，需要经常使用消息记录包。


<font color=darkred size=4 face="黑体">此处缺少一个节点状态图</font>
