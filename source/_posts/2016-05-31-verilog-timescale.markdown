---
layout: post
title: "关于 Verilog 的 TimeScale"
date: 2016-05-31 10:31:08 +0800
comments: true
categories:
---
最近做芯片的功耗分析，需要用 PTPX 读入门级仿真写出的 VCD 文件。门级仿真的速度非常慢，所以关注了一下和速度相关的 TimeScale 的东西。

对于 TimeScale 的精确定义，可以参考 Veriog 的 1364 标准。手头的 2001 和 2005 两个版本，这方面的阐述是一样的，没有变化。

简要说，TimeScale 分 time unit 和 time precision 两部分，用符号 / 分割。Time Unit 定义就是出现在代码中的所有时间数字的单位；Time Precision 就是这个数字的精度。通常可以把二者的比值，理解成小数点后的有效数字位数。整个 design 中可能出现多个 TimeScale 的定义，仿真器按照最近出现的 TimeScale 来解析当前的 module。除非在仿真器的命令行做强制的定义，例如 VCS 的命令行选项 `-override_timescale=<time_unit>/<time_precision>` 。

标准中还提到，整个 design 所有 TimeScale 定义中，最小的 time precision 参数决定了仿真过程中的 time unit 的精度。显而易见，精度越小，仿真器的负担越重，速度也越慢。VCS 在编译完所有 module，打印出 `top level module` 之后，会接着打印出此次仿真过程的 TimeScale 是多少。注意去掉 `-q` 选项，让 VCS 打印比较详细的报告信息。这个 TimeScale 并不是已有的某个 module 的 TimeScale 组合，其中的 time unit 和 time precision 分别代表了整个 design 中的最小的 time unit 和 最小的 time precision。这里的 time unit 并不影响特定 module 解析时的 TimeScale 取值，但是这里的 time precision 会影响到整个仿真过程的全局参数，例如时间打印、结束时间以及此次仿真的精度。

这里提到的 design 包括 test bench。

VCS 提供了调试 TimeScale 的命令行选项 `-diag timescale` 来打印出各个 module 最终采用的 TimeScale 来自哪里。也可以通过标准的系统函数 `$printtimescale(module.path)` 打印出关心的 module instance 采用的 TimeScale 取值。

一些体会。最好每个 module 都自己定义好 TimeScale，尽量避免强制和继承这样的做法，这样出问题容易找到对应的定义在哪里。另外不要过度定义，在允许的范围内，尽量定义成粒度较大的值，避免给仿真器造成不必要的负担，降低整个 design 的仿真速度。仿真速度这个东西，在重要性和紧急性两个维度的事务划分中，最多算是重要不紧急的事情，在成熟固化的设计环境中，想有所提高很困难。

说到仿真速度，在 A 司的时候，还研究了 unit delay 对于仿真速度的影响。根据已有的 paper 显示，不带 unit delay 的仿真速度可以提高多少多少。为了实现这个目标，还重写了 design 中全部的 unit delay 定义。可惜也是没有见到最终的速度比较结果。
