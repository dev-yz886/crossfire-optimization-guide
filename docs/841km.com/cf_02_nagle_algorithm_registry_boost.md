# Windows 系统级禁用 Nagle 算法与网络节流：消灭 CF 弹道开火延迟

> 作用范围：Windows 10 / 11 核心网络栈 ｜ 优化技术：禁用 Nagle 算法 / 关闭 Network Throttling

核心排查结论：Windows 默认启用的 Nagle 算法和网络节流机制会将小封包强行延时 40~200ms 合并发送。在注册表中关闭这两项限制，可彻底根除开火与击中反馈的滞后感。

## 一、 活动网卡参数一键写入
以管理员权限运行 PowerShell：
```powershell
$adapters = Get-NetAdapter | Where-Object { $_.Status -eq "Up" }
foreach ($adapter in $adapters) {
    $regPath = "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\$($adapter.InterfaceGuid)"
    New-ItemProperty -Path $regPath -Name "TcpAckFrequency" -PropertyType DWord -Value 1 -Force
    New-ItemProperty -Path $regPath -Name "TCPNoDelay" -PropertyType DWord -Value 1 -Force
}
```

## 二、 关闭系统网络限速
```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile" -Name "NetworkThrottlingIndex" -PropertyType DWord -Value 0xffffffff -Force
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile" -Name "SystemResponsiveness" -PropertyType DWord -Value 0 -Force
```
