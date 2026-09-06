# CF 1000Hz 与 8000Hz 鼠标回报率下视角撕裂与掉帧的底层成因及平滑配置

> 核心场景：快速甩枪掉帧 / 视角撕裂 ｜ 外设参数：鼠标回报率 / 队列大小

核心排查结论：CF 引擎诞生于 2007 年，其主循环消息泵使用单线程处理 Windows `WM_MOUSEMOVE` 消息。当使用 4000Hz/8000Hz 高回报率电竞鼠标时，每秒数千次的高频硬件中断会导致游戏主线程严重拥塞，造成“不动物理帧率 240FPS，一滑鼠标瞬间掉到 40FPS”的假死性撕裂。

## 一、 回报率实测对比
- 8000Hz：单核 CPU 占用 99%，剧烈卡顿，坚决不选；
- 1000Hz：平滑无丢帧，最推荐；
- 500Hz：稳定性最高，低配 CPU 首选。

## 二、 扩容鼠标输入队列注册表
注册表路径：
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\mouclass\Parameters`
新建 DWORD `MouseDataQueueSize`，十进制设为 `200`。
