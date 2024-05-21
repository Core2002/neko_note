# Cyber Security

## Hydra 爆破 SSH

```bash
hydra 192.168.56.12 ssh -l user -P passwd.txt -t 6 -V -f
```

```text
命令详细：
攻击目标：192.168.56.12
使用的模块：ssh
爆破用户名：user (-l)
使用的密码字典：/root/Work/sshpasswd.list (-P)
爆破线程数：6 (-t)
显示详细信息 (-V)
爆破成功一个后停止 (-f)
```

> 来自 <https://www.jianshu.com/p/4da49f179cee>

参数详解

| 参数 | 用途 |
| --- | --- |
| -l | 指定单个用户名，适合在知道用户名爆破用户名密码时使用 |
| -L | 指定多个用户名，参数值为存储用户名的文件的路径(建议为绝对路径) |
| -p | 指定单个密码，适合在知道密码爆破用户名时使用 |
| -P | 指定多个密码，参数值为存贮密码的文件(通常称为字典)的路径(建议为绝对路径) |
| -C | 当用户名和密码存储到一个文件时使用此参数。注意，文件(字典)存储的格式必须为 "用户名:密码" 的格式。 |
| -M | 指定多个攻击目标，此参数为存储攻击目标的文件的路径(建议为绝对路径)。注意：列表文件存储格式必须为 "地址:端口" |
| -t | 指定爆破时的任务数量(可以理解为线程数)，默认为16 |
| -s | 指定端口，适用于攻击目标端口非默认的情况。例如：http服务使用非80端口 |
| -S | 指定爆破时使用 SSL 链接 |
| -R | 继续从上一次爆破进度上继续爆破 |
| -v/-V | 显示爆破的详细信息 |
| -f | 一但爆破成功一个就停止爆破 |
| server | 代表要攻击的目标(单个)，多个目标时请使用 `-M` 参数 |
| service | 攻击目标的服务类型(可以理解为爆破时使用的协议)，例如 http ，在hydra中，不同协议会使用不同的模块来爆破，hydra 的 http-get 和 http-post 模块就用来爆破基于 get 和 post 请求的页面 |
| OPT | 爆破模块的额外参数，可以使用 `-U` 参数来查看模块支持那些参数，例如命令：`hydra -U http-get` |

> 来自 <https://www.jianshu.com/p/4da49f179cee>

## WLAN 握手报爆破

```bash
sudo airmon-ng check kill 
sudo airmon-ng start wlan0 
sudo airodump-ng wlan0mon 
sudo airodump-ng -c 1 -w wlan --bssid BA:FE:35:F4:8A:12 wlan0mon 
sudo mdk3 wlan0mon d -s 120 -c 1 
sudo aircrack-ng -w passwd.txt  wlan-01.cap 
```

## 100个网络安全工具

### Nessus：最好的UNIX漏洞扫描工具

> Nessus 是最好的免费网络漏洞扫描器，它可以运行于几乎所有的UNIX平台之上。它不止永久升级，还免费提供多达11000种插件（但需要注册并接受EULA-acceptance–终端用户授权协议）。
> 它的主要功能是远程或本地（已授权的）安全检查，客户端/服务器架构，GTK（Linux下的一种图形界面）图形界面，内置脚本语言编译器，可以用其编写自定义插件，或用来阅读别人写的插件。Nessus 3 已经开发完成（now closed source），其现阶段仍然免费，除非您想获得最新的插件。

### Wireshark：网络嗅探工具

> Wireshark （2006年夏天之前叫做 Ethereal）是一款非常棒的Unix和Windows上的开源网络协议分析器。它可以实时检测网络通讯数据，也可以检测其抓取的网络通讯数据快照文件。可以通过图形界面浏览这些数据，可以查看网络通讯数据包中每一层的详细内容。
> Wireshark拥有许多强大的特性：包含有强显示过滤器语言（rich display filter language）和查看TCP会话重构流的能力；它更支持上百种协议和媒体类型；拥有一个类似tcpdump（一个Linux下的网络协议分析工具）的名为tethereal的的命令行版本。不得不说一句，Ethereal已经饱受许多可远程利用的漏洞折磨，所以请经常对其进行升级，并在不安全网络或敌方网络（例如安全会议的网络）中谨慎使用之。

### Snort：一款广受欢迎的开源IDS（Intrusion Detection System）工具

> 这款小型的漏洞检测和预防系统擅长于通讯分析和IP数据包登录（packet logging）。Snort除了能够进行协议分析、内容搜索和包含其它许多预处理程序，还可以检测上千种蠕虫病毒、漏洞、端口扫描以及其它可疑行为检测。Snort使用一种简单的基于规则的语言来描述网络通讯，以及判断对于网络数据是放行还是拦截，其检测引擎是模块化的。用于分析Snort警报的网页形式的引擎 Basic Analysis and Security Engine (BASE)可免费获得。
> 开源的Snort为个人、小企业、集团用户提供良好的服务。其母公司SourceFire提供丰富的企业级特性和定期升级以丰富其产品线。提供（必须注册）5天免费的规则试用，您也可以在Bleeding Edge Snort找到很多免费规则。

### Netcat：网络瑞士军刀

> 这个简单的小工具可以读和写经过TCP或UDP网络连接的数据。它被设计成一个可靠的可以被其它程序或脚本直接和简单使用的后台工具。同时，它也是一个功能多样的网络调试和检查工具，因为它可以生成几乎所有您想要的网络连接，包括通过端口绑定来接受输入连接。
> Netcat最早由Hobbit在1995年发布，但在其广为流传的情况下并没有得到很好的维护。现在nc110.tgz已经很难找了。这个简单易用的工具促使了很多人写出了很多其它Netcat应用，其中有很多功能都是原版本没有的。其中最有趣的是Socat，它将Netcat扩展成可以支持多种其它socket类型，SSL加密，SOCKS代理，以及其它扩展的更强大的工具。它也在本列表中得到了自己的位置（第71位）。
> 还有Chris Gibson’s Ncat，能够提供更多对便携设备的支持。其它基于Netcat的工具还有OpenBSD’s nc，Cryptcat，Netcat6，PNetcat，SBD，又叫做GNU Netcat。

### Metasploit Framework：黑掉整个星球

> 2004年Metasploit的发布在安全界引发了强烈的地震。没有一款新工具能够一发布就挤进此列表的15强（也就是说2000年和2003年的调查没有这种情况），更何况此工具更在5强之列，超过了很多广为流传的诞生了几十年的老牌工具。它是一个强大的开源平台，供开发、测试和使用恶意代码。这种可扩展的模型将负载控制、编码器、无操作生成器和漏洞整合在一起，使得Metasploit Framework成为一种研究高危漏洞的途径。
> 它自带上百种漏洞，还可以在online exploit building demo（在线漏洞生成演示）看到如何生成漏洞。这使得您自己编写漏洞变得更简单，它势必将提升非法shellcode代码的水平，扩大网络阴暗面。与其相似的专业漏洞工具，例如Core Impact和Canvas已经被许多专业领域用户使用。Metasploit降低了这种能力的门槛，将其推广给大众。

### Hping2：一种网络探测工具，是ping的超级变种

>这个小工具可以发送自定义的ICMP，UDP和TCP数据包，并接收所有反馈信息。它的灵感来源于ping命令，但其功能远远超过ping。它还包含一个小型的路由跟踪模块，并支持IP分段。
>此工具可以在常用工具无法对有防火墙保护的主机进行路由跟踪/ping/探测时大显身手。它经常可以帮助您找出防火墙的规则集，当然还可以通过它来学习TCP/IP协议，并作一些IP协议的实验。

### Kismet：一款超强的无线嗅探器

> Kismet是一款基于命令行（ncurses）的802.11 layer2无线网络探测器、嗅探器、和漏洞检测系统。它对网络进行被动嗅探（相对于许多主动工具，例如NetStumbler），可以发现隐形网络（非信标）。
> 它可以通过嗅探TCP、UDP、ARP和DHCP数据包来自动检测网络IP段，以Wireshark/TCPDump兼容格式记录通讯日志，更加可以将被检测到的网络分块并按照下载的分布图进行范围估计。如您所想，这款工具一般被wardriving所使用。嗯！还有warwalking、warflying和warskating……

### Tcpdump：最经典的网络监控和数据捕获嗅探器

> 在Ethereal（Wireshark）出现之前大家都用Tcpdump，而且很多人现在还在一直使用。它也许没有Wireshark那么多花里胡哨的东西（比如漂亮的图形界面，亦或数以百计的应用协议逻辑分析），但它能出色的完成很多任务，并且漏洞非常少，消耗系统资源也非常少。
> 它很少添加新特性了，但经常修复一些bug和维持较小的体积。它能很好的跟踪网络问题来源，并能监控网络活动。其Windows下的版本叫做WinDump。Libpcap/WinPcap的包捕获库就是基于TCPDump，它也用在Nmap等其它工具中。

### Cain and Abel：Windows平台上最好的密码恢复工具

> UNIX用户经常声称正是因为Unix平台下有很多非常好的免费安全工具，所以Unix才会成为最好的平台，而Windows平台一般不在他们的考虑范围之内。他们也许是对的，但Cain & Abel确实让人眼前一亮。这种只运行于Windows平台的密码恢复工具可以作很多事情。
> 它可以通过嗅探网络来找到密码、利用字典破解加密密码、暴力破解密码和密码分析、记录VoIP会话、解码非常复杂的密码、星号查看、剥离缓存密码以及分析路由协议。另外其文档也很齐全（well documented）。

### John the Ripper：一款强大的、简单的以及支持多平台的密码破解器

> John the Ripper是最快的密码破解器，当前支持多种主流Unix （官方支持11种，没有计算不同的架构）、DOS、Win BeO和OpenVMS。它的主要功能就是检测弱Unix密码。
> 它支持主流Unix下的多种（3种）密码哈希加密类型，它们是Kerberos、AFS以及Windows NT/2000/XP LM。其它哈希类型可以通过补丁包加载。如果您希望从一些单词表开始的话，您可以在这里、这里和这里找到。

### Ettercap：为交换式局域网提供更多保护

> Ettercap是一款基于终端的以太网络局域网嗅探器/拦截器/日志器。它支持主动和被动的多种协议解析（甚至是ssh和https这种加密过的）。
> 还可以进行已建立连接的数据注入和实时过滤，保持连接同步。大部分嗅探模式都是强大且全面的嗅探组合。支持插件。能够识别您是否出在交换式局域网中，通过使用操作系统指纹（主动或被动）技术可以得出局域网结构。

### Nikto：非常全面的网页扫描器

> Nikto是一款开源的（GPL）网页服务器扫描器，它可以对网页服务器进行全面的多种扫描，包含超过3200种有潜在危险的文件/CGIs；超过625种服务器版本；超过230种特定服务器问题。扫描项和插件可以自动更新（如果需要）。基于Whisker/libwhisker完成其底层功能。这是一款非常棒的工具，但其软件本身并不经常更新，最新和最危险的可能检测不到。

### Ping/telnet/dig/traceroute/whois/netstat：基本命令

> 虽然有很多重型的高科技网络安全工具，但是不要忘记其基础！所有网络安全人士都要对这些基本命令非常熟悉，因为它们对大多数平台都适用（在Windows平台上whois为tracert）。
> 它们可以随手捏来，当然如果需要使用一些更高级的功能可以选择Hping2和Netcat。

### OpenSSH / PuTTY / SSH：访问远程计算机的安全途径

> SSH（Secure Shell）现在普遍应用于登录远程计算机或在其上执行命令。它为不安全网络上的两台不互信计算机间通讯提供安全加密，代替非常不可靠的telnet/rlogin/rsh交互内容。大多UNIX使用开源的OpenSSH服务器和客户端程序。Windows用户更喜欢免费的PuTTY客户端，它也可以运行在多种移动设备上。还有一些Windows用户喜欢使用基于终端的OpenSSH模拟程序Cygwin。还有其它很多收费和免费的客户端。您可以在这里和这里找到。

### THC Hydra：支持多种服务的最快的网络认证破解器

> 如果您需要暴力破解一个远程认证服务，Hydra经常会是选择对象。它可以同时对30个以上的端口进行基于字典的快速破解，包括telnet、ftp、http、https、smb、多种数据库及其它服务。和THC Amap一样，此Hydra版本来自于民间组织THC。

### Paros proxy：网页程序漏洞评估代理

> 基于Java的网页程序漏洞评估代理。支持实时编辑和浏览HTTP/HTTPS信息，修改例如Cookie和表字段中的内容。它包含有网页通讯记录器、网页小偷（web spider）、哈希计算器和一个常用网页程序攻击扫描器，例如SQL注入和跨网站脚本等。

### Dsniff：一款超强的网络评估和渗透检测工具套装

> 由Dug Song精心设计并广受欢迎的这款套装包含很多工具。Dsniff、filesnarf、mailsnarf、msgsnarf、urlsnarf和webspy通过被动监视网络以获得敏感数据（例如密码、邮件地址、文件等）。Arpspoof、dnsspoof和macof能够拦截一般很难获取到的网络通讯信息（例如由于使用了第二层转换（layer-2 switching））。
> Sshmitm和webmitm通过ad-hoc PKI中弱绑定漏洞对ssh和https会话进行重定向实施动态monkey-in-the-middle（利用中间人攻击技术，对会话进行劫持）攻击。Windows版本可以在这里获取。总之，这是一个非常有用的工具集。它能完成几乎所有密码嗅探需要作的工作。

### NetStumbler：免费的Windows 802.11嗅探器

> Netstumbler是广为人知的寻找开放无线访问接入点的Windows工具（”wardriving”）。其PDA上的WinCE系统版本名叫Ministumbler。此软件当前免费，但只能够运行在Windows平台上，且代码不公开。它使用很多主动方法寻找WAP，而Kismet或KisMAC则更多使用被动嗅探。

### THC Amap：一款应用程序指纹扫描器

> Amap是一款很棒的程序，它可以检测出某一端口正在被什么程序监听。因为其独有的version detection特性，所以其数据库不会象Nmap一样变得很大，在Nmap检测某一服务失败或者其它软件不起作用时可以考虑使用之。Amap的另一特性是其能够解析Nmap输出文件。这也是THC贡献的另一款很有价值的工具。

### GFI LANguard：一款Windows平台上的商业网络安全扫描器

> GFI LANguard通过对IP网络进行扫描来发现运行中的计算机，然后尝试收集主机上运行的操作系统版本和正在运行的应用程序。我曾经尝试收集到了Windows主机上的service pack级别、缺少的安全更新、无线访问接入点、USB设备、开放的共享、开放的端口、正在运行的服务和应用程序、主要注册表项、弱密码、用户和组别以及其它更多信息。扫描结果保存在一份可自定义/可查询的HTML报告文档中。它还含有一个补丁管理器，可以检查并安装缺少的补丁。试用版可以免费获得，但只能使用30天。

### Aircrack：最快的WEP/WPA破解工具

> Aircrack是一套用于破解802.11a/b/g WEP和WPA的工具套装。一旦收集到足够的加密数据包它可以破解40到512位的WEP密匙，它也可以通过高级加密方法或暴力破解来破解WPA 1或2网络。套装中包含airodump（802.11数据包捕获程序）、aireplay （802.11数据包注入程序）、aircrack（静态WEP和WPA-PSK破解），和airdecap（解密WEP/WPA捕获文件）。

### Superscan：只运行于Windows平台之上的端口扫描器、ping工具和解析器

> SuperScan是一款Foundstone开发的免费的只运行于Windows平台之上的不开源的TCP/UDP端口扫描器。它其中还包含许多其它网络工具，例如ping、路由跟踪、http head和whois。

### Netfilter：最新的Linux核心数据包过滤器/防火墙

> Netfilter是一款强大的运行于标准Linux核心上的包过滤器。它集成了用户空间IP列表工具。当前，它支持包过滤（无状态或有状态）、所有类型的网络地址和端口转换（NAT/NAPT）并支持多API层第三方扩展。它包含多种不同模块用来处理不规则协议，例如FTP。其它UNIX平台请参考Openbsd PF（只用于OpenBSD）或者IP Filter。许多个人防火墙（personal firewalls）都支持Windows （Tiny、Zone Alarm、Norton、Kerio…），但都不提供上述IP列表。微软在Windows XP SP2中集成了一款非常基础的防火墙，如果您不安装它，它就会不断地提示您安装。

### Sysinternals：一款强大的非常全面的Windows工具合集

> Sysinternals为Windows低级漏洞提供很多非常有用的小工具。其中一部分是免费的，有些还附有源代码，其它是需要付费使用的。受访者最喜欢此集合中的以下工具：

- ProcessExplorer 监视所有进程打开的所有文件和目录（类似Unix上的LSoF）。  
- PsTools 管理（执行、挂起、杀死、查看）本地和远程进程。  
- Autoruns 发现系统启动和登陆时加载了哪些可执行程序。  
- RootkitRevealer 检测注册表和文件系统API异常，用以发现用户模式或内核模式的rootkit工具。  
- TCPView 浏览每个进程的TCP和UDP通讯终点（类似Unix上的Netstat）。  
- 生产此软件的公司已被微软于2005年收购，所以其未来产品线特征无法预测。  

### Retina：eEye出品的商业漏洞评估扫描器

> 象Nessus一样，Retina的功能是扫描网络中所有的主机并报告发现的所有漏洞。eEye出品，此公司以其security research而闻名。

### Perl / Python / Ruby：简单的、多用途的脚本语言

> 常用的安全问题都能在网上找到工具解决，但使用脚本语言您可以编写您自己的（或编辑现有的）工具，当您需要解决某种特定问题的时候。快速、简单的脚本语言可以测试、发现漏洞甚至修复系统漏洞。CPAN上充满了类似Net：：RawIP和执行协议的程序模块，可以使您的工作更加轻松。

### L0phtcrack：Windows密码猜测和恢复程序

> L0phtCrack也叫作LC5，用来尝试通过哈希（通过某种访问方式获得的）方法破解诸如Windows NT/2000工作站、联网服务器、主域控制器、或活动目录密码，有时它也可以通过嗅探获得密码的哈希值。它还可以通过多种手段来猜测密码（字典、暴力破解等等）。
> Symantec公司2006年已经停止了LC5的开发，但LC5 installer的安装文件随处可以找到。免费试用版只能使用15天，Symantec已经停止出售此软件的注册码，所以如果您不想放弃使用它，就必须找到一个与其对应的注册码生成器（key generator）。因为Symantec不再维护此软件，所以最好尝试用Cain and Abel或John the Ripper来代替之。

### Scapy：交互式数据包处理工具

> Scapy是一款强大的交互式数据包处理工具、数据包生成器、网络扫描器、网络发现工具和包嗅探工具。它提供多种类别的交互式生成数据包或数据包集合、对数据包进行操作、发送数据包、包嗅探、应答和反馈匹配等等功能。Python解释器提供交互功能，所以要用到Python编程知识（例如variables、loops、和functions）。支持生成报告，且报告生成简单。

### Sam Spade：Windows网络查询免费工具

> Sam Spade为许多网络查询的一般工作提供了图形界面和方便的操作。此工具设计用于跟踪垃圾信息发送者，但它还可以用于许多其它的网络探查、管理和安全工作。它包含许多有用的工具，例如ping、nslookup、whois、dig、路由跟踪、查找器、原始HTTP网页浏览器、DNS地址转换、SMTP中继检查、网站搜索等等。非Windows用户可以在线使用更多其它工具。

### GnuPG / PGP ：对您的文件和通讯进行高级加密

> PGP是Phil Zimmerman出品的著名加密程序，可以使您的数据免受窃听以及其它危险。GnuPG是一款口碑很好的遵守PGP标准的开源应用（可执行程序名为gpg）。GunPG是免费的，而PGP对某些用户是收费的。

### Airsnort：802.11 WEP加密破解工具

> AirSnort是一款用来恢复加密密码的无线LAN（WLAN）工具。Shmoo Group出品，工作原理是被动监控传输信息，当收集到足够多的数据包后开始计算加密密码。Aircrack和它很像。

### BackTrack：一款极具创新突破的Live（刻在光盘上的，光盘直接启动） 光盘自启动Linux系统平台

> 这款卓越的光盘自启动Linux系统是由Whax和Auditor合并而成。它以其超级多的安全和防护工具配以丰富的开发环境而闻名。重点在于它的用户模块化设计，用户可以自定义将哪些模块刻到光盘上，例如自己编写的脚本、附加工具、自定义内核等等。

### P0f：万能的被动操作系统指纹工具

> P0f能够通过捕获并分析目标主机发出的数据包来对主机上的操作系统进行鉴别，即使是在系统上装有性能良好的防火墙的情况下也没有问题。P0f不增加任何直接或间接的网络负载，没有名称搜索、没有秘密探测、没有ARIN查询，什么都没有。某些高手还可以用P0f检测出主机上是否有防火墙存在、是否有NAT、是否存在负载平衡器等等！

### Google：人人喜爱的搜索引擎

> Google当然不是什么安全工具，但是它超级庞大的数据库却是安全专家和漏洞者最好的资源。如果您想了解某一公司，您可以直接用它搜索 “site：target-domain.com”，您可以获得员工姓名、敏感信息（通常公司不对外公开的，但在Google上就难说了）、公司内部安装的软件漏洞等等。同样，如果您在Google上发现一个有某个漏洞的网站，Google还会提供给您其它有相同漏洞的网站列表。
> 其中利用Google进行黑客活动的大师Johny Long建立了一个Google黑客数据库（Google Hacking Database）还出版了一本如何用Google进行黑客活动的书Google Hacking for Penetration Testers。

### WebScarab：一个用来分析使用HTTP和HTTPS协议的应用程序框架

> 它的原理很简单，WebScarab记录它检测到的会话内容（请求和应答），使用者可以通过多种形式来查看记录。WebScarab的设计目的是让使用者可以掌握某种基于HTTP（S）程序的运作过程；也可以用它来调试程序中较难处理的bug，也可以帮助安全专家发现潜在的程序漏洞。

### Ntop：网络通讯监控器

> Ntop以类似进程管理器的方式显示网络使用情况。在应用程序模式下，它能显示用户终端上的网络状况。在网页模式下，它作为网页服务器，以HTML文档形式显示网络状况。它是NetFlow/sFlow发射和收集器，通过一个基于HTTP的客户端界面来生成以ntop为中心的监控程序，RRD（Round Robin Database）（环形数据库）用来持续储存网络通讯状态信息。

### Tripwire：很老的文件完整性检查器

> 一款文件和目录完整性检查器。Tripwire是一种可以帮助系统管理员和一般用户监控某一特定文件或目录变化的工具。可以用以对系统文件作日常（例如：每天）检查，Tripwire可以向系统管理员通报文件损坏或被篡改情况，所以这是一种周期性的文件破坏控制方法。
> 免费的开源Linux版本可以在Tripwire.Org下载到。AIDE是UNIX平台的Tripwire替代品。或者Radmind、RKHunter和chkrootkit也是很好的选择。Windows用户请使用Sysinternals出品的RootkitRevealer。

### Ngrep：方便的数据包匹配和显示工具

> ngrep尽可能多的去实现GNU grep的功能，将它们应用于网络层。Ngrep是一款pcap-aware工具，它允许指定各种规则式或16进制表达式去对数据负载或数据包进行匹配。当前支持TCP、UDP、以太网上的ICMP、PPP、SLIP、FDDI、令牌环（Token Ring）和空接口（null interfaces），还能理解类似Tcpdump和snoop等一样形式的bpf过滤器逻辑。

### Nbtscan：在Windows网络上收集NetBIOS信息

> NBTscan是一款在IP网络上扫描NetBIOS名称信息的工具。它通过给指定范围内所有地址发送状态查询来获得反馈信息并以表形式呈现给使用者。每一地址的反馈信息包括IP地址、NetBIOS计算机名、登录用户、MAC地址。

### WebInspect：强大的网页程序扫描器

> SPI Dynamics’ WebInspect应用程序安全评估工具帮您识别已知和未知的网页层漏洞。它还能检测到Web服务器的配置属性，以及进行常见的网页攻击，例如参数注入、跨网站脚本、目录游走等等。

### OpenSSL：最好的SSL/TLS加密库

> OpenSSL项目的目的是通过开源合作精神开发一种健壮的、可以和同类型商业程序媲美的、全功能的，且开源的应用于SSL v2/v3（Secure Sockets Layer）和TLS v1（Transport Layer Security）协议的普遍适用的加密库工具集。本项目由世界范围内的志愿者们维护，他们通过互联网联络、计划和开发OpenSSL工具集及其相关文档。

### Xprobe2：主动操作系统指纹工具

> XProbe是一款远程主机操作系统探查工具。开发者基于和Nmap相同的一些技术（same techniques），并加入了自己的创新。Xprobe通过ICMP协议来获得指纹。

### EtherApe：EtherApe是Unix平台上的模仿etherman的图形界面网络监控器

> 包含连接层、IP和TCP三种模式，EtherApe网络活动图通过不同颜色来标识不同协议。主机和连接的图形大小随通讯情况而变化。它支持以太网、FDDI、令牌环、ISDN、PPP和SLIP设备。它可以实施过滤网络通讯，也可以抓取网络通讯快照文件。

### Core Impact：全自动的全面漏洞检测工具

> Core Impact可不便宜（先准备个上万美元吧），但它却是公认的最强的漏洞检测工具。它有一个强大的定时更新的专业漏洞数据库，它可以轻易的黑掉一台计算机，并以它为跳板再去作别的事情。如果您买不起Core Impact，可以看看比较便宜的Canvas或者免费的Metasploit Framework。当然，三个同时用是最好的了。

### IDA Pro：Windows或Linux反编译器和调试器

> 反编译器是一块很重要的安全研究方向。它可以帮您拆解微软的补丁，以了解微软未公开并悄悄修补的漏洞，或直接以二进制的方式对某个服务器进行检测，以找出为何某个存在的漏洞不起作用。反编译器有很多，但IDA Pro是遵守二进制包事实标准（de-facto standard）的恶意代码和漏洞研究分析工具。这个图形化的、可编程的、可扩展的、支持多处理器的反编译器现在有了一个和Windows一模一样的Linux（命令行模式）版本。

### SolarWinds：网络发现/监控/攻击系列工具

> SolarWinds生产和销售了许多专业的系统管理工具。安全相关的包括许多网络发现扫描器、一个SNMP暴力破解器、路由器密码解密器、TCP连接重置程序、最快最易用的一个路由器设置下载和上传程序等等。

### Pwdump：一款Windows密码恢复工具

> Pwdump可以从Windows主机中取得NTLM和LanMan哈希值，无论系统密码是否启用。它还能显示系统中存在的历史密码。数据输出格式为L0phtcrack兼容格式，也可以以文件形式输出数据。

### LSoF：打开文件列表

> 这是一款Unix平台上的诊断和研究工具，它可以列举当前所有进程打开的文件信息。它也可以列举所有进程打开的通讯socket（communications sockets）。Windows平台上类似的工具有Sysinternals。

### RainbowCrack：极具创新性的密码哈希破解器

> RainbowCrack是一款使用了大规模内存时间交换（large-scale time-memory trade-off）技术的哈希破解工具。传统的暴力破解工具会尝试每一个可能的密码，要破解复杂的密码会很费时。RainbowCrack运用时间交换技术对破解时间进行预计算，并将计算结果存入一个名叫”rainbow tables”的表里。预计算确实也会花费较长时间，但相对暴力破解来说则短多了，而且一旦预计算完成破解开始，那么破解所需要的时间就非常非常短了。

### Firewalk：高级路由跟踪工具

> Firewalk使用类似路由跟踪的技术来分析IP数据包反馈，以确定网关ACL过滤器类型和网络结构。这款经典的工具在2002年十月由scratch重写。这款工具的大部分功能Hping2的路由跟踪部分也都能实现

### Angry IP Scanner：一款非常快的Windows IP 扫描器和端口扫描器

> Angry IP Scanner能够实现最基本的Windows平台上的主机发现和端口扫描。它的体积非常的小，它还可以通过挂载插件（a few plugins）来获得主机其它信息。

### RKHunter：一款Unix平台上的Rootkit检测器

> RKHunter是一款检测例如rootkit、后门、漏洞等恶意程序的工具。它采用多种检测手段，包括MD5哈希值对比、rootkits原始文件名检测、文件权限检测，以及LKM和KLD模块中的可疑字符串检测。

### Ike-scan：VPN检测器和扫描器

> Ike-scan是一款检测IKE（Internet Key Exchange）服务传输特性的工具，IKE是VPN网络中服务器和远程客户端建立连接的机制。在扫描到VPN服务器的IP地址后，将改造过的IKE数据包分发给VPN网中的每一主机。只要是运行IKE的主机就会发回反馈来证明它存在。此工具然后对这些反馈数据包进行记录和显示，并将它们与一系列已知的VPN产品指纹进行对比。Ike-scan的VPN指纹包含来自Checkpoint、Cisco、Microsoft、Nortel和Watchguard的产品。

### Arpwatch：持续跟踪以太网/IP地址配对，可以检查出中间人攻击

> Arpwatch是LBNL网络研究组出品的一款经典的ARP中间人（man-in-the-middle）攻击检测器。它记录网路活动的系统日志，并将特定的变更通过Email报告给管理员。Arpwatch使用LibPcap来监听本地以太网接口ARP数据包。

### KisMAC：一款Mac OS X上的图形化被动无线网络搜寻器

> 这款Mac OS X下非常流行的搜寻器和Kismet功能差不多，但和Kismet不同的是Kismet是基于命令行的，而KisMac有很漂亮的图形化界面，在OS X上出现得也比Kismet早。它同时还提供映射、Pcap兼容格式数据输入、登录和一些解密、验证破解功能。

### OSSEC HIDS：一款开源的基于主机的漏洞检测系统

> OSSEC HIDS的主要功能有日志分析、完整性检查、rootkit检测、基于时间的警报和主动响应。除了具有检测系统功能外，它还一般被用在SEM/SIM（安全事件管理（SEM：Security Event Management）/安全信息管理（SIM：Security Information Management））解决方案中。因其强大的日志分析引擎，ISP（Internet service provider）（网络服务提供商）、大学和数据中心用其监控和分析他们的防火墙、检测系统、网页服务和验证等产生的日志。

### Openbsd PF：OpenBSD数据包过滤器

> 象其它平台上的Netfilter和IP Filter一样，OpenBSD用户最爱用PF，这就是他们的防火墙工具。它的功能有网络地址转换、管理TCP/IP通讯、提供带宽控制和数据包分级控制。它还有一些额外的功能，例如被动操作系统检测。PF是由编写OpenBSD的同一批人编写的，所以您完全可以放心使用，它已经经过了很好的评估、设计和编码以避免暴露其它包过滤器（other packet filters）上的类似漏洞。

### Nemesis：简单的数据包注入

> Nemesis项目设计目的是为Unix/Linux（现在也包含Windows了）提供一个基于命令行的、小巧的、人性化的IP堆栈。此工具套装按协议分类，并允许对已注入的数据包流使用简单的shell脚本。如果您喜欢Nemesis，您也许对Hping2也会感兴趣，它们是互补的关系。

### Tor：匿名网络通讯系统

> Tor是一款面向希望提高其网络安全性的广大组织和大众的工具集。Tor的功能有匿名网页浏览和发布、即时信息、irc、ssh以及其它一些TCP协议相关的功能。Tor还为软件开发者提供一个可开发内置匿名性、安全性和其它私密化特性的软件平台。在Vidalia可以获得跨平台的图形化界面。

### Knoppix：一款多用途的CD或DVD光盘自启动系统

> Knoppix由一系列典型的GNU/Linux软件组成，可以自动检测硬件环境，支持多种显卡、声卡、SCSI和USB设备以及其它外围设备。KNOPPIX作为一款高效的Linux光盘系统，可以胜任例如桌面系统、Linux教学光盘、救援系统等多种用途，经过这次在nmap中调查证实，它也是一款很小巧的安全工具。如果要使用更专业的Linux安全系统请看BackTrack。

### ISS Internet Scanner：应用程序漏洞扫描器

> Internet Scanner是由Christopher Klaus在92年编写的一款开源的扫描器工具。现在这款工具已经演变成了一个市值上亿美元生产无数安全产品的公司。

### Fport：Foundstone出品的加强版netstat

> Fport可以报告所有本地机上打开的TCP/IP和UDP端口，并显示是何程序打开的端口。所以用它可以快速识别出未知的开放端口以及与其相关的应用程序。它只有Windows版本，但现在很多UNIX系统上的netstat也提供同样的功能（Linux请用’netstat -pan’）。SANS article有Fport的使用说明和结果分析方法。

### chkrootkit：本地rootkit检测器

> chkrootkit是一款小巧易用的Unix平台上的可以检测多种rootkit漏洞的工具。它的功能包括检测文件修改、utmp/wtmp/last日志修改、界面欺骗（promiscuous interfaces）、恶意核心模块（malicious kernel modules）。

### SPIKE Proxy：HTTP攻击

> Spike Proxy是一款开源的以发现网站漏洞为目的的HTTP代理。它是Spike Application Testing Suite的一部分，功能包括自动SQL注入检测、 网站爬行（web site crawling）、登录列表暴力破解、溢出检测和目录游走检测。

### OpenBSD：被认为是最安全的操作系统

> OpenBSD是将安全作为操作系统首要任务的操作系统之一，甚至有时安全性级别要高于易用性，所以它骄人的安全性是不言而喻的。OpenBSD也非常重视系统的稳定性和对硬件的支持能力。也许他们最伟大的创举就是创造了OpenSSH。OpenBSD用户对此系统之上的[pf]（OpenBSD上的防火墙工具，本列表中第57位有介绍）也褒奖有佳。

### Yersinia：一款支持多协议的底层攻击工具

> Yersinia是一款底层协议攻击漏洞检测工具。它能实施针对多种协议的多种攻击。例如夺取生成树的根角色（生成树协议：Spanning Tree Protocol），生成虚拟CDP（Cisco发现协议：Cisco Discovery Protocol）邻居、在一个HSRP（热等待路由协议：Hot Standby Router Protocol）环境中虚拟成一个活动的路由器、制造假DHCP反馈，以及其它底层攻击。

### Nagios：一款开源的主机、服务和网络监控程序

> Nagios是一款系统和网络监控程序。它可以监视您指定的主机和服务，当被监视对象发生任何问题或问题被解决时发出提示信息。它的主要功能有监控网络服务（smtp、pop3、http、nntp、ping等等）、监控主机资源（进程负载、硬盘空间使用情况等等）、当发现问题或问题解决时通过多种形式发出提示信息（Email、寻呼机或其它用户定义的方式）。

### Fragroute/Fragrouter：一款网络漏洞检测逃避工具集

> Fragrouter 是一款单向分段路由器，发送（接收）IP数据包都是从攻击者到Fragrouter，将数据包转换成分段数据流发给受害者。很多检测系统都不能重建一段被视为一个整体的网络数据（通过IP分段和TCP流重组），详情请见这篇文章（this classic paper）。Fragrouter可以帮助骇客在逃避检测后发起基于IP的攻击。它是Dug Song出品的NIDSbench套装中的一部分。Fragroute是Dug song出品的另一款和Fragrouter相似的工具。

### X-scan：一款网络漏洞扫描器

> 一款多线程、支持插件的漏洞扫描器。X-Scan主要功能有全面支持NASL（Nessus攻击脚本语言：Nessus Attack Scripting Language）、检测服务类型、远程操作系统类型（版本）检测、弱用户名/密码匹配等等。最新版本可以在这里获取。请注意这是一个中文网站（原文为英文，所以原文作者提醒英文读者这是个中文网站）。

### Whisker/libwhisker：Rain.Forest.Puppy出品的CGI漏洞扫描器和漏洞库

> Libwhisker是一款Perl模板集用来测试HTTP。它的功能是测试HTTP服务器上是否存在许多已知的安全漏洞，特别是CGI漏洞。Whisker是一款基于libwhisker的扫描器，但是现在大家都趋向于使用Nikto，它也是基于libwhisker的。

### Socat：双向数据传输中继

> 类似于Netcat的工具，可以工作于许多协议之上，运行于文件、管道、设备（终端或调制解调器等等）、socket（Unix、IP4、IP6-raw、UDP、TCP）、Socks4客户端、代理服务器连接、或者SSL等等之间。它提供forking、logging和dumping，和不同模式的交互式处理通讯，以及更多其它选项。它可以作为TCP中继（单次触发：one-shot或者daemon（Internet中用于邮件收发的后台程序））、作为基于daemon的动态Sockes化（socksifier）、作为Unix平台上sockets的shell接口、作为IP6中继、将面向TCP的程序重定向成串行线路（Serial Line）程序、或者建立用来运行客户端或服务器带有网络连接的shell脚本相关安全环境（su和chroot）。

### Sara：安全评审研究助手

> SARA是一款源于infamous SATAN扫描器的漏洞评估工具。此工具大约两个月更新一次，出品此工具的开源社区还维护着Nmap和Samba。

### QualysGuard：基于网页的漏洞扫描器

> 在网站上以服务形式发布， 所以QualysGuard没有开发、维护和升级漏洞管理软件或ad-hoc安全应用程序的负担。客户端可以安全的通过一个简单易用的网页访问QualysGuard。QualysGuard含有5000种以上的单一漏洞检查，一个基于推理的扫描引擎，而且漏洞知识库自动天天升级。

### ClamAV：一款UNIX平台上的基于GPL（通用公开许可证：General Public License）的反病毒工具集

> ClamAV是一款强大的注重邮件服务器附件扫描的反病毒扫描器。它含有一个小巧的可升级的多线程daemon、一个命令行扫描器和自动升级工具。Clam AntiVirus基于AntiVirus package发布的开源病毒库，您也可以将此病毒库应用于您自己的软件中，但是别忘了经常升级。

### cheops / cheops-ng：提供许多简单的网络工具，例如本地或远程网络映射和识别计算机操作系统

> Cheops提供许多好用的图形化用户界面网络工具。它含有主机/网络发现功能，也就是主机操作系统检测。Cheops-ng用来探查主机上运行的服务。针对某些服务，cheops-ng可以探查到运行服务的应用程序是什么，以及程序的版本号。Cheops已经停止开发和维护，所以请最好使用cheops-ng。

### Burpsuite：一款网页程序攻击集成平台

> Burp suite允许攻击者结合手工和自动技术去枚举、分析、攻击网页程序。这些不同的burp工具通过协同工作，有效的分享信息，支持以某种工具中的信息为基础供另一种工具使用从而发动攻击。

### Brutus：一款网络验证暴力破解器

> 这款Windows平台上的暴力破解器通过字典猜测远程系统网络服务密码。它支持HTTP、POP3、FTP、SMB、TELNET、IMAP、NTP等等。不开放源码，UNIX平台上的类似软件有THC Hydra。

### Unicornscan：另类端口扫描器

> Unicornscan是一款通过尝试连接用户系统（User-land）分布式TCP/IP堆栈获得信息和关联关系的端口扫描器。它试图为研究人员提供一种可以刺激TCP/IP设备和网络并度量反馈的超级接口。它主要功能包括带有所有TCP变种标记的异步无状态TCP扫描、异步无状态TCP标志捕获、通过分析反馈信息获取主动/被动远程操作系统、应用程序、组件信息。它和Scanrand一样都是另类扫描器。

### Stunnel：用途广泛的SSL加密封装器

> stunnel用来对远程客户端和本地机（可启动inetd的：inetd-startable）或远程服务器间的SSL加密进行封装。它可以在不修改任何代码的情况下，为一般的使用inetd daemon的POP2、POP3和IMAP服务器添加SSL功能。它通过使用OpenSSL或SSLeay库建立SSL连接。

### Honeyd：您私人的蜜罐系统

> Honeyd是一个可以在网络上创建虚拟主机的小型daemon。可以对此虚拟主机的服务和TCP进行配置，使其在网络中看起来是在运行某种操作系统。Honeyd可以使一台主机在局域网中模拟出多个地址以满足网络实验环境的要求。虚拟主机可以被ping通，也可以对它们进行路由跟踪。通过对配置文件进行设置可以使虚拟计算机模拟运行任何服务。也可以使用服务代理替代服务模拟。它的库有很多，所以编译和安装Honeyd比较难。

### Fping：一个多主机同时ping扫描程序

> fping是一款类似ping（1）（ping（1）是通过ICMP（网络控制信息协议Internet Control Message Protocol）协议回复请求以检测主机是否存在）的程序。Fping与ping不同的地方在于，您可以在命令行中指定要ping的主机数量范围，也可以指定含有要ping的主机列表文件。与ping要等待某一主机连接超时或发回反馈信息不同，fping给一个主机发送完数据包后，马上给下一个主机发送数据包，实现多主机同时ping。如果某一主机ping通，则此主机将被打上标记，并从等待列表中移除，如果没ping通，说明主机无法到达，主机仍然留在等待列表中，等待后续操作。

### BASE：基础分析和安全引擎（Basic Analysis and Security Engine）

> BASE是一款基于PHP的可以搜索和实施安全事件的分析引擎，她的安全事件数据库来源于很多漏洞检测系统、防火墙、网络检测工具生成的安全事件。它的功能包括一个查找生成器和搜索界面，用来搜索漏洞；一个数据包浏览器（解码器）；还可以根据时间、传感器、信号、协议和IP地址等生成状态图。

### Argus：IP网络事务评审工具

> Argus是一款固定模型的实时的流量监视器，用来跟踪和报告数据网络通讯流中所有事务的状态和性能。Argus为流量评估定制了一种数据格式，其中包括连通性、容量、请求、丢包、延迟和波动，这些就作为评估事务的元素。这种数据格式灵活易扩展，支持常用流量标识和度量，还可以获得指定的应用程序/协议的信息。

### Wikto：网页服务器评估工具

> Wikto是一款检查网页服务器漏洞的工具。它和Nikto类似，但是添加了很多其它功能，例如一个整合了Google的后台发掘器。Wikto工作于MS …NET环境下，下载此软件和源代码需要注册。

### Sguil：网络安全监控器命令行分析器

> Sguil（按sgweel发音）是由network security analysts出品的网络安全分析工具。Sguil的主要组件就是一个Snort/barnyard实时事件显示界面。它还包含一些网络安全监控的辅助工具和事件驱动的漏洞检测系统分析报告。

### Scanrand：一个异常快速的无状态网络服务和拓朴结构发现系统

> Scanrand是一款类似Unicornscan的无状态主机发现和端口扫描工具。它以降低可靠性来换取异常快的速度，还使用了加密技术防止黑客修改扫描结果。此工具是Dan Kaminsky出品的Paketto Keiretsu的一部分。

### IP Filter：小巧的UNIX数据包过滤器

> IP Filter是一款软件包，可以实现网络地址转换（network address translation）（NAT）或者防火墙服务的功能。它可以作为UNIX的一个核心模块，也可以不嵌入核心，强烈推荐将其作为UNIX的核心模块。安装和为系统文件打补丁要使用脚本。IP Filter内置于FreeBSD、NetBSD和Solaris中。OpenBSD可以使用Openbsd PF，Linux用户可以使用Netfilter。

### Canvas：一款全面的漏洞检测框架

> Canvas是Aitel’s ImmunitySec出品的一款漏洞检测工具。它包含150个以上的漏洞，它比Core Impact便宜一些，但是它也价值数千美元。您也可以通过购买VisualSploit Plugin实现在图形界面上通过拖拽就可以生成漏洞。Canvas偶尔也会发现一些ODay漏洞。

### VMware：多平台虚拟软件

> VMware虚拟软件允许您在一个系统中虚拟运行另一个系统。这对于安全专家在多平台下测试代码和漏洞非常有用。它只运行在Windows和Linux平台上，但它可以虚拟运行几乎所有的x86操作系统。它对建立沙箱（sandboxes）也非常有用。在VMware虚拟系统上感染了恶意软件不会影响到宿主机器，可以通过加载快照文件恢复被感染了的虚拟系统。VMware不能创建虚拟系统的镜像文件。VMware最近刚刚宣布免费。另一款在Linux下颇受瞩目的虚拟平台软件是Xen。

### Tcptraceroute：一款基于TCP数据包的路由跟踪工具

> 现代网络广泛使用防火墙，导致传统路由跟踪工具发出的（ICMP应答（ICMP echo）或UDP）数据包都被过滤掉了，所以无法进行完整的路由跟踪。尽管如此，许多情况下，防火墙会准许反向（inbound）TCP数据包通过防火墙到达指定端口，这些端口是主机内防火墙背后的一些程序和外界连接用的。通过发送TCP SYN数据包来代替UDP或者ICMP应答数据包，tcptraceroute可以穿透大多数防火墙。

### SAINT：安全管理综合网络工具

> SAINT象Nessus、ISS Internet Scanner和Retina一样，也是一款商业漏洞评估工具。它以前是运行在UNIX系统之上的免费开源工具，但现在收费了。

### OpenVPN：全功能SSL VPN解决方案

> OpenVPN是一款开源的SSL VPN工具包，它可以实现很多功能，包括远程登录、站对站VPN、WiFi安全、带有负载平衡的企业级远程登录解决方案、节点控制移交（failover）、严密的访问控制。OpenVPN运行于OSI 2层或3层安全网络，使用SSL/TLS工业标准协议，支持灵活的基于证书、智能卡、二元验证的客户端验证方法，允许在VPN虚拟接口上使用防火墙规则作为用户或指定用户组的访问控制策略。OpenVPN使用OpenSSL作为其首选加密库

### OllyDbg：汇编级Windows调试器

> OllyDbg是一款微软Windows平台上的32位汇编级的分析调试器。因其直接对二进制代码进行分析，所以在无法获得源代码的时候它非常有用。OllyDbg含有一个图形用户界面，它的高级代码分析器可以识别过程、循环、API调用、交换、表、常量和字符串，它可以加载运行时程序，支持多线程。OllyDbg可以免费下载，但不开源。

### Helix：一款注重安全防护的Linux版本

> Helix是一款自定义版本的Knoppix自启动Linux光盘系统。Helix远不止是一张自启动光盘。除了光盘启动到自定义的Linux环境，还具有超强的硬件支持能力，包含许多应付各种问题的软件。Helix尽量少的接触主机软硬资源。Helix不自动加载交换（swap）空间，不自动加载其它任何外围设备。Helix还可以自动加载Windows，以应对意外情况。

### Bastille：Linux、Mac OS X和HP-UX的安全加强脚本

> Bastille使操作系统固若金汤，减少系统遭受危险的可能，增加系统的安全性。Bastille还可以评估系统当前的安全性，周期性的报告每一项安全设置及其工作情况。Bastille当前支持Red Hat（Fedora Core、Enterprise和Numbered/Classic版本）、SUSE、Debian、Gentoo和Mandrake这些Linux版本，还有HP-UX和Mac OS X。Bastille旨在使系统用户和管理员了解如何加固系统。在其默认的最坚固模式下，它不断的询问用户问题，并对这些问题加以解释，根据用户对问题不同的回答选择不同的应对策略。在其评估模式下，它会生成一份报告旨在告诉用户有哪些安全设置可用，同时也提示用户哪些设置被加固了。

### Acunetix Web Vulnerability Scanner：商业漏洞扫描器

> Acunetix WVS自动检查您的网页程序漏洞，例如SQL注入、跨网站脚本和验证页面弱密码破解。Acunetix WVS有着非常友好的用户界面，还可以生成个性化的网站安全评估报告。

### TrueCrypt：开源的Windows和Linux磁盘加密软件

> TrueCrypt是一款非常出色的开源磁盘加密系统。用户可以加密整个文件系统，它可以实时加密/解密而不需要用户干涉，只要事先输入密码。非常巧妙的hidden volume特性允许您对特别敏感的内容进行第二层加密来隐藏它的存在。所以就算加密系统的密码暴露，黑客也不知道还有隐藏内容存在。

### Watchfire AppScan：商业网页漏洞扫描器

> AppScan按照应用程序开发生命周期进行安全测试，早在开发阶段就进行单元测试和安全保证。Appscan能够扫描多种常见漏洞，例如跨网站脚本、HTTP应答切开、参数篡改、隐藏值篡改、后门/调试选项和缓冲区溢出等等。

### N-Stealth：网页服务器扫描器

> N-Stealth是一款网页服务器安全扫描器。它比Whisker/libwhisker和Nikto这些免费的网页扫描器升级得更频繁，但是它网站上声称的可以扫描30000种漏洞（30000 vulnerabilities and exploits）和每天添加数十种漏洞（Dozens of vulnerability checks are added every day）的说法是很值得怀疑的。象Nessus、ISS Internet Scanner、Retina、SAINT和Sara这些防漏洞工具都含有网页扫描组件，它们都很难做到每日更新。N-Stealth运行于Windows平台之上，且不开源。

### MBSA：微软基准安全分析器（Microsoft Baseline Security Analyzer）

> Microsoft Baseline Security Analyzer（MBSA）是一款简单易用的工具，帮助IT专业人员检测其小型和中型商业应用的安全性，将用户系统与微软安全建议（Microsoft security recommendations）进行比对，并给出特定的建议指导。通过与Windows内置的Windows自动升级代理器（Windows Update Agent）和微软自动升级基础架构（Microsoft Update infrastructure）的协作，MBSA能够保证和其它微软管理产品的数据保持一致，它们包括微软自动升级（Microsoft Update（MU））、Windows服务器自动升级服务（Windows Server Update Services（WSUS））、系统管理服务器（Systems Management Server（SMS））和微软运行管理器（Microsoft Operations Manager（MOM））。MBSA平均每周要扫描3百万台电脑。
