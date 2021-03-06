# Access-Control-System
PPPoE-Radius Access Control System

##PPPoE协议运行原理：

基于 PPPoE 协议的接入控制实质上是通过在以太网上运行 PPP 协议，在每个以太网站点与 PPPoE 服务器之间建立一条 PPPoE 虚拟连接，每个虚拟连接具有唯一标识，通过虚拟连接实现以太网单个用户到 PPPoE 服务器之间的端到端通信。PPPoE 服务器通过虚拟连接就可以实现对以太接入的单个用户进行接入认证、授权和记账。 
PPPoE 协议操作分为两个阶段：PPPoE 发现阶段和 PPP 会话阶段。PPPoE 发现阶段完成以太网中 PPPoE 的会话建立，PPP 会话阶段则是在已建立的 PPPoE 会话中传送 PPP 帧。 

### PPPoE 发现阶段 
在 PPPoE 发现阶段，主机可能发现多个 PPPoE 服务器，但只在发现的所有 PPPoE 服务器中选择一个来建立 PPPoE 会话。发现阶段的操作如图 所示。

 
	主机广播一个 PADI(有效发现启动)分组，寻找合适的 PPPoE 服务器。 
	PPPoE 接入服务器收到其服务范围内的 PADI 后，如果可以向发送请求的主机提供服务时就会回应一个 PADO(有效分组发现提供)，否则不做任何回应。 
	主机收到 PADO 分组后，根据分组中的信息选择一个合适的接入服务器，然后向所选择的 PPPoE 服务器发送 PADR(有效发现请求)分组。 
	PPPoE 服务器收到 PADR 分组后准备开始 PPPoE 会话，为该会话产生一个唯一的会话标识，并在 PADS(有效发现会话证实)分组中将该会话标识交给主机。 
	
### PPP 会话阶段 
PPPoE 发现阶段结束后，主机和 PPPoE 接入服务器之间建立点到点隧道，进入会话阶段。在会话阶段，PPP 帧封装在 PPPoE 帧中，而 PPPoE 帧封装在 Ethernet 帧中，通过以太接入网承载、运送。会话阶段又分为以下几个过程：



####	链路建立阶段
客户机与PPPoE/S间：由LCP完成
建立请求、协商MRU、认证协议、链路质量监测协议，从而建立数据链路。
####	认证阶段
客户机与PPPoE/S间：由LCP和CHAP类认证协议协作完成
PPPoE/S与Radius/S间：由Radius协议完成
对用户身份的鉴别，若认证通过，则证明是合法的用户。
####	网络层协议阶段
客户机与PPPoE/S间：由NCP（IPCP）完成
对网络层协议进行配置，进行网络层地址（IP地址）分配
####	链路终止阶段
客户机与PPPoE/S间：由LCP完成
可以在任何时候终止链路，链路的正常终止由LCP分组完成，一个NCP的关闭不一定引起链路的关闭。

##RADIUS协议与RSA模型的运行
RADIUS (Remote Authentication Dial In User Service) 拨号用户远程认证服务，是一种通用的、广泛使用的实现AAA功能的协议，对接入用户提供认证授权和记账功能，且支持对漫游用户的接入控制，作用于NAS(这里就是PPPoE服务器)与RADIUS服务器之间，但并不直接定义用户与NAS之间的前台认证协议，可以与多种前台协议协同工作。
###交互过程：
-	用户：将用户名和口令信息发到PPPoE服务器
-	PPPoE服务器：将用户认证信息转发给RADIUS服务器
-	RADIUS服务器：发送质询值Challenge
-	PPPoE服务器：将质询值Challenge转发给用户
-	用户：将用户名和由计算得到的质询响应值发给PPPoE服务器
-	PPPoE服务器： 将用户名和质询响应值转发给RADIUS服务器
-	RADIUS服务器：验证用户的合法性并予以适当授权，发送Access-Accept或Access-Reject
-	PPPoE服务器：接受后台授权，控制用户接入
