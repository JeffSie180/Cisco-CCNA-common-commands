# Cisco CCNA 備考指令 功德無量版
 
常用查詢
==

- 特權 EXEC 模式 `enable`
- 全域模式 `configure terminal`
- 線路子設定模式 `line console 0`
- 指定介面 `interface FastEthernet 0/1 `
- 終止執行中命令 `Ctrl-C`
- 終止執行錯誤指令 `Ctrl-Shift-6`
- 更改設備名稱
- 儲存設定檔 `R1# copy running-config startup-config`
```
Switch# configure terminal
Switch(config)# hostname 名稱
```
- 標語
```
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# banner motd #Authorized Access Only#
```

查看
==
### running 組態檔內容
`show  run(runing-config)`
  
### startup 組態檔內容
`show  start(startup-config)`
  
### flash(存放 IOS)內容
`show flash`
  
### 路由資訊
`show ip route`
  
### 網路介面
`show ip int brief`
  
### 登入狀況
`show users`
  
### 路由器軟體資訊
`show version`

### 網卡狀態/MAC位址/雙工模式/速度
`show  int   fa0/1`
  
### 清空 MAC Table
```
show mac-address-table  查詢
clear  mac-address-table 清空
```
  
### 檢查/追蹤路由
`traceroute IP`
  
- 查看 ARP 暫存資料：
`show arp`
  
- 顯示啟動開始執行過的指令：
`show  history`
  
- 設定儲存歷史指令的大小
`terminal history size`
  
- 查詢 Switch Vlan：
`show vlan`
  
### 查看vlan 
```
show vlan brief 
show vlan summary 
show vlan vlan 20 
show interface vlan 20 
show interface trunk 
show interface fa0/1 switchport
```
  
### 查看 RIP 
```
show ip protocols
show ip route (ip 位址)
```
  
### 查看 OSPF 
```
show ip protocols 
show ip ospf neighbor 
show ip ospf database 
show ip ospf interface serial 0/0/0
```
  
### 查詢 TRUNK 協定
`show int trunk`
  
- 查詢路由 AD 值：  
`show ip route 172.30.100.0`
  
### 查詢 CDP 鄰居表
`show cdp neighbors`
  
### 查詢目前正在運作的  EIGRP 介面
`show ip eigrp int`
  
### 查詢 EIGRP 鄰居
`show ip eigrp neighbors`


密碼
==

- 特權模式密碼
```
enable secret 密碼
enable password 密碼
```
- console 密碼
```
line console 0　//設定 console 閒置時間
password 密碼
login   //啟動密碼
```

- VTY 密碼
```
line vty 0 15
password 密碼
exec  time-out 60(六十秒不用就斷線) 
login   //啟動密碼
```
### 限制 telnet or ssh 連線
`Router(config-line)# transport input {ssh | telnet}`

### 加密所有明文密碼 
`ervice password-encryption`

### SSH
```
R1(config)# ip domain-name example.com
R1(config)# crypto key generate rsa general-keys modulus 2048
R1(config)# username Admin secret Str0ng3rPa55w0rd
R1(config)# ip ssh version 2
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
```


IPv6
==

### ipv6 IP設定 
```
int g0/0
ipv6 addr 2001:DB8:1:9::2/64 
no shut 
```
  
### Ripng 設定：
```
ipv6 unicast-routing
ipv6 router rip ccna04
int g0/0
ipv6 rip ccna04 enable
```
  
### ipv6 EUI-64 設定
```
int g0/0
ipv6 addr 2001:DB8:1:9::/64  eui-64   
no shut
```
  
### ipv6 靜態路由設定 
`ipv6 route 2001:1:1:1::/64 s0/0/0(或 next hop)`

### ipv6 預設路由設定
```
ipv6 route ::/0 2001:1:1:1::1(或離開介面)
或
ipv6 route ::/0 s0/0/0 2001:1:1:1::1(離開介面 + next hop)
```
  
> 如果以 link local 為 next hop ，必加離開介面!! 
  
### 啟動 ipv6 路由協定
```
ipv6 unicast-routing     //啟動 ipv6 路由協定(繞送功能)
ipv6 router rip ccna04   //啟動 RIPng 程序號碼
exit    
int g0/0
ipv6 rip ccna04 enable   //在該介面啟動RIPng 程序號碼
```
  
### 查詢 IPv6 NDP Cache 內容 
`show ipv6 neighbors`
  
### 刪除 IPv6 NDP Cache 內容 
`clear ipv6 neighbors`
  
### 介面啟動 IPv6 
```
int f0/1
ipv6 enable
  
```
### 啟動 DHCPv6 指令
```
ipv6 dhcp pool mypool          //設定 IPv6 DHCP 存儲區
dns-server 2001:aaaa::1111     //設定 DNS 位址
int f0/0                       
ipv6 dhcp server mypool        //指定 f0/0 使用 IPv6 DHCP 存儲區
ipv6 nd other-config-flag      //DHCP 僅會送出 DNS 資訊，O bit
```
  
 或-------------------------
  
`ipv6 nd managed-config-flag   //DHCP 負責 IPv6 位址配發，M bit`
  
- IPv6 Tunnel轉換機制 
```
int tunnel 3                       //建立 Tunnel 3 介面 
ipv6 addr c::1/64                  //設定 Tunnel 3 介面IP
tunnel mode ipv6ip                 //設定 Tunnel 3 使用的模式(IPv6  over IPv4)
tunnel source s0/0/0               //設定 Tunnel 3 的來源
tunnel destination 209.165.200.2   //設定 Tunnel 3 的目的
```
  
 成對----------------------------
  
```
int tunnel 2                       //建立 Tunnel 2 介面 
ipv6 addr c::2/64                  //設定 Tunnel 2 介面IP
tunnel mode ipv6ip                 //設定 Tunnel 2 使用的模式(IPv6  over IPv4)
tunnel source s0/0/1               //設定 Tunnel 2 的來源
tunnel destination 209.165.100.2   //設定 Tunnel 2 的目的
```
  
- 設定 IPv6 Tunnel 路由協定
  
 
```
ipv6 router rip p100
int tunnel 3               //請注意這裡是 Tunnel 介面，而不是連接 internet 的s0/0/0 介面
ipv6 rip p100 enable
int f0/0
ipv6 rip p100 enable
``` 
  
 成對---------------------------
  
```
ipv6 router rip p100
int tunnel 2               //請注意這裡是 Tunnel 介面，而不是連接 internet 的s0/0/0 介面
ipv6 rip p100 enable
int f0/0
ipv6 rip p100 enable
```
---

Switch
==

- SVI (Switched Virtual Interface)
```
SVI（Switched Virtual Interface）是在 Cisco 交換機上配置的虛擬介面，主要用於管理目的和路由之間的交互。SVI 通常用於 Layer 3（三層）交換機，也就是能夠進行路由功能的交換機。以下是 SVI 的一些主要特點和用途：

虛擬路由介面：

SVI 是一種虛擬路由介面，不同於物理介面，它不與任何實際的硬件端口直接關聯。SVI 通常與 VLAN（虛擬局域網）關聯，允許該 VLAN 上的主機通過 SVI 進行互聯網路溝通。
管理目的：

SVI 通常用於為交換機本身提供管理訪問點。例如，你可以為一個 VLAN 配置 SVI，然後通過這個介面來進行遠端管理，如 SSH 或 Telnet。
互聯網路路由：

在多層交換機中，SVI 可用於在不同 VLAN 之間進行路由，從而允許不同 VLAN 的主機彼此溝通。這避免了需要一個專用的物理路由器。
配置方式：

配置 SVI 時，你會為它指定一個 VLAN ID，然後配置一個 IP 地址。這個 IP 地址用於該 VLAN 上的路由和管理目的。
多用途：

SVI 也可以用於設定某些安全和 QoS（服務品質）政策，使它們應用於整個 VLAN。
例如，配置一個 VLAN 10 的 SVI 可以這樣進行：

arduino
Copy code
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.1.1 255.255.255.0
Switch(config-if)# no shutdown
這樣就創建了一個 VLAN 10 的 SVI，並給它分配了 IP 地址 192.168.1.1。這個介面可以用於在 VLAN 10 內部的主機和交換機之間進行溝通，也可以作為遠端管理的入口點。
```


- boot system `S1(config)# boot system flash:/檔案路徑/檔案名稱`

### switch vlan
```
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# interface vlan 1
Sw-Floor-1(config-if)# ip address 192.168.1.20 255.255.255.0
Sw-Floor-1(config-if)# no shutdown
Sw-Floor-1(config-if)# exit
Sw-Floor-1(config)# ip default-gateway 192.168.1.1
```

### 修改交換器雙工模式及速度：
```
int fa0/3
duplex auto
duplex full
duplex half
speed auto
speed 10
speed 100
```

### 當機復原
```
switch: set
BOOT=flash:/c2960-lanbasek9-mz.122-55.SE7/c2960-lanbasek9-mz.122-55.SE7.bin
(output omitted)
switch: flash_init
Initializing Flash...
flashfs[0]: 2 files, 1 directories
flashfs[0]: 0 orphaned files, 0 orphaned directories
flashfs[0]: Total bytes: 32514048
flashfs[0]: Bytes used: 11838464
flashfs[0]: Bytes available: 20675584
flashfs[0]: flashfs fsck took 10 seconds.
...done Initializing Flash.
```
```
switch: dir flash: 
Directory of flash:/
    2  -rwx  11834846                 c2960-lanbasek9-mz.150-2.SE8.bin
    3  -rwx  2072                     multiple-fs
```

```
switch: BOOT=flash:c2960-lanbasek9-mz.150-2.SE8.bin
switch: set
BOOT=flash:c2960-lanbasek9-mz.150-2.SE8.bin
(output omitted)
switch: boot
```

### 刪除 vlan 
```
no vlan 20 
delete flash:vlan.dat
``` 

### 啟動 L3 Switch 繞送功能：
`ip routing`
  
### 將L3交換器變成路由器的方法：
```
int f0/6
no switch port
ip addr 192.168.200.1 255.255.255.0
no shut
```

### Vlan Trunks & Native
```
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan vlan-id
Switch(config-if)# switchport trunk allowed vlan vlan-list
```

### Switch mode
```
Switch(config)# switchport mode { access | dynamic { auto | desirable } | trunk }
```

### 修改 Native Vlan   
```
inf f0/1
switchport trunk native  vlan 10
```

### 啟動port security 
```
switchport mode access(一定要access mode) 
switchport port-security 
switchport port-security mac-address  0000.0c9b.d2d8
switchport port-security mac-address sticky 
switchport port-security violation shutdown 
show port-security int fa0/24 
```

### 修改自動協商(DTP dynamic trunk protocol)參數  
```
int f0/1   //下列指令四選一
switchport mode access 
switchport mode trunk
switchport mode dynamic auto  
switchport mode dynamic desirable
```
  
### 停止自動協商(DTP)   
```
int f0/1
switchport nonegotiate  //如果先前是設定  dynamic 不可停止，需變更
```

### L3 Switch vlan routing
```
MLS(config-if)# switchport mode trunk
MLS(config-if)# switchport trunk native vlan ID
MLS(config-if)# switchport trunk encapsulation dot1q
MLS(config)# ip routing

MLS(config)# ipv6 unicast-routing
```

常見 VLAN 問題
==

### 缺少 VLAN
```
show vlan [brief]
show interfaces switchport
ping
```
### 交換器中繼連接埠問題
```
show interfaces trunk
show running-config
```
### 交換器存取連接埠問題
```
show interfaces switchport
show running-config interface
ipconfig
```
### 路由器設定問題

```
show ip interface brief
show interfaces
```

### switch port security
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security
Command rejected: FastEthernet0/1 is a dynamic port.
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# end
```

- Limit and Learn MAC Addresses
```
Switch(config-if)# switchport port-security maximum value 
```

- Port Security Aging
```
Switch(config-if)# switchport port-security aging { static | time time | type {absolute | inactivity}}
```

- Port Security Violation Modes
```
Switch(config-if)# switchport port-security violation { protect | restrict | shutdown}
```


## STP

### 查詢擴張樹協定(STP；Spanning-Tree Protocol)執行狀況  
`show spanning-tree`
  
### 調整 STP BID Priority  
```
spanning-tree vlan 1 priority 28672 (數值必為4096的倍數)
或
spanning-tree vlan 1 root primary(指定交換器為 ROOT 交換器)
spanning-tree vlan 1 root secondary(指定交換器為備援交換器)
``` 
  
### 查詢 STP 表   
`show spanning-tree summary`
  
### 變更cisco pvst 為 Rapid-pvst    
`spanning-tree mode rapid-pvst`
  
### 查詢 Vlan 的 STP  
`show spanning-tree vlan 10,20`
  
### 指定 Vlan 的 STP Root 交換器 
```
spanning-tree vlan 10 root primary
spannong-tree vlan 20 root primary
```
  
### 指定 Vlan 的 STP 備援交換器  
```
spanning-tree vlan 10 root secondary
spannong-tree vlan 20 root secondary
```
  
### 啟動 all port 的 BPDU Guard 
```
spanning-tree portfast bpduguard default (PT  不支援)
或
int range f0/1-24
spanning-tree bpduguard enable
```
  
- 停用 STP 功能
`no  spanning-tree vlan 1`    
  
### 啟動 all port 的 PortFast 功能  
```
spanning-tree portfast default
spanning-ttee bpduguard enable  (通常會搭配啟用 bpduguard)
```
  
### 啟動 port 的 PortFast 功能  
```
int f0/5
spanning-tree portfast
spanning-ttee bpduguard enable  (通常會搭配啟用 bpduguard)
```


### BPDU Guard
```
S1(config)# interface fa0/1
S1(config-if)# spanning-tree bpduguard enable
S1(config-if)# exit
S1(config)# spanning-tree portfast bpduguard default
S1(config)# end
S1# show spanning-tree summary
```

EtherChannel
==

### PAgp
 - mode (On, desirable, auto)
```
S1# show interface trunk

S1#(config)# interface range f0/21-22
S1#(config-if-range)# shutdown
S1#(config-if-range)# channel-group 1 mode desirable
S1#(config-if-range)# no shutdown

S1#(config)# interface port-channel 1
S1#(config-if)# switchport mode trunk

S1# EtherChannel summary
```

### LACP
- mode (On, active, passive)
```
S1#(config)# interface range g0/1-2
S1#(config-if-range)# shutdown
S1#(config-if-range)# channel-group 2 mode desirable
S1#(config-if-range)# no shutdown

S1#(config)# interface port-channel 2
S1#(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 1,2,20
```

### 查詢 EtherChannel 執行狀態    
```
show etherchannel summary 
或 
show etherchannel port-channel
``` 

### Trobleshoot Etherchannel

- Summary Information (摘要資訊)
- Verify EtherChannel is Operational(確認運作)
`S1# show etherchannel summary`

- Port Channel Configuration
`S1# show run | begin interface port-channel`

- Correct the Misconfiguration(更正錯誤設定)
`S1(config)# no interface port-channel 1`


### 修改負載平衡方式   
```
port-channel load-blance ?
port-channel load-blance dst-mac
```
  
### 查詢負載平衡狀態  
`show etherchannel load-balance`
  
### 在 EtherChannel 執行 Trunk   
```
int po1
switchport mode trunk

int po2
switchport mode trunk
```
  
### 設定 GW 備援(HSRP 模式)
```
int f0/0
standby 10 ip 10.10.10.10
```
  
 ------成對---------------
```
conf t
int f0/0
standby 10 ip 10.10.10.10
```
  
### 指定 HSRP Active Router(設定優先權)  
```
int f0/0
standby 10 preempt
standby 10 priority 150
```
  
### 查詢 HSRP 狀態綱要    
`show standby brief`
  
### 設定 GW 負載平衡(設定二個 HSRP，各自指定一個 Active Router)  
```
R1#
conf t
int f0/0 
standby 10 ip 10.10.10.10
standby 10 preepmt
standby 10 priority 150
syandby 20 ip 10.10.10.20
  
R2#
conf t
int f0/0 
syandby 10 ip 10.10.10.10
```
  
 -----成對---------------------- 
  
```
R2#
conf t
int f0/0 
standby 20 ip 10.10.10.20
standby 20 preepmt
standby 20 priority 150
  
R1#
conf t
int f0/0 
syandby 20 ip 10.10.10.20
```
  
### 設定 HSRP 介面追蹤指令    
```
int f0/0
standby 10 track s0/0/0 60 
``` 
//連往外網的線路有問題時，優先權就減60，讓另一部 router 成為 active router
  
### 設定 VRRP(PT不支援)     
```
show vrrp
show vrrp brief
conf t
vrrp 10 ip 10.10.10.10
vrrp 10 preepmt
vrrp 10 priority 150
```
  
### 設定 GLBP(PT不支援)     
```
show glbp
show glbp brief
conf t
int f0/0
glbp 20 ip 10.10.10.20
glbp 20 preepmt
glbp 20 priority 150
```
  
---


Router
==

### IPv4 Static Route
```
Router(config)# ip route network-address subnet-mask { ip-address | exit-intf [ip-address]} [distance]
```

### IPv6 Static Route
```
Router(config)# ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-intf [ipv6-address]} [distance]
```

### Common Troubleshooting Commands
```
ping
traceroute
show ip route
show ip interface brief
show cdp neighbors detail
```


DHCPv4
==

### IPv4 
```
Router(config)# ip dhcp excluded-address low-address [high-address]
```

### dhcp pool
```
Router(config)# ip dhcp pool pool-name
Router(dhcp-config)# 
```

### 定義位址集區 (Define the address pool)
```
network network-number [mask | / prefix-length]
```
### 定義預設路由器或閘道 (Define the default router or gateway)
```
default-router address [ address2….address8]
```
### 定義 DNS 伺服器 (Define a DNS server)
```
dns-server address [ address2…address8]
```
### 定義網域名稱 (Define the domain name)
```
domain-name domain
```
### 定義 DHCP 租用的持續時間 (Define the duration of the DHCP lease)
```
lease {days [hours [ minutes]] | infinite}
```
### 定義 NetBIOS WINS 伺服器 (Define the NetBIOS WINS server)
```
netbios-name-server address [ address2…address8]
```


### DHCPv4 Service
```
R1(config)# no service dhcp
R1(config)# service dhcp
R1(config)# 
```



### 無狀態 DHCPv6 Server
```
R1(config-if)# ipv6 nd other-config-flag
R1(config-if)# end
R1#
R1# show ipv6 interface g0/0/1 | begin ND
```
```
1. R1(config)# ipv6 unicast-routing
```
```
2. R1(config)# ipv6 dhcp pool IPV6-STATELESS
```
```
3. R1(config-dhcpv6)# dns-server 2001:db8:acad:1::254
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)# exit
```
```
4. R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# description Link to LAN
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 nd other-config-flag
R1(config-if)# ipv6 dhcp pool IPV6-STATELESS
R1(config-if)# no shut
R1(config-if)# end
```

### 無狀態 DHCPv6 用戶端 (Configure a Stateless DHCPv6 Client)

```
1. R1(config)# ipv6 unicast-routing
```
```
2. R3(config)# interface g0/0/1
R3(config-if)# ipv6 enable
```
```
3. R3(config-if)# ipv6 address autoconfig
R3(config-if)# end
```
```
4. R3# show ipv6 interface brief
R3# show ipv6 dhcp interface g0/0/1
```


### 有狀態 DHCPv6
```
R1(config)# int g0/0/1
R1(config-if)# ipv6 nd managed-config-flag
R1(config-if)# ipv6 nd prefix default no-autoconfig
R1(config-if)# end
```



### DHCPv6 轉送代理 (Relay Agent)
```
Router(config-if)# ipv6 dhcp relay destination ipv6-address [interface-type interface-number]
```

OSPF
==

### 啟動ospf 
```
router ospf 1 
network 172.16.1.16 0.0.0.15  area 0 
network  172.30.1.0  0.0.0.255  area 0
network  192.168.10.0  0.0.0.3      area 0
```
  
### 查看 ospf 
```
show ip protocols 
show ip ospf neighbor 
show ip ospf database 
show ip ospf interface serial 0/0/0 
```

### 重新啟動OSPF 
`clear  ip ospf process `

### 直接設定router ID 
```
router ospf 1 
router-id 10.4.4.4 
clear ip ospf process
```

### Configure a Loopback Interface as the Router ID
```
R1(config-if)# interface Loopback 1
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# end
R1# show ip protocols | include Router ID
  Router ID 1.1.1.1
R1#
```

### 設定被動介面 (Passive Interfaces)
```
R1(config)# router ospf 10
R1(config-router)# passive-interface loopback 0
R1(config-router)# end
R1#
```

### 驗證 OSPF 鄰居
```
show ip interface brief - 這驗證所需的介面是否以正確的 IP 位址活動。
show ip route- 這驗證路由表包含所有預期的路由。
判斷 OSPF 如預期般運作的其他命令包括下列：

show ip ospf neighbor
show ip protocols
show ip ospf
show ip ospf interface
```

OSPFv3
==

### 設定  OSPFv3
```
ipv6 unicast-routing
ipv6 router ospf 100
router-id 1.1.1.1
int f0/0
ipv6 ospf 100 area 0
int s0/0/0
ipv6 ospf 100 area 0
```
  
### 重啟  OSPFv3
`clear ipv6 ospf process`
  
### 設定  link-local 
```
int g0/0
ipv6 addr fe80::1  link-local   
```



---
> 如果這篇文章對您有幫助，可以花 30 秒登入 LikeCoin 並點擊下方拍手按鈕（最多五下！）給予免費的支持，讓我們一起創造更多有價值的內容。



<style>
.likecoin-button {
  position: relative;
  width: 100%;
  max-width: 485px;
  max-height: 240px;
  margin: 0 auto 20px; /* 添加間距以分隔兩個部分 */
}
.likecoin-button > div {
  padding-top: 49.48454%;
}
.likecoin-button > iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.flex-container {
  display: flex;
  align-items: center;
  justify-content: center; /* 居中對齊元素 */
  margin-bottom: 20px; /* 底部間距 */
}
.flex-item {
  margin-right: 10px; /* 右邊間距 */
}
</style>

<!-- 將 "Buy me a coffee" 和 GIF 按鈕移到上方 -->
<div class="flex-container">
  <a href="https://www.buymeacoffee.com/jeffsie180" class="flex-item">
    <img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=☕&slug=jeffsie180&button_colour=FFDD00&font_colour=000000&font_family=Lato&outline_colour=000000&coffee_colour=ffffff" />
  </a>
  <iframe src="https://giphy.com/embed/FoAQVAmLEsOz8DV2HS" width="80" height="80" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>
</div>

<!-- 將 LikeCoin 按鈕移到下方 -->
<div class="likecoin-embed likecoin-button">
  <div></div>
  <iframe scrolling="no" frameborder="0" src="https://button.like.co/in/embed/jeffsie180/button?referrer=hackmd.io"></iframe>
</div>
