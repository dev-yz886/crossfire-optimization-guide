# CF跨区对局跳Ping与网络抖动排查：MTU最佳寻优与路由节点丢包探测

> 适用大区：跨电信/联通/移动全区 ｜ 核心指标：消除网络抖动 (Jitter) / 解决瞬移拉扯

核心排查结论：CF 对局中的瞬时跳 Ping 和角色“瞬移回弹”，超过 80% 是由于本地宽带路由器 MTU 配置过大，导致游戏 UDP 封包在跨网骨干路由器遭遇分片重组失败。通过终端探测最佳报文长度并锁定网卡 MTU 为 1492 或 1472 可彻底根除。

## 一、 探测无分片 MTU 临界值
以管理员权限运行终端：
```cmd
ping -f -l 1472 119.147.16.1
```
若提示分片，以每次递减 10 探测，直至成功响应。最佳 MTU = 响应数值 + 28（通常 PPPoE 宽带为 1492）。

## 二、 网卡 MTU 持久化写入
1. 查看网卡索引：
```cmd
netsh interface ipv4 show subinterfaces
```
2. 写入最佳 MTU（假设以太网 Idx 为 14）：
```cmd
netsh interface ipv4 set subinterface "14" mtu=1492 store=persistent
```
