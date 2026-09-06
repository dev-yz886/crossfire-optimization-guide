# AMD Radeon 显卡驱动优化：消除 CF 烟雾弹卡顿与 Shader 编译卡顿

> 适用显卡：AMD Radeon RX 5000 / 6000 / 7000 系 ｜ 核心场景：进烟雾弹掉帧 / 着色器重编译顿挫

核心排查结论：AMD 显卡原生转译层在处理 CF 老式 Alpha 粒子透明度混合时，会突发性触发微着色器重新编译。通过清空损坏的 DxCache 缓存并启用 AMD Anti-Lag，可完全消除进烟顿挫。

## 一、 清空 AMD Shader 缓存
```powershell
Remove-Item -Path "$env:LOCALAPPDATA\AMD\DxCache\*" -Recurse -Force
```

## 二、 AMD 驱动关键配置
- Radeon Anti-Lag：开
- Radeon Chill：关
- 等待垂直刷新：始终关闭
- 曲面细分：覆盖应用程序并设为“关”
