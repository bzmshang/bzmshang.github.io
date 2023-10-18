---
title: Xposed墓碑模块-NoActive专属文档
permalink: NoActive_Tombstone.html
date: 2022-08-14
tags:
banner_img: img/uploadfile/202208/30831660473623.jpg
index_img: img/uploadfile/202208/30831660473623.jpg
---

# <i id="文档版本"></i>文档版本
```
ver=v2.9.7

latest-update=2023.08.02

以下内容全文为NoActive的介绍与使用，请分享给有需要的人
```

## <i id="声明"></i>声明
> 1. 本文档
> 	 - 依据NoActive版本:`v2.9-Alpha(282)`<u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/NoActive官方版/历史版本 "下载")</u>
> 	 - 依据设备：`一加9PRO Color13`
> 2. 本文档是借鉴<u>[**`NoActive墓碑Xposed模块`**](https://www.myflv.cn/mobile/282.html "NoActive墓碑Xposed模块")</u>究极改良
> 3. 如搬运本文档的任何内容，请说明文档来源
：本文作者： <u>[**`冰之梦殇520`**](https://www.coolapk.com/u/3571197 "冰之梦殇520")</u>
> 4. NoActive：<u>[**`官网`**](https://app.myflv.cn/ "官网")</u>
> 5. **QQ频道**：<u>[**`点击加入`**](https://pd.qq.com/s/95pl09232 "点击加入")</u>
>
> - 更多：(加群，教程，文件)
> 	 - 手机版：请点击左上角三条杠查看更多
> 	 - 电脑版：请看顶部导航栏

## <i id="模块介绍"></i>1. 模块介绍：
### <i id="功能"></i>1-1. 功能：
- 通过Hook系统框架实现Android墓碑

### <i id="版本"></i>1-2. 版本：
1. NoActive群内版本(内测版)
2. myflavor博客版本(公测版)
3. LSPosed版本(稳定版)

## <i id="作用域说明"></i>2. 作用域说明：
### <i id="系统框架"></i>2-1. 系统框架：
1. Hook应用切换事件，冻结切换至后台的应用，解冻切换至前台的应用
2. Hook广播分发事件，屏蔽被冻结的应用接收广播，从而避免触发广播ANR
3. ~~Hook计算oom_adj事件，修改后台应用的oom_adj，白名单主进程500子进程700，冻结名单主进程700+子进程900+~~
4. Hook系统ANR事件，由于冻结之后，应用无法做出响应被系统认为是ANR，所以需要屏蔽ANR避免系统误杀被冻结的APP
5. Hook系统是否开启暂停执行已缓存变量获取，由于系统自带的暂停执行已缓存在收到广播后可能解冻再次活跃

### <i id="电量和性能"></i>2-2. 电量和性能(MIUI专属)：
-  以下是`NoActive` 跟 `NoActive Plugin` Hook `电量和性能`的作用
1. NoActive：防止夜间后台掉卡片
	 - <u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/NoActive官方版/历史版本 "下载")</u>
2. NoActive Plugin：防止杀掉后台
	 - <u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/NoActive+Plugin_1.0.apk "下载")</u>
- PS：`NoActive` 跟 `NoActive Plugin` Hook `电量和性能`不与任何模块冲突
## <i id="冻结方式说明"></i>3. 冻结方式说明：
- 目前Linux进程冻结方式有：
	 1. Kill -19
	 2. Kill -20
	 3. Cgroup Freezer v1
	 4. Cgroup Freezer v2

- 其中Kill -19和Kill -20兼容性最好，但是冻结过久，依然重载
- Google官方使用Cgroup Freezer v2
- Kill使用Android的Process.sendSignal，该方法为安卓封装间接调用Kill，所以可能存在部分系统19有效或者20有效，需要自测
- Cgroup Freezer v1和v2采用NoActive参考millet自行实现并封装，或v2调用安卓Process.setProcessFrozen实现
- 所以NoActive支持5种冻结方式分别为：
	 1. Kill -19
	 2. Kill -20
	 3. Cgroup Freezer v1(NoActive)
	 4. Cgroup Freezer v2(NoActive)
	 5. Cgroup Freezer v2(系统API)

- 推荐冻结方式 API & V2 > Kill -19 & Kill -20

**如果5种模式和提权模式都无法生效，可以切换Kill -19模式，在每次开机后执行下面两行命令**
```
su
```
```
magiskpolicy --live "allow system_server * process {sigstop}"
```

## <i id="配置文件说明"></i>4. 配置文件说明：
- 目录 /data/system/NoActive

### <i id="即时生效配置"></i>4-1. 即时生效配置：
1. blackSystemApp.conf 系统黑名单(系统APP默认白名单)
2. directApp.conf 强制冻结
3. topApp.conf 可见窗口
3. killProcess.conf 杀死进程名单(后台3S杀死进程)
4. socketApp.conf 保持连接(保持App后台网络连接)
5. whiteApp.conf 白名单APP(用户APP默认黑名单)
6. whiteProcess.conf 白名单进程(添加白名单APP无需添加)
7. highApp.conf 定时解冻(每3min解冻一次APP，5s后冻结APP,针对单应用)

### <i id="重启生效配置"></i>4-2. 重启生效配置：
1. debug 开启调试日志
2. ~~disable.oom 禁用修改oom_adj功能~~
3. ~~color.os ColorOS专属配置(特殊oom_adj方式)(新版本已去除此功能)~~
4. 冻结方式
	 4-1. kill.19 使用Kill -19冻结
	 4-2. kill.20 使用kill -20冻结
	 4-3. freezer.v1 使用Cgroup Freezer v1(NoActive)冻结
	 4-4. freezer.v2 使用Cgroup Freezer v2(NoActive)冻结
	 4-5. freezer.api 使用Cgroup Freezer API(系统API)冻结
5. interval.freeze 定时冻结（1分钟重新冻结一次所有黑名单应用）
6. interval.unfreeze <u>[**`轮番解冻`**](#轮番解冻)</u>
7. su.excute <u>[**`提权模式`**](#提权模式)</u>
8. v1.plus 缓解部分系统使用v1内存泄露

## <i id="日志文件说明"></i>5. 日志文件说明：
- 目录 /data/system/NoActive/log 会出现的两个文件
1. current.log (本次开机日志)
2. last.log (上一次开机日志)
[![](img/uploadfile/202208/312a1660985666.jpg)](img/uploadfile/202208/312a1660985666.jpg)

### <i id="如何开启详细日志"></i>5-1. 如何开启详细日志：
<font color="red">(PS：正常无需开启，一般用于跟作者讨论bug时所需)</font>
- NoActive版本在2.0+的可以在APP的设置里面打开详细日志这个按钮，并重启
[![](img/uploadfile/202210/fc601664966003.jpg)](img/uploadfile/202210/fc601664966003.jpg)

### <i id="日志级别"></i>5-2. 日志级别：
1. DEBUG(调试信息)
2. INFO(基本信息)
3. WARN(警告信息)
4. ERROR(错误信息)

## <i id="NoActive常用关键词说明"></i>6. NoActive常用关键词说明：
### <i id="重载"></i>6-1. 重载：
- 解释1：挂在后台的应用长时间不打开，等到下次打开，明明后台和进程都还在，但是应用会重新冷启动
- 解释2：应用卡片依旧存在于多任务界面，切换应用或是亮屏后进入应用（从多任务界面进入或是直接点击应用图标进入），该应用冷启动，丢失离开时的界面
[![](img/uploadfile/202210/868b1666883078.gif)](img/uploadfile/202210/868b1666883078.gif)

### <i id="闪弹"></i>6-2. 闪弹：
- 解释1：应用进入后台之后，你再次打开，应用在动画没结束之前闪退，你必须点击第二次，闪弹触发条件不明
- 解释2：应用卡片依旧存在于多任务界面，切换应用或是亮屏后进入应用（从多任务界面进入或是直接点击应用图标进入），该应用对操作无反应或是短时间内掉出应用界面，打开多任务界面任务卡片丢失，应用将冷启动
- 其中搜图神器出现的问题就是经典的闪弹
[![](img/uploadfile/202210/2a241666765033.gif)](img/uploadfile/202210/2a241666765033.gif)

### <i id="内存泄漏"></i>6-3. 内存泄漏：
- 解释1：当你打开软件，然后清掉后台，重复操作，内存占用会一直累加，等到内存耗尽，就会出现卡顿和死机
- 解释2：正常情况下杀死应用后会回收应用占用内存（如QQ占用1.7G内存，杀死QQ及其后台以后系统应能空闲出1.7G左右内存），但是内存泄漏情况下杀死应用后内存不会被回收，导致内存占用越来越多。目前仅freezer.v1存在此问题。
- <font color="red">下图中的只是比较经典的内存泄漏（多生进程），并不代表，这就是内存泄漏的唯一样式</font>
[![](img/uploadfile/202210/26851666766142.jpg)](img/uploadfile/202210/26851666766142.jpg)

### <i id="掉卡片"></i>6-4. 掉卡片：
- 解释1：软件进程还在，但是后台卡片被系统清理了
- 解释2：长时间待机的应用在划出多任务界面后，该应用卡片消失，有可能该应用进程也被杀。
- <font color="red">下图中以**抖音**为例</font>
[![](img/uploadfile/202210/79c91666792638.jpg)](img/uploadfile/202210/79c91666792638.jpg)

### <i id="所需软件"></i>7. 所需软件：
1. NoActive：<u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/NoActive官方版/历史版本 "下载")</u>、<u>[**`官网`**](https://app.myflv.cn/ "官网")</u>
2. MT管理器：<u>[**`官网`**](https://mt2.cn/ "MT管理器")</u>
3. 查看CPU占比：
> 1. Scene：<u>[**`酷安下载`**](https://www.coolapk.com/apk/com.omarea.vtools "酷安下载")</u>
> 2. Thanox：<u>[**`酷安下载`**](https://www.coolapk.com/apk/github.tornaco.android.thanos "酷安下载")</u>
4. Termux终端：<u>[**`官网`**](https://termux.dev/cn/index.html "官网")</u>

## <i id="模块内设置介绍"></i>8. 模块内设置介绍：
### <i id="模块设置"></i>8-1. 模块设置：
- 打开NoActive，并点击右上角的设置 **(如下)**
[![](img/uploadfile/202209/03171663724011.jpg)](img/uploadfile/202209/03171663724011.jpg)

#### <i id="冻结方式"></i>8-1-1. 冻结方式：
- `v1+`用来 **缓解** 部分系统使用v1造成内存泄露导致手机卡死所使用的冻结模式
- 其余冻结方式详情请查看<u>[**`冻结方式说明`**](#冻结方式说明)</u>
[![](img/uploadfile/202212/e75e1672220923.jpg)](img/uploadfile/202212/e75e1672220923.jpg)

#### <i id="提权模式"></i>8-1-2. 提权模式：
- 使原本NoActive的XP权限提升至ROOT权限
- 可以尝试使之前冻结失败或者无法使用的模式，可以正常使用
- 目前支持的提权模式仅有四种：Freezer v1/v2、kill-19/20
- 提权后，NoActive会自己给自己拉成白名单
- 需要`手动给`NoActive`自启动权限`，且不要用任何软件限制他(否则会导致系统卡死等等bug)，然后划卡后，就可以不再管他即可，工作时会自己启动

#### <i id="定时冻结"></i>8-1-3. 定时冻结：
- 1分钟重新冻结一次所有黑名单应用

#### <i id="轮番解冻"></i>8-1-4. 轮番解冻：
(该模式非常适合非MIUI系统的后台保活，建议非MIUI系统开启此功能)
- 目的：冻结过久，APP就死了，所以让他透透气，不让他憋死
- 顾名思义，就是一段时间，解冻一次，之前冻结的APP
- 具体：
	1分钟解冻一次，一次只解冻一个APP 3s，并且解冻的是最久没有打开的那个APP
- 详细：
	A-B-C-D依次为打开顺序
	会先解冻A，重新排序
	B-C-D-A
	然后解冻B
	此时顺序为
	C-D-A-B
	如果此时C被打开了
	那么就是
	D-A-B-C
	接下来就会解冻D
[![](img/uploadfile/202209/b0191663999135.jpg)](img/uploadfile/202209/b0191663999135.jpg)

#### <i id="详细日志"></i>8-1-5. 详细日志：
- 其实就是开启debug，一般无需开启， **除非作者跟你说你需要开启，才开启**

#### <i id="开发者"></i>8-1-6. 开发者：
- 就是作者，没啥好解释的

#### <i id="交流群"></i>8-1-7. 交流群：
- 跟文档右上角的加群一样的，也没啥好解释的

#### <i id="打赏"></i>8-1-8. 打赏：
- 懂得都懂

### <i id="APP设置"></i>8-2. APP设置(QQ为例)：
#### <i id="APP主页"></i>8-2-1. APP主页：
- 打开NoActive，映入眼帘的是APP的主页面
PS：(每个人使用的APP不一样，所以展示的仅为本人自用的)
- 在搜索框中搜索QQ，并打开他，在这，就是单独设置QQ（）
PS：(设置完成即可回到主页面)
[![](img/uploadfile/202208/41ea1660543582.jpg)](img/uploadfile/202208/41ea1660543582.jpg)

##### <i id="应用白名单"></i>8-2-1(1). 应用白名单：
- 让整个应用成为白名单
- PS：就是NoActvie不对QQ做任何操作

##### <i id="电池优化"></i>8-2-1(1/1). 电池优化：
- Tip：NoActive只要安装并激活后，就会接管`电池优化`
- 想要不限制应用又想优化续航
- 此功能在激活NoActive后，就会接管系统的电池优化
	 - 例如使用第三方MiPush的时候就需要他来开关电池优化<u>[**`查看`**](https://blog.bzmshang.top/MiPush-Framework_User-Guide#%E7%94%B5%E6%B1%A0%E4%BC%98%E5%8C%96 "查看")</u>
- PS：只有白名单情况下可开启
- QQ
[![](img/uploadfile/202210/365d1665990544.jpg)](img/uploadfile/202210/365d1665990544.jpg)
[![](img/uploadfile/202210/4df11665990838.jpg)](img/uploadfile/202210/4df11665990838.jpg)

- Play商店
[![](img/uploadfile/202210/ada71665990544.jpg)](img/uploadfile/202210/ada71665990544.jpg)
[![](img/uploadfile/202210/81231665990394.jpg)](img/uploadfile/202210/81231665990394.jpg)

##### <i id="保持连接"></i>8-2-1(2). 保持连接：
1. ~~网络解冻（MIUI专属）~~
2. 是否使用HMS、mipush这类第三方推送通道，而不使用APP自身的推送通道
	 - 开启时使用APP自身的推送，不使用HMS或者mipush这类第三方推送通道
	 - 关闭时不使用APP自身的推送，使用HMS或者mipush这类第三方推送通道

##### <i id="定时解冻"></i>8-2-1(3). 定时解冻：
- 定时解冻，每3min解冻一次APP，解冻后3s再冻结APP
- 定时解冻跟轮番解冻是相对的，定时解冻只针对单应用，轮番解冻则是全部在后台冻结的
- 如果开了定时解冻也开了轮番解冻，那么轮番解冻将会忽略开启定时解冻的APP

##### <i id="后台级别"></i>8-2-1(4). 后台级别：
- 原名：忽略前台
##### <i id="前台服务"></i>8-2-2(4/1). 前台服务
- 默认的冻结方式
- 切换应用或回到桌面3s后冻结
- 前台服务就是原先的默认模式
- 前台服务≠前台窗口
- 前台服务包括：可以看见窗口、通知栏、可以听到声音看到视频，只要你能凭借感官感知到这个应用在运行，他都是前台服务
- 图标长相（啥也不显示）
[![](img/uploadfile/202210/31361665137564.jpg)](img/uploadfile/202210/31361665137564.jpg)

##### <i id="可见窗口"></i>8-2-2(4/2). 可见窗口：
- 切换应用或回到桌面3s后冻结
- 可见窗口：范围就很窄了，你看得见的前台窗口(当前界面显示的应用)，系统小窗(反应在oomadj分数上，就是0.1.2的应用)
[![](img/uploadfile/202210/295e1665137989.jpg)](img/uploadfile/202210/295e1665137989.jpg)

- 图标长相
[![](img/uploadfile/202210/093e1665137564.jpg)](img/uploadfile/202210/093e1665137564.jpg)

##### <i id="强制冻结"></i>8-2-2(4/3). 强制冻结：
- 切换应用或回到桌面0s后冻结（俗称秒冻）
- 无脑冻结，只要不再当前应用的界面，就直接冻结
- 猜测是oomadj除了0，全都冻结
- 图标长相
[![](img/uploadfile/202210/afa61665137564.jpg)](img/uploadfile/202210/afa61665137564.jpg)

##### <i id="进程设置"></i>8-2-1(5). 进程设置：
- 剩下的就是QQ的进程，我们可以对每一个进程进行单独设置
- PS：例如：冻结、杀死、白名单(只是单个进程的白名单，跟APP白名单同理)
[![](img/uploadfile/202303/d7ac1677723655.jpg)](img/uploadfile/202303/d7ac1677723655.jpg)

#### <i id="黑白名单介绍"></i>8-2-2. 黑白名单介绍：
- 用户应用（QQ为例）
1. 黑名单
[![](img/uploadfile/202210/31361664967021.jpg)](img/uploadfile/202210/31361664967021.jpg)
2. 白名单
[![](img/uploadfile/202210/e12b1664967021.jpg)](img/uploadfile/202210/e12b1664967021.jpg)

- 系统应用（谷歌商店为例）
1. 黑名单
[![](img/uploadfile/202210/2e881664967021.jpg)](img/uploadfile/202210/2e881664967021.jpg)
2. 白名单
[![](img/uploadfile/202210/e4d61664967021.jpg)](img/uploadfile/202210/e4d61664967021.jpg)

## <i id="How_to_use_NoActive"></i>9. 如何使用NoActive：
1. 下载《NoActive》<u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/NoActive官方版/历史版本 "下载")</u>
2. 安装NoActive，并在LSPosed中，将NoActive的作用域勾选为《系统框架》
[![](img/uploadfile/202209/f6521663559631.jpg)](img/uploadfile/202209/f6521663559631.jpg)

3. 在LSPosed勾选玩作用域后，打开NoActive，这时候，就是跳出申请ROOT权限的提示，允许即可
[![](img/uploadfile/202209/5f301663725944.jpg)](img/uploadfile/202209/5f301663725944.jpg)
(<font color="red">如果没有弹出，请自行前往Magisk授予权限</font>)
[![](img/uploadfile/202209/a05e1663725944.jpg)](img/uploadfile/202209/a05e1663725944.jpg)

4. 重启手机
> 	 - PS:
> 1. 如果是MIUI系统需在重启前多一步，刷入<u>[**`Millet禁用`**](https://backup.bzmshang.top/Software/Tombstone/NoActive/%E4%BD%9C%E8%80%85%E7%9A%84millet%E9%85%8D%E7%BD%AE%E7%A6%81%E7%94%A8%E6%94%B9%E7%89%883.zip "Millet禁用")</u>，NoActive默认是禁用Millet的，但是my写NoActive的时候是基于米10开发版7月那一版，所以后面版本的MIUI都需要刷入Millet禁用(NoActive的理念就是干掉Millet，所以没有所谓的启用Millet，想启用可以试试f19没有新欢的MiT Lite，<u>[下载官网](mitlite.tf19.co "下载官网")</u>)
> 2. 如果重启手你设置的配置没有了，请看<u>[**`帮助3`**](#Help3 "帮助3")</u>)

5. 因为NoActive默认第三方APP都是黑名单，所以打开NoActive，并(**强烈建议**)将以下软件添加至白名单<u>[**`查看帮助0`**](#Help0 "查看帮助14")、</u>（不了解黑白名单请看<u>[**`黑白名单介绍`**](#黑白名单介绍)</u>）

6. 下面我们用抖音为例
(PS：如果你不清楚你要测试的APP是白名单还是黑名单<u>[**`黑白名单介绍`**](#黑白名单介绍)</u>)

7. 打开抖音，当页面加载完成后回到桌面
(PS：<font color="red">目前NoActive默认3s后开始冻结,所以在Scene或者Thanox查看的时候，一定需要在回到桌面后的3s后，查看CPU的占用</font>)
> 1. 使用Scene
> 	a. 打开Scene
> 	b. 点击功能，选择进程管理
> 	[![](img/uploadfile/202210/ee831664968988.jpg)](img/uploadfile/202210/ee831664968988.jpg)
> 	c. 在搜索框中搜索《**抖音**》，如果抖音的CPU为0%，则说明冻结成功，反之，则冻结失败
> 	[![](img/uploadfile/202210/439a1664968988.jpg)](img/uploadfile/202210/439a1664968988.jpg)
> 2. Thanox
> 	a. 打开Thanox
> 	b. 点击箭头的地方
> 	[![](img/uploadfile/202210/7c8d1664968988.jpg)](img/uploadfile/202210/7c8d1664968988.jpg)
> 	c. 进来后，找到《**抖音**》，如果抖音的CPU为0%，则说明冻结成功，反之，则冻结失败
> 	[![](img/uploadfile/202210/1cf31664968988.jpg)](img/uploadfile/202210/1cf31664968988.jpg)

8. 如果冻结成功，请继续第9步，如果冻结失败，请<u>[**`切换冻结方式`**](#Help7)</u>
9. <font color="Darkgreen">你可以正常使用NoActive了</font>
PS1：NoActive是不需要保留后台就可以运行的，所以你设置好后，就可以将NoActive划卡了
PS2：其中关于“定时冻结”与“轮番解冻”，建议所有非MIUI系统的都开启该选项，开启后，须重启手机
PS3：提权模式，如果你将，v1、v2、kill-（19、20）四种冻结方式都使用后，还是冻结失败，你可以开启提权模式再次尝试，该配置须重启
(PS3-1：关于<u>[**`提权模式`**](#提权模式)</u>)
[![](img/uploadfile/202209/cdf01663723844.jpg)](img/uploadfile/202209/cdf01663723844.jpg)

### <i id="Help0"></i>帮助0：白名单使用推荐
1. `Magiak`(任何版本，包括第三方)
> 理由：如果不给白名单，大概率会发现，打开Magisk的时候，会导致Magisk卡住，无法操作
> 如果随机包名了请将随机后的Magisk白名单

2. `Thanox/Thanox Pro`/`Scene`
> 理由：这两种软件有些功能需要自启动，墓碑可能会导致他启动不起来，而引发功能失效

3. `输入法`(第三方非系统)
> 理由：由于NoActive对于第三方应用都是默认黑名单，而经过我多次测试，输入法如果不给白名单，大概率用着用着，就无法唤醒了

4. `微信`
> 由于微信FCM推送，且国内环境原因，FCM很鸡肋，所以就有以下两种方法
> [推荐] 将微信直接白名单，最保险
> [不推荐] 将微信主进程和推送进程白名单
> ```
com.tencent.mm
com.tencent.mm:push
```

5. `QQ`
> 对于QQ两种态度
> 1. 将QQ白名单，使用QQ自己的推送
> 2. 将QQ主进程和推送进程白名单
> ```
com.tencent.mobileqq
com.tencent.mobileqq:MSF
```
> 3. 将QQ黑名单，使用MiPush/Hms推送
> 	 - 关于MiPush推送，可以查阅<u>[**`MiPush Framework使用指南`**](https://blog.bzmshang.top/MiPush-Framework_User-Guide "MiPush Framework使用指南")</u>

6. `需要自启动的第三方APP`
> Color中最常用的模块`Luckytools`，由于他有部分功能，例如磁贴，强制刷新率等等需要软件自启动，所以，就需要给予`Luckytools`白名单
> 其他类似于这种软件的请自行给予白名单

7. `电池优化`
> 最经典的就是`第三方的MiPush推送服务`，由于NoActive接管了电池优化，所以，需要将推送服务白名单
> 其他类似于这种软件的请自行给予白名单

### <i id="Help1"></i>帮助1：保后台
- 用前须知
1. 本方法是跟我我自身使用的`一加9pro，Color13`所写，可能并不适用所有人
2. 操作方法
> - 方法1：
> -  使用保后台Magisk模块
> 1. 下载并安装`保后台`，该模块内置了`Don't Kill`，且针对Color系统，会自动禁用雅典娜
> 	 - <u>[**`云盘下载`**](https://backup.bzmshang.top/Software/Tombstone/保后台/Magisk版-焕晨 "焕晨")</u>
> 2. 重启手机
> - <i id="Help1-方法2"></i>方法2：
> -  使用Don't KillXP模块
> - 仅Color13可以不用管雅典娜了，Color11/12雅典娜依然是神一般的存在，请自行精简/冻结/卸载等
> 1. 下载并安装`Don't Kill`
> 	 - <u>[**`云盘下载`**](https://backup.bzmshang.top/Software/Tombstone/保后台/Don.t.Kill "云盘下载")</u>、作者：<u>[**`海浪逃向岛屿`**](http://www.coolapk.com/u/1755624 "海浪逃向岛屿")</u>
> 3. 重启手机
> - 方法3：
> -  使用超强保后台Magisk模块
> - 仅Color13可以不用管雅典娜了，Color11/12雅典娜依然是神一般的存在，请自行精简/冻结/卸载等
> 2. 下载并安装`超强保后台`
> 	 - <u>[**`云盘下载`**](https://backup.bzmshang.top/Software/Tombstone/保后台/Magisk版-开心小阳光 "云盘下载")</u>、作者：<u>[**`开心小阳光`**](http://www.coolapk.com/u/3520868 "开心小阳光")</u>
> 3. 重启手机
> - 方法4：
> - 使用`Thanox`的新功能`后台保护`，将自己需要保后台的应用选上
PS1：这个是新版本才有的功能，如果自己当前版本的`Thanox`没有这个功能，请升级
[![](img/uploadfile/202212/165e1672220923.jpg)](img/uploadfile/202212/165e1672220923.jpg)
PS2：`Thanox`的`后台保护`属于让系统认为该应用在前台运行，会导致墓碑失效，导致在后台冻结失败，所以使用此方法保后台，不建议对大量应用使用

- 重要须知
> 1. 上文中的方法2/3/4可以自行组合或单一使用，由于每个人手机不一样，所以，最终效果请自行测试
> 2. 上文中的方法1不适合给Color13用，因在本机上，杀后台比官方自己还严重，建议，使用方法2`Don't Kill`

- 以上方法可能已经过时，其余方法请自行实验，此部分只是提供思路
- 接下来你配合着NoActive就可以体验水果墓碑的感受

### <i id="Help2"></i>帮助2：NoActive支持到安卓几
- 【NoActive3.0之前的版本】最低支持到安卓10，但体验较好的是12-13

### <i id="Help3"></i>帮助3：NoActive重启后配置没了
1. 在LSPosed里面给NoActive勾选推荐作用域
2. 给予NoActive ROOT权限
3. 重启手机，这时候设置好再重启手机就不会丢配置了

### <i id="Help4"></i>帮助4：冲突
- 系统冲突
NoActive会屏蔽系统自己的墓碑，所以无需在意系统墓碑的开关
PS：此系统墓碑为开发者模式中的暂停应用缓存

- 模块冲突
由于NoActive自身就是墓碑模块，所以，只要是墓碑模块，就不能与NoActive共存，**任何**

- 作用域冲突
暂未发现

- 情景模式冲突
依靠Thanox中的情景模式的墓碑，例如早期myflaver写的<u>[**`情景模式墓碑`**](https://www.myflv.cn/other/281.html "情景模式墓碑")</u>

### <i id="Help5"></i>帮助5：Millet在MIUI几出现
1. MIUI13开始支持, 由电量与性能负责运行，分为FreezerV1和V2两个版本
2. MIUI13开发版最先支持，有最新特性，而MIUI稳定版在慢慢紧跟(开发板的运行稳定了再下放到稳定版)

### <i id="Help7"></i>帮助7：切换冻结方式
1. 打开NoActive，点击右上角的设置会进入设置界面(如下)
[![](img/uploadfile/202209/03171663724011.jpg)](img/uploadfile/202209/03171663724011.jpg)
2. 更改冻结方式
	- 冻结方式，只需右边选择即可
	[![](img/uploadfile/202212/e75e1672220923.jpg)](img/uploadfile/202212/e75e1672220923.jpg)
3. 选择好冻结方式后，就可以重启手机，例如我这换成v2模式的
<font color="Dark green">冻结方式推荐：API & V2 > Kill -19 & Kill -20 </font>
[![](img/uploadfile/202212/ff6a1672220923.jpg)](img/uploadfile/202212/ff6a1672220923.jpg)
4. 重启完成后即可
(PS：<u>[**`如何使用NoActive`**](#如何使用NoActive)</u>的6~9步，讲述的如何确定冻结模式生效)

### <i id="Help8"></i>帮助8：查看冻结状态(通用)
- 优先推荐myflavor的软件查看：【<u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/冻结查询/NoActive-Monitor "下载")</u>】

### <i id="Help15"></i>帮助15：挂载V2没效果，挂载不上？
1. 挂载V2需要你的内核支持V2才能挂载，如果挂载不了请换内核
2. 挂载V2需要你的内核支持V2才能挂载，如果挂载不了请换内核
3. 挂载V2需要你的内核支持V2才能挂载，如果挂载不了请换内核

- 挂载模块：<u>[**`下载`**](https://backup.bzmshang.top/Software/Tombstone/Other/cgroup挂载 "挂载模块")</u>

### <i id="Help-Last"></i>帮助Last：(如何卸载干净NoActive)
1. 将NoActive卸载
2. 删除在/data/system目录下的NoActive文件夹
3. 你获得了新生