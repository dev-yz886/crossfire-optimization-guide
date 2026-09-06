# Windows 全屏优化 (FSO) 与 GameDVR 深度清洗：消除 CF 窗口化虚假全屏与撕裂

> 影响机制：Desktop Window Manager (DWM) / GameDVR ｜ 优化技术：独占全屏还原 / 禁用全屏优化

核心排查结论：Windows 默认启用的“全屏优化 (FSO)”将 CF 强行劫持在无边框伪全屏下，引入 DWM 桌面合成滞后。禁用 FSO 并清洗注册表 GameDVR，可恢复绝对低延迟的独占全屏。

## 一、 文件兼容性属性设置
定位 `crossfire.exe`：
1. 勾选【禁用全屏优化】；
2. 高 DPI 设置中勾选【替代高 DPI 缩放行为】为“应用程序”。

## 二、 注册表彻底清空 GameDVR
```powershell
New-ItemProperty -Path "HKCU:\System\GameConfigStore" -Name "GameDVR_Enabled" -PropertyType DWord -Value 0 -Force
New-ItemProperty -Path "HKCU:\System\GameConfigStore" -Name "GameDVR_FSEBehaviorMode" -PropertyType DWord -Value 2 -Force
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\GameDVR" -Name "AllowGameDVR" -PropertyType DWord -Value 0 -Force
```
