# Intel 12/13/14 代大小核 CPU 在 CF 中的调度冲突排查：绑定 P-Core 性能核实战

> 适用硬件：Intel 12/13/14代酷睿 ｜ 核心瓶颈：Thread Director 误把游戏主线程派发至 E-Core

核心排查结论：Intel 异构 CPU 在运行 CF 时，Windows 线程调度器极易将游戏主线程错派至低频的能效小核（E-Core），造成帧率骤降。使用 PowerShell 进程掩码锁定 P-Core 运行，可稳定锁定高帧率。

## 一、 P-Core 核心掩码计算
前 8 个大核（16 线程）掩码为：`0xFFFF`。

## 二、 自动化守护绑定脚本
```powershell
$proc = Get-Process -Name "crossfire" -ErrorAction SilentlyContinue
if ($proc) {
    $proc.ProcessorAffinity = [IntPtr]0xFFFF
    $proc.PriorityClass = [System.Diagnostics.ProcessPriorityClass]::High
}
```

## 三、 BIOS 终极选项
在 BIOS 设置中关闭“Active Efficient Cores”，彻底消除小核物理干扰。
