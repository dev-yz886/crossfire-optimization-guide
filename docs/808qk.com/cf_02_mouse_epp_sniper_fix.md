# 狙击镜准心移动平滑度与 Windows 指针精度提高机制（EPP）底层解绑

> 核心目标：根除甩狙飘移 / 关闭系统鼠标加速度 ｜ 参数体系：EPP置零 / eDPI线性校正

核心排查结论：Windows 默认开启的“提高指针精确度”本质是一种非线性的硬件鼠标加速度算法（EPP）。它会导致手速越快准星滑移越远，彻底破坏肌肉记忆。必须在操作系统与注册表两个维度将其物理置零。

## 一、 系统与注册表置零操作
1. 运行 `main.cpl`，指针选项取消“提高指针精确度”，速度拉到 6/11；
2. 注册表清空加速度：
```cmd
reg add "HKEY_CURRENT_USER\Control Panel\Mouse" /v "MouseSpeed" /t REG_SZ /d "0" /f
reg add "HKEY_CURRENT_USER\Control Panel\Mouse" /v "MouseThreshold1" /t REG_SZ /d "0" /f
reg add "HKEY_CURRENT_USER\Control Panel\Mouse" /v "MouseThreshold2" /t REG_SZ /d "0" /f
```
