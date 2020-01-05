---
title: 分析IOS上APP的崩溃堆栈
categories: IOS
date: 2019-07-25
---

### 崩溃堆栈的几个来源
---

#### 真机上APP崩溃时，操作系统收集到的
```
0   libsystem_kernel.dylib        	0x00000001a24b70f4 0x1a249f000 + 98548
1   libsystem_kernel.dylib        	0x00000001a24b65a0 0x1a249f000 + 95648
2   CoreFoundation                	0x00000001a28b6a10 0x1a280d000 + 694800
3   CoreFoundation                	0x00000001a28b1920 0x1a280d000 + 674080
4   CoreFoundation                	0x00000001a28b10b0 0x1a280d000 + 671920
5   GraphicsServices              	0x00000001a4ab179c 0x1a4aa7000 + 42908
6   UIKitCore                     	0x00000001cf0e0978 0x1ce824000 + 9161080
7   SimpleGame iOS                	0x00000001046d1340 0x1045ec000 + 938816
8   libdyld.dylib                 	0x00000001a23768e0 0x1a2375000 + 6368
```
上面是IOS操作系统记录的原始崩溃堆栈信息，此时还未经由符号表**符号化**。看到的都是十六进制的内存地址，如果想要找到崩溃的地方对应于**哪个文件、哪个函数、哪一行**，就需要**符号化**才行

*分析堆栈格式：*
* 第一列：栈帧
* 第二列：二进制的应用程序名或者库名
* 第三列：被调用的函数的地址
* 第四列：是以**基址+偏移量**的形式表示的，前半部分指向某个源文件，后半部分指向这个源文件的某一行。

#### bugly上收集的

下面是bugly收集的原始堆栈信息，同样未经未**符号化**。

```
0 SimpleGame iOS	0x00000001050ae238 __cxa_throw + 8825280
1 SimpleGame iOS	0x0000000105095e34 __cxa_throw + 8725948
2 CoreFoundation	0x0000000181dbcc3c 0x0000000181ce5000 + 883772
3 CoreFoundation	0x0000000181dbc1b8 0x0000000181ce5000 + 881080
4 CoreFoundation	0x0000000181dbbf14 0x0000000181ce5000 + 880404
5 CoreFoundation	0x0000000181e3984c 0x0000000181ce5000 + 1394764
6 CoreFoundation	0x0000000181cf2f38 _CFXNotificationPost + 384
7 Foundation	        0x0000000182763bbc 0x000000018275d000 + 27580
8 UIKit	                0x000000018bbfe8a4 0x000000018b9f0000 + 2156708
9 UIKit	                0x000000018c5f854c 0x000000018b9f0000 + 12617036
10 UIKit	        0x000000018bbf7df4 0x000000018b9f0000 + 2129396
11 UIKit	        0x000000018c5f8230 0x000000018b9f0000 + 12616240
12 UIKit	        0x000000018ba31730 0x000000018b9f0000 + 268080
13 UIKit	        0x000000018ba310b8 0x000000018b9f0000 + 266424
14 UIKit	        0x000000018bb1afcc 0x000000018b9f0000 + 1224652
15 QuartzCore	        0x0000000185fc1504 0x0000000185e8f000 + 1254660
16 libdispatch.dylib	0x000000018171ca60 0x000000018171b000 + 6752
17 libdispatch.dylib	0x000000018175dd80 0x000000018171b000 + 273792
18 CoreFoundation	0x0000000181dd3070 0x0000000181ce5000 + 974960
19 CoreFoundation	0x0000000181dd0bc8 0x0000000181ce5000 + 965576
20 CoreFoundation	0x0000000181cf0da8 CFRunLoopRunSpecific + 552
21 GraphicsServices	0x0000000183cd5020 GSEventRunModal + 100
22 UIKit	        0x000000018bd0d758 UIApplicationMain + 236
23 SimpleGame iOS	0x00000001048f960c __cxa_throw + 744852
24 libdyld.dylib	0x0000000181781fc0 0x0000000181781000 + 4032
```

下面是上面的原始堆栈信息经过**符号化**后的样子。

```
0 SimpleGame iOS	cocos2d::GLView::getScaleX() const
1 SimpleGame iOS	-[CCEAGLView onUIKeyboardNotification:] + 656
2 CoreFoundation	___CFNOTIFICATIONCENTER_IS_CALLING_OUT_TO_AN_OBSERVER__ + 20
3 CoreFoundation	__CFXRegistrationPost + 428
4 CoreFoundation	____CFXNotificationPost_block_invoke + 216
5 CoreFoundation	-[_CFXNotificationRegistrar find:object:observer:enumerator:] + 1408
6 CoreFoundation	_CFXNotificationPost + 384
7 Foundation	        -[NSNotificationCenter postNotificationName:object:userInfo:] + 68
8 UIKit	                -[UIInputWindowController postEndNotifications:withInfo:] + 1628
9 UIKit	                ___77-[UIInputWindowController moveFromPlacement:toPlacement:starting:completion:]_block_invoke.1099 + 100
10 UIKit	        -[UIInputWindowController performWithSafeTransitionFrames:] + 220
11 UIKit	        ___77-[UIInputWindowController moveFromPlacement:toPlacement:starting:completion:]_block_invoke.1090 + 928
12 UIKit	        -[UIViewAnimationBlockDelegate _didEndBlockAnimation:finished:context:] + 760
13 UIKit	        -[UIViewAnimationState sendDelegateAnimationDidStop:finished:] + 312
14 UIKit	        -[UIViewAnimationState animationDidStop:finished:] + 296
15 QuartzCore	        CA::Layer::run_animation_callbacks(void*) + 284
16 libdispatch.dylib	__dispatch_client_callout + 16
17 libdispatch.dylib	__dispatch_main_queue_callback_4CF$VARIANT$armv81 + 964
18 CoreFoundation	___CFRUNLOOP_IS_SERVICING_THE_MAIN_DISPATCH_QUEUE__ + 12
19 CoreFoundation	___CFRunLoopRun + 2272
20 CoreFoundation	CFRunLoopRunSpecific + 552
21 GraphicsServices	GSEventRunModal + 100
22 UIKit	        UIApplicationMain + 236
23 SimpleGame iOS	main (main.m:6)
24 libdyld.dylib	_start + 4
```

#### 一些列外情况

下面这样的：
```
0 SimpleGame iOS	0x0000000103469820 __cxa_throw + 6463400
1 SimpleGame iOS	0x00000001034697b8 __cxa_throw + 6463296
2 SimpleGame iOS	0x00000001033e6898 __cxa_throw + 5926944
3 SimpleGame iOS	0x0000000103427c1c __cxa_throw + 6194084
4 SimpleGame iOS	0x00000001033e6c6c __cxa_throw + 5927924
5 SimpleGame iOS	0x00000001033e62bc __cxa_throw + 5925444
6 SimpleGame iOS	0x00000001033e6f40 __cxa_throw + 5928648
7 SimpleGame iOS	0x000000010344eb38 __cxa_throw + 6353600
8 SimpleGame iOS	0x00000001033b97c8 __cxa_throw + 5742416
9 SimpleGame iOS	0x00000001033b98c0 __cxa_throw + 5742664
10 SimpleGame iOS	0x0000000103382ef8 __cxa_throw + 5518976
11 SimpleGame iOS	0x00000001034fb724 __cxa_throw + 7061164
12 SimpleGame iOS	0x00000001034fa994 __cxa_throw + 7057692
13 SimpleGame iOS	0x00000001034fdbd0 __cxa_throw + 7070552
14 SimpleGame iOS	0x00000001034fd924 __cxa_throw + 7069868
15 SimpleGame iOS	0x000000010355e300 __cxa_throw + 7465608
16 SimpleGame iOS	0x0000000103581a0c __cxa_throw + 7610772
17 SimpleGame iOS	0x0000000103583478 __cxa_throw + 7617536
18 QuartzCore	        0x00000001fcf18f90 0x00000001fcf08000 + 69520
19 QuartzCore	        0x00000001fcfe2b10 0x00000001fcf08000 + 895760
20 CoreFoundation	0x00000001f8afca8c 0x00000001f8a79000 + 539276
21 CoreFoundation	0x00000001f8b23690 0x00000001f8a79000 + 698000
22 CoreFoundation	0x00000001f8b22ddc 0x00000001f8a79000 + 695772
23 CoreFoundation	0x00000001f8b1dc00 0x00000001f8a79000 + 674816
24 CoreFoundation	0x00000001f8b1d0b0 CFRunLoopRunSpecific + 436
25 GraphicsServices	0x00000001fad1d79c GSEventRunModal + 104
26 UIKitCore	        0x0000000225389978 UIApplicationMain + 212
27 SimpleGame iOS	0x0000000102ef560c __cxa_throw + 744852
28 libdyld.dylib	0x00000001f85e28e0 0x00000001f85e1000 + 6368
```
符号化后，变成这样了：只有部分栈帧被符号化成有用的信息，但还有一部分和符号化之前的形式差不多！
```
0 SimpleGame iOS	__cxa_throw + 6463400
1 SimpleGame iOS	__cxa_throw + 6463296
2 SimpleGame iOS	__cxa_throw + 5926944
3 SimpleGame iOS	__cxa_throw + 6194084
4 SimpleGame iOS	__cxa_throw + 5927924
5 SimpleGame iOS	__cxa_throw + 5925444
6 SimpleGame iOS	__cxa_throw + 5928648
7 SimpleGame iOS	__cxa_throw + 6353600
8 SimpleGame iOS	__cxa_throw + 5742416
9 SimpleGame iOS	__cxa_throw + 5742664
10 SimpleGame iOS	__cxa_throw + 5518976
11 SimpleGame iOS	__cxa_throw + 7061164
12 SimpleGame iOS	__cxa_throw + 7057692
13 SimpleGame iOS	__cxa_throw + 7070552
14 SimpleGame iOS	__cxa_throw + 7069868
15 SimpleGame iOS	__cxa_throw + 7465608
16 SimpleGame iOS	__cxa_throw + 7610772
17 SimpleGame iOS	__cxa_throw + 7617536
18 QuartzCore		CA::Display::DisplayLink::dispatch_items(unsigned long long, unsigned long long, unsigned long long) + 636
19 QuartzCore		display_timer_callback(__CFMachPort*, void*, long, void*) + 272
20 CoreFoundation	___CFMachPortPerform + 188
21 CoreFoundation	___CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE1_PERFORM_FUNCTION__ + 56
22 CoreFoundation	___CFRunLoopDoSource1 + 440
23 CoreFoundation	___CFRunLoopRun + 2096
24 CoreFoundation	CFRunLoopRunSpecific + 436
25 GraphicsServices	GSEventRunModal + 104
26 UIKitCore		UIApplicationMain + 212
27 SimpleGame iOS	__cxa_throw + 744852
28 libdyld.dylib	_start + 4
```

### 疑惑
---

* 符号化后的堆栈信息依然带有`+ number`的形式，不清楚是什么意思。(行数应该以`main (main.m:6)`的形式标识的)
* 某些堆栈经过符号化后，和未符号化的差不多，依然看不到有用的信息
* 内存不足或者用户强制杀死app，bugly也会搜集信息吗？
* 暂时先这样吧，后面有空有机会再填这些坑。。。

*因为要查找线上IOS APP的一个崩溃问题，我查阅了很多相关的网页。发现一个现象：好多好多的中文内容其实都是雷同，甚至复制粘贴的。后来我又发现：尼玛，这些中文内容竟然还是翻译的同一篇英文文章！*

### 参考
* 最初的英文文章：https://www.raywenderlich.com/2805-demystifying-ios-application-crash-logs
* 下面是雷同或者相似的中文文章：
* https://www.jianshu.com/p/3261493e6d9e
* https://www.jianshu.com/p/12a2402b29c2
* https://blog.csdn.net/hello_hwc/article/details/80946318
* https://www.jianshu.com/p/104c362189d8
* https://www.jianshu.com/p/5efbb02ca6c3
* https://blog.csdn.net/jasonblog/article/details/19031517
* https://cloud.tencent.com/developer/article/1070400


