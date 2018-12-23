# iptables

## 防火墙分类

主机防火墙、网络防火墙、硬件防火墙、软件防火墙、网络层防火墙、应用层防火墙/代理服务器

## 基本认识

Netfilter组件

    内核空间，集成在linux内核中

    扩展各种网络服务的结构化底层框架

    内核中选取五个位置放了五个hook(勾子) function(INPUT、 OUTPUT、FORWARD、 PREROUTING、 POSTROUTING)，而这五个hook function向用户开放，用户可以通过一个命令工具（iptables）向其写入规则

    由信息过滤表（table）组成，包含控制IP包处理的规则集（rules），规则被分组放在链（chain）上

三种报文流向：

    流入本机： PREROUTING --> INPUT-->用户空间进程

    流出本机：用户空间进程 -->OUTPUT--> POSTROUTING

    转发： PREROUTING --> FORWARD --> POSTROUTING

## iptables的组成

iptables由五个表和五个链以及一些规则组成

五个表table： filter、 nat、 mangle、 raw、 security

    filter表： 过滤规则表， 根据预定义的规则过滤符合条件的数据包
    
    nat表： network address translation 地址转换规则表

    mangle： 修改数据标记位规则表
    
    raw：关闭NAT表上启用的连接跟踪机制， 加快封包穿越防火墙速度

    security：用于强制访问控制（MAC）网络规则，由Linux安全模块（如SELinux）实现

优先级由高到低的顺序为:security -->raw-->mangle-->nat-->filter

五个内置链chain

    INPUT
    
    OUTPUT

    FORWARD

    PREROUTING
    
    POSTROUTING

## 命令使用格式

iptables [-t table] SUBCOMMAND chain [num|rule-spec]

rule-specification = [matches...][-j target]

CRUD：增删改查

子命令：

- 管理规则：

  - -A: append，追加

  - -I：insert：插入

  - -D: 删除

  - -R：替换

- 管理链:

  - -N: new

  - -X：删除一条自定义、空的、引用计数为0的链；

  - -E: 重命名自定义链；引用计数不为0的自定义链不能够被重命名，也不能被删除

  - -P：policy，设置 链的默认策略

  - -F：flush, 清空

  - -Z：zero，计数器置零

    - iptables的每条规则和每个链都有专用的两个计数器：pkts, bytes

- 查看：-L

  - -n：以数字显示主机地址和端口；

  - -v：verbose，详细，-vv, -vvv

  - -x：exact，精准

  - --line-numbers：行号

- 链：

  - 内置链:每个内置链对应于一个钩子函数

  - 自定义链:用于对内置链进行扩展或补充，可实现更灵活的规则组织管理机制；只有Hook钩子调用自定义链时，才生效

- 匹配条件

  - 检查报文：

    - TCP或UDP首部：源端口、目标端口

      - FSM: 有限状态机

    - IP首部: SIP，DIP

    - MAC首部：MAC地址 

  - 匹配条件

    - 通用匹配

      - [!] -s, --sip, --source-ip：报文源地址，其值可以是IP或网络地址；

      - [!] -d, --dip, --destination-ip: 

      - -i, --in-interface name：报文流入的接口；只能应用于数据报文流入环节，只应用于INPUT、 FORWARD、 PREROUTING链

      - -o, --out-interface，

      - -p protocol：四层协议 ， tcp, udp, icmp

    - 扩展匹配
      - 隐式扩展:在使用-p选项指明了特定的协议时，无需再用-m选项指明扩展模块的扩展机制，不需要手动加载扩展模块

      -p tcp [-m tcp]

        tcp协议的扩展选项
            [!] --source-port, --sport port[:port]：匹配报文源端口,可为端口范围

            [!] --destination-port,--dport port[:port]：匹配报文目标端口,可为范围
            
            [!] --tcp-flags mask comp

                mask 需检查的标志位列表，用,分隔
                    
                    例如 SYN,ACK,FIN,RST

                comp 在mask列表中必须为1的标志位列表，无指定则必须为0，用,分隔
            
            [!] --syn：用于匹配第一次握手

                相当于： --tcp-flags SYN,ACK,FIN,RST SYN

      -p udp

        udp

            [!] --source-port, --sport port[:port]：匹配报文的源端口或端口范围

            [!] --destination-port,--dport port[:port]：匹配报文的目标端口或端口范围


      -p icmp

        icmp

            [!] --icmp-type {type[/code]|typename}

                type/code

                    0/0 echo-reply icmp应答

                    8/0 echo-request icmp请求

      - 显式扩展：必须使用-m选项指明要调用的扩展模块的扩展机制，要手动加载扩展模块

        使用帮助：

            CentOS 6: man iptables

            CentOS 7: man iptables-extensions

        1、 multiport扩展

            以离散方式定义多端口匹配,最多指定15个端口

            [!] --source-ports,--sports port[,port|,port:port]...

                指定多个源端口

            [!] --destination-ports,--dports port[,port|,port:port]...

                指定多个目标端口

            [!] --ports port[,port|,port:port]...多个源或目标端口
        
        示例：

        iptables -A INPUT -s 172.16.0.0/16 -d 172.16.100.10 -p tcp -m multiport --dports 20:22,80 -j ACCEPT

        2、 iprange扩展

            指明连续的（但一般不是整个网络） ip地址范围

            [!] --src-range from[-to] 源IP地址范围

            [!] --dst-range from[-to] 目标IP地址范围

        3、 mac扩展

        指明源MAC地址

        适用于： PREROUTING, FORWARD， INPUT chains

        [!] --mac-source XX:XX:XX:XX:XX:XX

        示例：

        iptables -A INPUT -s 172.16.0.100 -m mac --mac-source 00:50:56:12:34:56 -j ACCEPT

        iptables -A INPUT -s 172.16.0.100 -j REJECT

        4、 string扩展

        对报文中的应用层数据做字符串模式匹配检测

        --algo {bm|kmp} 字符串匹配检测算法

            bm： Boyer-Moore

            kmp： Knuth-Pratt-Morris

        --from offset 开始偏移

        --to offset 结束偏移

        [!] --string pattern 要检测的字符串模式
        
        [!] --hex-string pattern要检测字符串模式， 16进制格式

        示例：

        iptables -A OUTPUT -s 172.16.100.10 -d 0/0 -p tcp --sport 80 -m string --algo bm --string “google" -j REJECT

        5、 time扩展

        根据将报文到达的时间与指定的时间范围进行匹配

        --datestart YYYY[-MM[-DD[Thh[:mm[:ss]]]]] 日期

        --datestop YYYY[-MM[-DD[Thh[:mm[:ss]]]]]

        --timestart hh:mm[:ss] 时间

        --timestop hh:mm[:ss]

        [!] --monthdays day[,day...] 每个月的几号

        [!] --weekdays day[,day...] 星期几， 1 – 7 分别表示星期一到星期日

        --kerneltz：内核时区，不建议使用， CentOS7系统默认为UTC

        注意： centos6 不支持kerneltz ， --localtz指定本地时区(默认)

        示例：

        iptables -A INPUT -s 172.16.0.0/16 -d 172.16.100.10 -p tcp --dport 80 -m time --timestart 14:30 --timestop 18:30 --weekdays Sat,Sun --kerneltz -j DROP

        6、 connlimit扩展

        根据每客户端IP做并发连接数数量匹配

        可防止CC(Challenge Collapsar挑战黑洞)攻击

        --connlimit-upto #：连接的数量小于等于#时匹配

        --connlimit-above #：连接的数量大于#时匹配

        通常分别与默认的拒绝或允许策略配合使用
        
        示例：

        iptables -A INPUT -d 172.16.100.10 -p tcp --dport 22 -m connlimit --connlimit-above 2 -j REJECTiptables命令

        7、 limit扩展

        基于收发报文的速率做匹配

        令牌桶过滤器

        --limit #[/second|/minute|/hour|/day]

        --limit-burst number

        示例：

        iptables -I INPUT -d 172.16.100.10 -p icmp --icmp-type 8 -m limit --limit 10/minute --limit-burst 5 -j ACCEPT

        8、 state扩展

            根据”连接追踪机制“去检查连接的状态，较耗资源

        conntrack机制：追踪本机上的请求和响应之
        
        状态有如下几种：

            NEW：新发出请求；连接追踪信息库中不存在此连接的相关信息条目，因此，将其识别为第一次发出的请求

            ESTABLISHED： NEW状态之后，连接追踪信息库中为其建立的条目失效之前期间内所进行的通信状态

            RELATED：新发起的但与已有连接相关联的连接，如： ftp协议中的数据连接与命令连接之间的关系

            INVALID：无效的连接，如flag标记不正确

            UNTRACKED：未进行追踪的连接，如raw表中关闭追踪iptables命令

        [!] --state state

        示例：

        iptables -A INPUT -d 172.16.1.10 -p tcp -m multiport --dports 22,80 -m state --state NEW,ESTABLISHED -j ACCEPT

        iptables -A OUTPUT -s 172.16.1.10 -p tcp -m multiport --sports 22,80 -m state --state ESTABLISHED -j ACCEPT

        已经追踪到的并记录下来的连接信息库

            /proc/net/nf_conntrack

        调整连接追踪功能所能够容纳的最大连接数量

            /proc/sys/net/nf_conntrack_max

        不同的协议的连接追踪时长

            /proc/sys/net/netfilter/

        注意： CentOS7 需要加载模块： modprob

 iptables的链接跟踪表最大容量为/proc/sys/net/nf_conntrack_max，各种状态的超时链接会从表中删除；当模板满载时，后续连接可能会超时

 * 解决方法：1.加大nf_conntrack_max 值2.降低 nf_conntrack timeout时间

- 处理动作，target

  - DROP：丢弃

  - REJECT：拒绝

  - ACCEPT：接受

  - RETURN：返回调用链

  - REDIRECT：端口重定向

  - SNAT：源地址转换

  - DNAT：目标地址转换

  - MASQUERADE：地址伪装

  - LOG：记录日志， dmesg

  - 自定义链

  - LOG：非中断target,本身不拒绝和允许,放在拒绝和允许规则前

    并将日志记录在/var/log/messages系统日志中

    --log-level level 级别： debug， info， notice, warning, error, crit,alert,emerg

    --log-prefix prefix 日志前缀，用于区别不同的日志，最多29个字符

## iptables命令

任何不允许的访问，应该在请求到达时给予拒绝

规则在链接上的次序即为其检查时的生效次序

基于上述，规则优化

    1 安全放行所有入站和出站的状态为ESTABLISHED状态连接

    2 谨慎放行入站的新请求

    3 有特殊目的限制访问功能，要在放行规则之前加以拒绝

    4 同类规则（访问同一应用），匹配范围小的放在前面，用于特殊处理

    5 不同类的规则（访问不同应用），匹配范围大的放在前面

    6 应该将那些可由一条规则能够描述的多个规则合并为一条

    7 设置默认策略，建议白名单（只放行特定连接）

        1） iptables -P，不建议

        2） 建议在规则的最后定义规则做为默认策略

规则有效期限：

使用iptables命令定义的规则，手动删除之前，其生效期限为kernel存活期限

保存规则：

保存规则至指定的文件

CentOS 6

    service iptables save

    将规则覆盖保存至/etc/sysconfig/iptables文件中

CentOS 7

    iptables-save > /PATH/TO/SOME_RULES_FILE

CentOS 6：

    service iptables restart

    会自动从/etc/sysconfig/iptables 重新载入规则

CentOS 7 重新载入预存规则文件中规则：

    iptables-restore < /PATH/FROM/SOME_RULES_FILE

    -n, --noflush：不清除原有规则

    -t, --test：仅分析生成规则集，但不提交

## 开机自动重载规则

开机自动重载规则文件中的规则：

(1) 用脚本保存各iptables命令；让此脚本开机后自动运行

/etc/rc.d/rc.local文件中添加脚本路径

/PATH/TO/SOME_SCRIPT_FILE

(2) 用规则文件保存各规则，开机时自动载入此规则文件中的规则

/etc/rc.d/rc.local文件添加

iptables-restore < /PATH/FROM/IPTABLES_RULES_FILE

(3)自定义Unit File，进行iptables-restore

## 网络防火墙

iptables/netfilter网络防火墙：

(1) 充当网关

(2) 使用filter表的FORWARD链

注意的问题：

(1) 请求-响应报文均会经由FORWARD链，要注意规则的方向性

(2) 如果要启用conntrack机制，建议将双方向的状态为ESTABLISHED的报文直接放行

## NAT

NAT: network address translation

    PREROUTING， INPUT， OUTPUT， POSTROUTING

    请求报文： 修改源/目标IP， 由定义如何修改

    响应报文：修改源/目标IP，根据跟踪机制自动实现
SNAT： source NAT POSTROUTING, INPUT

    让本地网络中的主机通过某一特定地址访问外部网络，实现地址伪装
    
    请求报文：修改源IP

DNAT： destination NAT PREROUTING , OUTPUT

    把本地网络中的主机上的某服务开放给外部网络访问(发布服务和端口映射)，但隐藏真实IP

    请求报文：修改目标IP

PNAT: port nat，端口和IP都进行修改SNAT

nat表的target：

SNAT：固定IP

    --to-source [ipaddr[-ipaddr]][:port[-port]]

    --random

iptables -t nat -A POSTROUTING -s LocalNET ! -d LocalNet -j SNAT --tosource ExtIP

示例：

iptables -t nat -A POSTROUTING -s 10.0.1.0/24 ! –d 10.0.1.0/24 -j SNAT --to-source 172.18.1.6-172.18.1.9SNAT

MASQUERADE：动态IP，如拨号网络

    --to-ports port[-port]

    --random

iptables -t nat -A POSTROUTING -s LocalNET ! -d LocalNet -j MASQUERADE

示例：

iptables -t nat -A POSTROUTING -s 10.0.1.0/24 ! –d 10.0.1.0/24 -j MASQUERADE

## DNAT

DNAT

--to-destination [ipaddr[-ipaddr]][:port[-port]]

iptables -t nat -A PREROUTING -d ExtIP -p tcp|udp --dport PORT -j DNAT --to-destination InterSeverIP[:PORT]

示例：

iptables -t nat -A PREROUTING -s 0/0 -d 172.18.100.6 -p tcp --dport 22-j DNAT --to-destination 10.0.1.22

iptables -t nat -A PREROUTING -s 0/0 -d 172.18.100.6 -p tcp --dport 80-j DNAT --to-destination 10.0.1.22:8080

## 转发

REDIRECT：

    NAT表

    可用于： PREROUTING OUTPUT 自定义链

    通过改变目标IP和端口，将接受的包转发至不同端口

    --to-ports port[-port]

示例：

iptables -t nat -A PREROUTING -d 172.16.100.10 -p tcp --dport 80 -j REDIRECT --to-ports 8080
