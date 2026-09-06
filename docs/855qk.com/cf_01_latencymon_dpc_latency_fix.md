# 使用 LatencyMon 诊断根治 CF 突发微卡顿：DPC 延迟杀手定位与卓越性能模式

> 排查工具：LatencyMon ｜ 核心瓶颈：DPC Latency（延迟过程调用）｜ 系统方案：卓越性能模式

核心排查结论：高配电脑玩 CF 出现突发抽搐微卡顿，多由驱动级 DPC 延迟超标（超过 1000μs）阻塞 CPU 中断引起。通过 LatencyMon 排查 nvlddmkm.sys/ndis.sys 并激活 Windows 卓越性能模式，可消灭抽搐卡顿。

## 一、 解锁并应用卓越性能电源方案
以管理员权限运行终端：
```powershell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```
在控制面板 -> 电源选项中勾选【卓越性能】。

## 二、 网卡驱动级节能关闭
在设备管理器中停用：
- Energy Efficient Ethernet (节能以太网)
- Green Ethernet (环保节能)
