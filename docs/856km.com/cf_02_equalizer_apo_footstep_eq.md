# 利用 Equalizer APO 精准强化 CF 脚步声频段：硬件级 EQ 调音实战

> 调音工具：Equalizer APO ｜ 核心频段：150Hz 木板重音 / 2400Hz 铁板换弹 / 4200Hz 拆包

核心排查结论：常规耳机过重的低音轰头会掩盖细微脚步声。通过开源 Equalizer APO 对 CF 关键声学频段（150Hz 与 2400Hz）实施 +4~5dB 精确提升并前置 -4dB 防破音，可大幅提升隔墙辨位能力。

## 一、 config.txt 黄金参数配置
```text
Preamp: -4 dB
Filter 1: ON PK Fc 150 Hz Gain 5 dB Q 1.41
Filter 2: ON PK Fc 500 Hz Gain -3.5 dB Q 2.0
Filter 3: ON PK Fc 2400 Hz Gain 4 dB Q 1.2
Filter 4: ON PK Fc 4200 Hz Gain 3.5 dB Q 1.0
Filter 5: ON HS Fc 9000 Hz Gain -2 dB
```

## 二、 安全合规说明
Equalizer APO 是微软官方标准 APO 架构扩展，仅在系统声卡管线层运算，完全不触碰游戏内存。
