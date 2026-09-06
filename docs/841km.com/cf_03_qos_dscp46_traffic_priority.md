# Windows 基于策略的 QoS 锁定 CrossFire.exe 封包最高 DSCP 转发优先级

> 关键技术：Windows 组策略 QoS / DSCP 46 加速转发 ｜ 核心成效：杜绝后台抢网 / 保障对局平稳

核心排查结论：后台下载或局域网并发流量容易掠夺 CF 关键封包。通过 Windows 本地组策略（gpedit.msc）为 crossfire.exe 挂载基于策略的 QoS 并标记 DSCP 46（加速转发），可强令网卡与路由器将 CF 数据包置于绝对第一顺位发送。

## 一、 组策略配置路径
1. `Win + R` 输入 `gpedit.msc`；
2. 展开：计算机配置 -> Windows 设置 -> 基于策略的 QoS；
3. 新建策略：
   - 策略名称：`CF_Game_Priority`
   - DSCP 值：`46`
   - 应用程序：`crossfire.exe`
   - 协议类型：`TCP 和 UDP`

## 二、 刷新组策略使之立即生效
```cmd
gpupdate /force
```
