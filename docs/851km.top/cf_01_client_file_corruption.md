# CF 客户端启动提示 Client File Corruption Detected 报错的系统性修复SOP

> 适用大区：全服 ｜ 排查耗时：约 5-10 分钟 ｜ 核心标签：客户端报错 / rez校验 / 崩溃排查

核心排查结论：该报错本质是 CrossFire 游戏引擎（LithTech Jupiter 修改版）在加载地图资源时，内存中挂载的 `.rez` 压缩包校验哈希与服务端下发签名不一致触发的完整性保护，通常由于磁盘扇区坏道、NTFS 权限异常或 DirectX 9 动态链接库被篡改所致。

## 一、 故障表征与触发场景定位
在游戏启动进度条达到 98% 或从大厅进入运输船/黑色城镇读条结算时，客户端突然黑屏弹出弹窗：`Client file corruption detected, Closing game client` 并强制结束 `crossfire.exe` 进程。

| 报错代码/现象 | 核心故障模块 | 底层成因 | 修复优先级 |
| :--- | :--- | :--- | :--- |
| Client Corruption 001 | rez/RF*.REZ | 地图或角色模型资源哈希校验失败 | P0 (核心) |
| Client Corruption 002 | d3dx9_43.dll | DirectX 9 运行时被第三方覆盖重写 | P1 (高) |
| Client Corruption 003 | CSystem.dll | Windows 系统权限隔离拦截内存读写 | P1 (高) |

## 二、 分步骤深度修复方案

### 步骤 1：系统级底层映像与系统文件完整性校验
以管理员身份打开 PowerShell，依次执行以下命令：
```powershell
sfc /scannow
DISM.exe /Online /Cleanup-image /Restorehealth
```

### 步骤 2：DirectX 9.0c 专属动态库强制注册与重装
```cmd
cd /d C:\Windows\SysWOW64
regsvr32 /u d3dx9_43.dll
regsvr32 d3dx9_43.dll
```

### 步骤 3：重置注册表客户端安装根路径
打开注册表编辑器 `regedit`，导航至：
`HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Tencent\CrossFire`
确认键值 `InstallPath` 必须与当前硬盘上的实际路径保持绝对一致，严禁包含中文。
