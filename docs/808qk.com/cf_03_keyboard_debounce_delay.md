# CF 快速切枪与跳箱身法中的机械键盘消抖时间（Debounce Time）与回报延迟调优

> 战术领域：身法连跳 / 闪电切枪 ｜ 硬件机制：按键消抖 (Debounce) / 磁轴RT行程

核心排查结论：CF 身法和闪身切枪对按键时间戳区间仅有 10~25ms。当键盘开启长达 16ms 的消抖延迟时，连续点按会被固件当做杂波过滤。调整至 2~4ms 或使用磁轴可彻底解决吞键。

## 一、 Windows 键盘连击与延迟参数
注册表：`HKEY_CURRENT_USER\Control Panel\Keyboard`
- `KeyboardDelay` = 0
- `KeyboardSpeed` = 31

## 二、 磁轴 RT 黄金参数
- Actuation: 0.3mm
- RT 触发: 0.1mm
- RT 抬起: 0.1mm
