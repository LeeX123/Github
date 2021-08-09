# Service Group

## BIRD

[BIRD](https://bird.network.cz/) 项目旨在开发一个全功能的动态 IP 路由守护进程，主要针对(但不限于) Linux、 FreeBSD 和其他类 unix 系统，并在 GNU通用公共许可协议下发布。

### BGP

### OSPF

### RADV

>Router Advertisement

路由器广告守护进程(RADVD)用于 IPv6的自动配置和路由。如果启用，消息将由路由器周期性地发送并响应请求。主机使用这些信息来学习本地网络的前缀和参数。禁用此功能将有效禁用自动配置，需要手动配置每个设备上的 IPv6地址、子网前缀和默认网关。

在具有多播功能的链路和点对点链路上，每个路由器定期向多播组发送一个路由器广告包，宣布其可用性。主机接收来自所有路由器的路由器广告，构建默认路由器列表。路由器频繁地产生路由器广告，以便主机在几分钟内了解它们的存在。然而，路由器的广告不够频繁，不能依靠广告来检测路由器故障。确定邻居不可达性的单独检测算法提供故障检测。

### RIP

### Static

> 静态路由协议

## EMANE

可扩展的移动 Ad-hoc 网络模拟器(EMANE)允许异质网路使用可插拔的 MAC 和 PHY 层架构进行模拟。EMANE 框架以网络仿真模块(nem)的形式为不同的无线接口类型建模提供了一个实现架构，并将这些模块合并到分布式环境中运行的实时仿真中。

EMANE (extensible Mobile Ad-hoc Network Emulator)是一个开源框架，它为无线网络实验者提供了一个高度灵活的模块化环境，用于设计、开发和测试简单和复杂的网络结构。EMANE 使用一个物理层模型来考虑信号传播、天线轮廓效应和干扰源，以便为无线实验提供一个真实的环境。单独的无线电模型插件用于模拟波形的最低层，并且可以与现有的软件定义无线电(SDR)实现相结合，以支持共享代码仿真。

### Transport Service

虚拟运输能力包括以下方面:

1. IPv4 and IPv6 Capable - Supports IPv4 and IPv6 virtual interface address assignments and packet processing.

   IPv4和 IPv6 Capable-支持 IPv4和 IPv6虚拟接口地址分配和数据包处理。

2. Flow Control - Supports flow control with a corresponding flow control capable NEM layer in order to provide feedback between the emulation stack and application domain socket queues.

   流量控制-支持流量控制与相应的流量控制能力的 NEM 层，以提供反馈之间的仿真堆栈和应用程序域套接字队列。

3. Virtual Interface Management - Supports configuring virtual interface addresses or can be configured to allow virtual interfaces to be managed externally, for example via DHCP.

   虚拟接口管理——支持配置虚拟接口地址，或者可以配置虚拟接口以允许外部管理虚拟接口，例如通过 DHCP。

4. Raw Transport Interoperability - Supports interoperability with [Raw Transport](https://github.com/adjacentlink/emane/wiki/Raw-Transport) emulation/application domain boundaries using ARP caching to learn network/NEM Id associations.

   原始传输互操作性-支持与原始传输仿真/应用程序域边界的互操作性，使用 ARP 缓存来学习网络/NEM Id 关联。

5. Bitrate Enforcement - Supports bitrate enforcement for use with models that do not limit bitrate based on emulation implementation.

   基于仿真实现，支持在不限制比特率的模型中使用比特率强制。

6. Broadcast Only Mode - Supports forced NEM broadcasting of all IP packet types: unicast, broadcast and multicast.

   广播模式-支持所有 IP 包类型的强制 NEM 广播: 单播，广播和多播。

## [FRR](https://docs.frrouting.org/en/latest/index.html)

FRRouting (FRR)是一个针对 Linux 和 Unix 平台的免费开源互联网路由协议套件。它实现了 BGP，OSPF，RIP，IS-IS，PIM，LDP，BFD，Babel，PBR，OpenFabric 和 VRRP，对 EIGRP 和 NHRP 的 alpha 支持。

FRR 与原生 Linux/Unix IP 网络堆栈的无缝集成使其成为一个通用的路由堆栈，适用于各种各样的用例，包括连接主机/虚拟机/容器到网络、广告网络服务、局域网交换和路由、互联网接入路由器和互联网监视。

FRR 起源于 Quagga 项目。

### [BABEL](https://docs.frrouting.org/en/latest/babeld.html)

BABEL是一种内部网关协议，既适用于有线网络，也适用于无线网状网络。BABEL被描述为“ RIP on speed”——它基于与 RIP 相同的原理，但包括一些改进，使其对拓扑结构变化的反应更快，而不用计数到无穷大，并允许它在无线链路上执行可靠的链路质量评估。Babel 是一个双协议栈路由协议，这意味着一个 Babel 实例可以同时为 IPv4和 IPv6执行路由。

FRR 实现了 RFC 6126中所描述的BABEL。

### BGP

### OSPFv2

### OSPFv3

### PIMD

> PIM – Protocol Independent Multicast(协议无关组播)

### RIP

### RIPNG

> 路由信息协议的下一代(RIPng)是一个内部网关协议，它使用距离向量算法来确定到达目的地的最佳路径，使用跳数作为度量指标。RIPng 交换用于计算路由的路由信息，用于基于 IP 版本6(IPv6)的网络，是为支持 IPv6而开发的 RIP 扩展。在默认情况下，RIPng 是禁用的。

### Zebra

Zebra 是一个 IP 路由管理器。它提供了内核路由表更新、接口查找和不同路由协议之间路由的重新分配。

## NRL

> NRL(Naval Research Laboratory:美国海军研究实验室)

Protian Protocol Prototyping Library (ProtoLib)是一个跨平台的库，允许构建应用程序，同时支持多种平台，包括 Linux、 Windows、 WinCE/PocketPC、 MacOS、 FreeBSD、 Solaris 等，以及 NS2和 Opnet 的仿真环境。Protolib 的目标是提供一组简单的跨平台 c + + 类，允许开发可以在不同平台和网络模拟环境中运行的网络协议和应用程序。尽管 Protolib 为开发工作协议实现、应用程序和模拟模块提供了一个总体框架，但是各个类被设计为尽可能作为独立组件使用。尽管 Protolib 主要是用于研究目的，但是代码的构造是为了提供健壮、高效的性能和对实际应用的适应性。在某些情况下，代码由数据结构等组成，在协议实现中很有用，在其他情况下，代码为系统服务和功能(如套接字、定时器、路由表等)提供通用的跨平台接口。

目前，海军研究实验室使用这个库来开发各种各样的协议。

### arouted

### MGEN Sink

多生成器(MGEN)网络测试工具是一个开源软件，由 NRL 协议工程高级网络(protel)研究小组开发。MGEN 提供了使用传输控制协议协议(TCP)和用户数据报协议协议(UDP/IP)流量执行互联网协议(IP)网络性能测试和测量的能力。工具集生成实时流量模式，以便网络可以以多种方式加载。还可以接收和记录生成的流量以便进行分析。脚本文件用于在一段时间内驱动生成的加载模式。这些脚本文件可用于模拟单播和/或多播 UDP 和 TCP IP 应用程序的流量模式。这个工具集的接收部分可以用脚本动态地加入，离开 IP 多播组并侦听流量。MGEN 日志数据可以通过 Tcpdump Rate Plot Real Time (TRPR)或其他工具计算吞吐量、丢包率、通信延迟等性能统计数据。MGEN 目前运行在各种基于 unix 的平台(包括 MacOS x)和 Win 32平台上。MGEN Version 5.0支持 TCP 消息传递、 IPv6网络，并且除了广泛的基于 unix 的平台之外，还支持 Win32平台。增加了对额外的统计流量生成模式(JITTER 和 CLONE)、传输缓冲、消息计数和有效负载增强的支持。

### MGEN Actor

Mgen Utility生成实时流量模式，以便可以以各种方式加载模拟网络。 它是由海军研究实验室开发的。 我们必须安装mgen以在核心网络分析仪中使用流量生成函数。 更多信息可用于：http://www.nrl.navy.mil/itd/ncs/products/mgen。

### NHDP

邻域发现协议（NHDP，RFC 6130）为基于移动IP的网络提供了两跳邻域发现。

更多信息和安装说明可用于：http：//www.nrl.navy.mil/itd/ncs/products/nhdp

### OLSR

优化链路状态路由

### [OLSRORG](http://www.olsr.org/mediawiki/index.php/Main_Page)

### OLSRv2

### SMF

NRL 简化组播转发(nrlsmf)项目是用户空间软件实现的简化组播转发引擎(SMF，RFC 6621)。该软件由海军研究实验室(NRL)协议工程高级网络(protel)研究小组开发。这项工作的目标是提供一种实验技术的实现，以便在动态的无线网络(如移动自组网)中稳健、有效地分发广播或多播数据包。Nrlsmf 应用程序可以作为一个独立的应用程序运行，能够为指定的网络接口提供广播和多播通信的“经典”洪流，或者可以与控制程序一起使用，以执行更复杂的多播转发。提供了一个进程间通信“远程控制”接口，使兼容程序(例如 NRL-OLSR)可以向 nrlsmf 发出实时命令来控制组播转发进程。支持 IPv4和 IPv6操作。可以为以下操作系统构建版本的 nrlsmf: Linux、 MacOS、 BSD、 Win32和 WinCE。

简化的多播转发（SMF）提供适用于无线网格和移动临时网络（MANET）的基本互联网协议（IP）多播转发。 它由（RFC 6621）描述。

更多信息和安装说明可用于：http://www.nrl.navy.mil/itd/ncs/products/smf。

## [Quagga](https://quagga.net/)

[Quagga (http://www.quagga.net)](https://quagga.net/docs/quagga.html)

是一个路由软件套件，为 Unix 平台，特别是 FreeBSD，Linux，Solaris 和 NetBSD 提供了 OSPFv2，OSPFv3，RIP v1和 v2，RIPng 和 BGP-4的实现。Quagga 是 GNU Zebra 的一个分支，是由石黑SDN国弘开发的。

体系结构由一个核心守护进程 zebra 组成，它充当底层 Unix 内核的抽象层，通过 Unix 或 TCP 流向 Quagga 客户端显示 Zserv API。正是这些 Zserv 客户机通常实现一个路由协议协议，并将路由更新通知到 zebra 守护进程。

每个 Quagga 守护进程都可以通过网络可访问的 CLI (称为“ vty”)进行配置。CLI 遵循与其他路由软件类似的风格。Quagga 还包含一个称为“ vtysh”的附加工具，它作为所有守护进程的单一内聚前端，允许人们在一个地方管理各个 Quagga 守护进程的几乎所有方面。

### BABEL

### BGP

### OSPFv2

### OSPFv3

### [OSPFv3 MDR](https://www.nrl.navy.mil/Our-Work/Areas-of-Research/Information-Technology/NCS/OSPF-MANET/)

开放式最短路径优先-manet 指定路由器(OSPF-MDR)是针对移动 Ad hoc 网络(manet)高效路由的 rfc5614(OSPFv3)、 rfc5243(OSPFv3)和 rfc5838(OSPFv3地址族)的实现。

该软件基于开源的 Quagga Routing Suite，最初由 Richard Ogier 和波音鬼怪工厂开发，现在由 NRL 的移动路由项目维护。

### RIP

### RIPNG

### XPIMD

[ospf-mdr/xpimd.service at master · USNavalResearchLaboratory/ospf-mdr (github.com)](https://github.com/USNavalResearchLaboratory/ospf-mdr/blob/master/redhat/xpimd.service)

### Zebra

## SDN

**软件定义网络**（英语：software-defined networking，缩写作 **SDN**）是一种新型网络架构。它利用[OpenFlow](https://zh.wikipedia.org/wiki/OpenFlow)协议将[路由器](https://zh.wikipedia.org/wiki/路由器)的[控制平面](https://zh.wikipedia.org/w/index.php?title=控制平面&action=edit&redlink=1)（control plane）从数据平面（data plane）中分离，改以[软件](https://zh.wikipedia.org/wiki/軟體)方式实现，从而使得将分散在各个网络设备上的控制平面进行集中化管理成为可能 ，该架构可使[网络管理员](https://zh.wikipedia.org/wiki/網絡管理員)在不更动[硬件](https://zh.wikipedia.org/wiki/硬件)设备的前提下，以中央控制方式用[程序](https://zh.wikipedia.org/wiki/程序)重新规划[网络](https://zh.wikipedia.org/wiki/计算机网络)，为控制[网络流量](https://zh.wikipedia.org/wiki/网络流量)提供了新方案，也为核心网络和应用创新提供了良好平台。

### [OVS](https://en.wikipedia.org/wiki/Open_vSwitch)

Open vSwitch，有时缩写为 OVS，是一个分布式虚拟 multiple-switch 的开源实现。的主要目的是提供一个硬件虚拟化/服务环境的交换堆栈，同时支持计算机网络中使用的多种协议和标准。

The project's source code is distributed under the terms of [Apache License 2.0](https://en.wikipedia.org/wiki/Apache_License_2.0).

该项目的源代码是根据 Apache License 2.0条款发布的。

### [RYU](https://ryu-sdn.org/)

Ryu 是一个基于组件的软件定义的网络框架。Ryu 为软件组件提供了定义良好的 API，使得开发人员可以轻松地创建新的网络管理和控制应用程序。Ryu 支持各种用于管理网络设备的协议，如 OpenFlow、 Netconf、 OF-config 等。关于 OpenFlow，Ryu 完全支持1.0,1.2,1.3,1.4,1.5和 Nicira Extensions。所有的代码都可以在 Apache 2.0许可下免费获得。

## Security

### Firewall

防火墙是一种软件程序，或系统，可以配置为在两个网络之间强制执行数据访问策略。 防火墙通常会阻止不符合一组配置规则的网络数据流量，以便防止不符合数据分组。 防火墙软件也可以在主机上配置，以保护应用程序在该计算机上运行的网络连接。

标准的Linux防火墙解决方案是iptables，它已安装在大多数Linux发行版中。 有关更多信息，请参阅：https://help.ubuntu.com/community/iptableshowto。

### IPsec

互联网协议保安(IPsec)是一个安全的网络协议组合，对数据包进行认证和加密，使两台电脑通过互联网协议网络进行安全的加密通信。它用于虚拟专用网络(vpn)。

IPsec 包括在会话开始时在代理之间建立相互身份验证的协议，以及在会话期间使用的密钥的协商。IPsec 可以保护一对主机(主机对主机)之间、一对安全网关(网络对网络)之间或安全网关与一个主机(网络对主机)之间的数据流。IPsec 使用加密安全服务来保护 IP 网络上的通信。它支持网络级别的对等认证、数据源认证、数据完整性、数据机密性(加密)和重放保护。

最初的 IPv4套件是在几乎没有安全条款的情况下开发的。作为 IPv4增强的一部分，IPsec 是一种三层 OSI 模型或者说是一种网络层端到端的安全机制。相比之下，虽然其他一些广泛使用的互联网安全系统在第3层之上运行，如在传输层运行的传输层安全性(TLS)和在应用层运行的安全 Shell (SSH) ，但 IPsec 可以在 IP 层自动保护应用程序。

IPSec是一组网络层协议，提供加密和验证在不安全IP网络上两台计算机之间发送的数据。 许多VPN使用IPSec作为VPN解决方案中的加密层。 核心网络仿真器的脚本还假设IPSec密钥管理器应用程序，varoon安装在系统中。

有关更多信息，请参阅：http://ipsec-tools.sourceforge.net/。

### NAT

### VPN Client

虚拟专用网络（VPN）解决方案类似于SSH，在其上提供两台计算机之间的加密通道，在不安全的网络上。 VPN通常提供比SSH隧道更多的功能，但成本更加复杂。

有关更多信息，请参阅：http://openvpn.net/index.php/open-source.html

### VPN Server

## Utility

### ATD

ATD守护程序通过使用AT命令在将来运行时执行计划的命令。 它类似地运行到Cron，但用于在将来计划和运行一次性事件。

更多信息可用于：http://debian-handbook.info/browse/stable/sect.task-scheduling-cron-atd.html.

### Routing Utils

### DHCP

### FTP

文件传输协议（FTP）是用于通过IP网络将文件从一台计算机传输到另一台计算机的网络协议。 基于TCP的网络，例如互联网。

大多数Linux发行版已经包括FTP客户端。 对于FTP PSERVER，核心预计将使用VSFTPD，“非常安全的FTP守护程序”，作为FTP服务器。 更多信息可在：https://help.ubuntu.com/community/vsftpd。

### IP Forward

IP转发，转发即当主机拥有多于一块的网卡时，其中一块收到数据包，根据数据包的目的ip地址将数据包发往本机另一块网卡，该网卡根据路由表继续发送数据包。这通常是路由器所要实现的功能。

### PCAP

数据包捕获（PCAP）是一个程序，可以从网络接口捕获数据包并将内容发送到文件。 TCPDUMP是一个命令行数据包分析器，即核心网络仿真器用于执行PCAP函数并分析捕获的数据包。 更多信息可用于：http://www.tcpdump.org/。

### RADVD

系统管理员使用路由器通告守护程序（Radvd）以自动在IPv6网络上配置网络主机。 更多信息可用于：http://www.litech.org/radvd/。

### [SSF](https://securesocketfunneling.github.io/ssf/#home)

安全套接字漏斗（SSF）是一个网络工具和工具包。

它提供了通过单个安全TLS链接到远程计算机的单个安全TLS链接来提供从多个套接字（TCP或UDP）的简单有效的方法。

### UCARP

常见的地址冗余协议（CARP）是自动故障转移和冗余协议。 Carp旨在在同一网络段中的多个主机之间共享常见的IP地址，以便为多个服务器或主机提供故障转移冗余。 鲤鱼设计为自由源和开源交替，与虚拟路由器冗余协议（VRRP）2交替。

更多信息可用于：http://www.pureftpd.org/project/ucarp和http://manpages.ubuntu.com/manpages/lucid/man8/cumarp.8.html。

## XORP

> XORP 是一个开源的 Internet 协议路由软件套件，最初由加利福尼亚州伯克利的国际计算机科学研究所设计。名称来源于可扩展开放式路由器平台。它支持 OSPF，BGP，RIP，PIM，IGMP，OLSR。
>
> XORP是另一个开源路由平台。 XORP提供包含的组播路由支持，这可能是在模拟中选择使用XORP而不是Quagga的主要原因。
>
> 更多信息可用于：http://www.xorp.org/。

### BGP

> 边界网关协议

### [OLSR]([Optimized Link State Routing Protocol - Wikipedia](https://en.wikipedia.org/wiki/Optimized_Link_State_Routing_Protocol))

> Optimized Link State Routing Protocol(优化链路状态路由协议)，优化链路状态路由协议(OLSR)[1]是一个为移动自组网络优化的 IP 路由协议，它也可以用于其他无线自组网络。OLSR 是一个主动的链路状态路由协议，它使用 hello 和拓扑控制(TC)消息来发现和传播整个随建即连网络的链路状态信息。各个节点使用此拓扑信息使用最短跳转路径计算网络中所有节点的下一跳目的地。

### OSPFv2

> **开放式最短路径优先**（英语：Open Shortest Path First，缩写为 OSPF）是一种基于[IP协议](https://zh.wikipedia.org/wiki/IP协议)的[路由协议](https://zh.wikipedia.org/wiki/路由协议)。它是大中型网络上使用较为广泛的[IGP](https://zh.wikipedia.org/wiki/内部网关协议)协议。OSPF是对[链路状态路由协议](https://zh.wikipedia.org/w/index.php?title=链路状态路由协议&action=edit&redlink=1)的一种实现，运作于[自治系统](https://zh.wikipedia.org/wiki/自治系统)内部。OSPF分为OSPFv2和OSPFv3两个版本：OSPFv2定义于[RFC 2328](https://tools.ietf.org/html/rfc2328)（1998），支持[IPv4](https://zh.wikipedia.org/wiki/IPv4)网络；而OSPFv3定义于[RFC 5340](https://tools.ietf.org/html/rfc5340)（2008），支持[IPv6](https://zh.wikipedia.org/wiki/IPv6)网络。

### OSPFv3

### PIMSM4

> PIM(Protocol Independent Multicast: 协议无关组播),协议无关组播(Protocol-Independent	 Multicast，PIM)是一组适用于 IP 网络的组播路由协议，通过 LAN、 WAN 或 Internet 提供一对多和多对多的数据分发。它被称为协议无关，因为 PIM 没有包含自己的拓扑发现机制，而是使用其他路由协议提供的路由信息。PIM 不依赖于特定的单播路由协议，它可以使用网络上使用的任何单播路由协议。PIM 不构建自己的路由表。PIM 使用单播路由表进行反向路径转发。
>
> PIMSM(**PIM Sparse Mode**: **稀疏模式**)，

### PIMSM6

### RIP

路由信息协议

### RIPNG

> 路由信息协议的下一代(RIPng)是一个内部网关协议，它使用距离向量算法来确定到达目的地的最佳路径，使用跳数作为度量指标。RIPng 交换用于计算路由的路由信息，用于基于 IP 版本6(IPv6)的网络，是为支持 IPv6而开发的 RIP 扩展。在默认情况下，RIPng 是禁用的。

### Router Manager



