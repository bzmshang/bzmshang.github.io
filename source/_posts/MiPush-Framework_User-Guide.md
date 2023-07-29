---
title: MiPush Framework使用指南
date: 2022-07-21
updated: 2022-07-21
biannr_img: img/uploadfile/202302/57a41675408064.png
index_img: img/uploadfile/202302/57a41675408064.png
---

# <i id="指南版本"></i>指南版本
```
ver=v0.7.1

latest-update=2022.07.21

以下内容全文为MiPush Framework使用指南，请分享给有需要的人
```

## <i id="声明"></i>声明
> 1. 本文档
> 	- 依据MiPush版本:`v0.3.9-XXXXXXXX`即群内最新内测版
> 	- 依据设备：`一加9PRO Color13`
> 2. 本文档是大量搬运群内精华消息然后重新排版优化
> 3. 如搬运本文档的任何内容，请说明文档来源
：本文作者：<u>[**`冰之梦殇520`**](https://www.coolapk.com/u/3571197 "冰之梦殇520")</u>
MiPushFramework维护者：<u>[**`NihilityT`**](https://github.com/NihilityT "NihilityT")</u>
> 4. 链接
> 	- MiPush：<u>[**`官网`**](https://github.com/NihilityT/MiPush/releases "官网")</u>
> 	- 推送服务：<u>[**`官网`**](https://github.com/NihilityT/MiPushFramework/releases "官网")</u>
> 	- 配置文件：<u>[**`官网`**](https://github.com/NihilityT/MiPushConfigurations "官网")</u>
> 	- 所需文件(全)：<u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush "云盘下载")</u>
> 	- QQ历史版本：<u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/QQ_Updates "云盘下载")</u>
>
> - 更多：(加群，教程，文件)
> 	 - 手机版：请点击左上角三条杠查看更多
> 	 - 电脑版：请看顶部导航栏

## <i id="名词解释"></i>名词解释
### <i id="什么是分应用"></i>1. 什么是分应用？
1. 模块的名字为：MiPush
2. 可以以应用身份发出通知，而不是以推送服务的身份
- 好处：以目标应用身份来通知，像应用本身发出的

### <i id="什么是透传消息"></i>2. 什么是透传消息？
- 直接发送给APP的消息，不会在通知栏显示
- 补充：小米推送已于2022年9月12日0点起，针对非 MIUI 官方应用，停止提供透传消息下发的服务<u>[**`详情`**](https://dev.mi.com/console/doc/detail?pId=2766)</u>

### <i id="关于精简"></i>3. 关于精简
- MiPush Framework最初是为了非MIUI也能享受到MIUI的推送服务，为了实现功能，和官方小米服务框架包名相同
- MIUI是自带小米推送的，如果为了通知自定义使用这个第三方的MiPush，需要先使用模块精简原有的小米服务框架
- 使用Delta版面具，请不要开强制使用超级用户列表，会导致精简模块失效

## <i id="指南"></i>指南
### <i id="MiPush使用教程-ROOT"></i>1. MiPush使用教程-ROOT
- 图文版：<u>[**`查看`**](https://www.coolapk.com/feed/41357294?shareKey=ZmQ2MWNhZTgxMzI4NjNjYjlmODk~&shareUid=330645&shareFrom=com.coolapk.market_12.4.2 "查看")</u>
- 文字版：<u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/Download "云盘下载")</u>

1. <i id="使用教程-ROOT-小米机型MIUI系"></i>小米机型MIUI系统：
PS1:黑鲨手机(没刷别的系统的)也使用此方法
PS2:<u>[**`小米机型MIUI系统视频教程`**](https://b23.tv/6vqqYWs "小米机型MIUI系统视频教程")</u>
> 1. 卸载小米服务框架更新
> 	 - PS1：卸载更新指的是小米服务框架更新过，把更新卸载掉，用系统自带的应用管理来卸载更新，如果没有更新过，没有卸载更新这个按钮就不需要进行这个操作
> 	- PS2：不要用Scene或者爱玩机工具箱之类的卸载小米服务框架，那只是假卸载，当然爱玩机的精简功能还是可以用的，用完后，就不需要刷入精简模块了
> 2. 刷精简模块
> 3. 重启手机
> 4. 安装推送服务和Mipush模块（Mipush模块，只需要勾选推荐应用）

2. 小米机型非MIUI系统：
> 1. 刷小米机型模块(Prop)
> 2. 安装推送服务和Mipush模块（Mipush模块，勾选推荐应用）
> 3. 重启手机

3. 非小米机型：
> 1. 安装推送服务和Mipush模块（Mipush模块，勾选推荐应用和需要伪装推送的软件）
> 2. 重启手机
> - Tip：不能注册的软件：
> 	 安装MiPushDeviceFake或MiPushfaker，勾选不能注册的软件，并重启不能注册的软件

4. 非小米机型MIUI系统：
> 1. 跟着<u>[**`小米机型MIUI系`**](#使用教程-ROOT-小米机型MIUI系)</u>的4步操作
> 2. 伪装
	 (方法1)安装Magisk模块(改成小米机型)(全局)
	 (方法2)安装MiPushDeviceFake或MiPushfaker
> 3. 重启
>
> - Tip：(方法2)待测试，成了记得反馈下

### <i id="MiPush使用教程-免ROOT"></i>2. MiPush使用教程-免ROOT
- 方法由群友<u>[wyj809](http://www.coolapk.com/u/21035894 "wyj809")</u>提出
- 下载
> 1. <u>[**`云盘下载-LSPatch`**](https://cloud.bzmshang.top/Software/LSP/LSPatch "云盘下载-LSPatch")</u>
> 2. <u>[**`云盘下载-伪装/推送`**](https://cloud.bzmshang.top/Software/MiPush/Download "云盘下载-伪装/推送")</u>
> 3. <u>[**`云盘下载-配置文件`**](https://cloud.bzmshang.top/Software/MiPush/Download/MiPushConfigurations "云盘下载-配置文件")</u>

- <i id="LSPatch如何使用"></i>LSPatch如何使用
	- <u>[**`本机网络adb调试、Lspatch使用方法`**](https://www.coolapk.com/feed/38114209?shareKey=MDc5YTgyZjI3ZmZhNjNjZTJlZmI~&shareFrom "本机网络adb调试、Lspatch使用方法")</u>

- 操作过程
	 1. 安装推送服务
	 2. 安装LSPatch
	 3. 安装伪装模块（目前有MiPushDeviceFake、MiPushFaker、MiPush模块等几种）
		 - 无需全部安装以及全部修补进应用里，后面会推荐使用
	 4. 使用LSPatch将伪装模块修补进需要推送的应用里（具体修补方法查看<u>[**`LSPatch如何使用`**](#LSPatch如何使用)</u>）
	 5. 安装修补好的应用，登录
	 6. 到推送服务应用页面可查询该应用是否已注册，如已注册说明伪装成功。如未注册或者注册了不推送，可以尝试更换其他伪装模块
	 7. 应用可推送后，如需配置文件可至云盘内下载使用

- <i id="应用下载"></i>应用下载
	 - 华为应用市场网页版下载应用安装包方法：
> 1. 手机浏览器输入网址： appgallery.huawei.com 
> 2. 选择一个应用，例如酷安，点击进入，这时网址显示为： appgallery.huawei.com/app/C10414599 
> 3. 把网址改成： appgallery.cloud.huawei.com/appdl/C10414599
> 	 1. 在 `huawei.com` 前增加个 `cloud.` 
> 	 2. 在 `app` 后面加入 `dl` 
> 4. 回车，下载

	 - 小米应用商店网页版下载应用安装包方法：
> 1. 手机浏览器输入网址： m.app.mi.com/details?id=（此处填应用包名）
> 	 - 例如下载今日头条
> 	 - 链接为： m.app.mi.com/details?id=com.ss.android.article.news
> 2. 点击：免费下载
> 3. 开始拼图验证
> 4. 通过后自动下载

- 伪装模块推荐
	 - 以下推荐模块并非仅能使用该模块才可注册及推送
	 - 另外其他应用未测试，可自己使用几种伪装模块换着修补尝试，方法相同
	 - 如果有测试新的模块和应用组合，记得来酷安或者Q群跟我提，我来新增上去


| **伪装模块**   | **MiPush**       | **MiPushFaker** | **MiPushDeviceFake** |
|:-------------:|:----------------:|:---------------:|:--------------------:|
|               |                  |                 |                      |
| **应用1**      | QQ&nbsp;         | QQ              | 百度                 |
| **应用2**      | 淘宝             | 淘宝            | 酷安                  |
| **应用3**      | 闲鱼             | 闲鱼            | 支付宝                |
| **应用4**      | 抖音             | 菜鸟            | 拼多多                |
| **应用5**      | 京东             |                 |                      |
| **应用6**      | 哈啰             |                 |                      |
|                |                  |                |                       |
| **应用1**      | 苏e行           | 米游社          |                       |
| **应用2**      |                 | 支付宝          |                       |
| **应用3**      |                 | 浙政钉          |                       |
| **应用4**      |                 | 饿了么          |                       |
| **应用5**      | 小红书           | 小宇宙          |                       |
|                |                 |                 |                       |
| **应用1**      | 哔哩哔哩         | 哔哩哔哩        |                       |
| **应用2**      | QQ邮箱           | 高德地图        |                       |
| **应用3**      | 今日头条         | 企业微信         |                       |
| **应用4**      | 虎牙直播         |                 |                       |
| **应用5**      | 百度贴吧         |                 |                       |
|                |                 |                 |                       |
| **应用1**      | Boss直聘        | APP分享          |                       |
| **应用2**      | 哔哩哔哩漫画     |                  |                       |

- 免root推送小贴士：
1. 酷安建议使用便携模式修补，否则会出现反复注册的情况。
2. QQ建议使用便携模式修补，可以改善漏消息的情况。
3. 闲鱼不建议使用MiPushFaker及MiPushDeviceFake模块修补，会出现打开闲鱼时状态栏消失的bug，建议使用MiPush模块修补。
4. 微博、高德地图、B站、百度贴吧，支付宝，使用修补版注册成功后，可卸载修补版再安装回原版，仍然可以收到消息推送。
5. 支付宝修补后偶尔会出现打开时闪退和字体大小失效等现象，重新打开、重新设置即可，不影响应用使用和消息推送。
6. MiPush模块，在未root手机上使用，不能实现以应用身份发送通知功能，只能实现伪装小米设备功能。
7. 修补后的应用如更新，可下载最新应用安装包，修补后覆盖安装，可保留应用原有的数据不丢失。

## <i id="注册"></i>注册
### <i id="自动注册"></i>1. 自动注册
- 除小米MIUI机型外，其余的需先进行需进行相应伪装，已成功伪装后打开相应的应用进行注册

- 打开目标应用即可自动进行注册，未触发注册可能是伪装不到位或注册异常，对于注册异常的应用，参考如下方式进行解决：
	 - 对于 root 用户，前往注册异常应用的数据目录：/data/data/应用包名/shared_prefs/ ，删除该目录下的mipush.xml文件并重启应用
	 - 对于无 root 用户，需要清空应用数据或卸载重装软件

- 特别注明：
> 1. 全部都显示异常的，可以直接用MT管理器去data/data目录下搜mipush，然后全删，重新注册即可
> 2. 如果还是注册异常，检查一下是否启用了全局广告拦截的软件、模块或者hosts文件，关了重启再试；还是不行，检查/data/data/com.xiaomi.xmsf/shared_prefs 路径下是否有push_message_ids.xml文件生成，没有的话手动新建一个，记得改一下权限与别的文件权限相同

- 无法自动注册的，在推送服务里面点击相应应用的图标进行强制注册

### <i id="强制注册"></i>2. 强制注册
- 具体操作
[![](img/uploadfile/202302/b4961676027252.jpg)](img/uploadfile/202302/b4961676027252.jpg)

- 强制注册成功
[![](img/uploadfile/202302/d97c1676016787.png)](img/uploadfile/202302/d97c1676016787.png)
	 - 如果出现上图情况，基本问题不大，强制停止一次该应用，再打开基本就注册成功了
	 - 注册成功，能不能推是另外一回事（大部分能注册成功就能推送，除了一些奇葩的，比如：酷安、抖音）
	 - 咸鱼为例(框出来的是应用包名，每个应用都不一样)

- 强制注册失败
如果出现下图情况
[![](img/uploadfile/202302/00061676016787.png)](img/uploadfile/202302/00061676016787.png)
	 - Color系统的就没必要关注了，直接去推送服务里面看就行，根据我Color13使用来看，这个提示跟注册成功与否，没有半毛钱关系
	 - 非Color系统,出现这个，大概率就是注册失败

- 如果强制注册还是无法注册的，排查伪装方法及广告拦截、网络权限等

### <i id="注册异常"></i>3. 注册异常
- 注册异常表示应用注册推送失败或者信息异常，遇到这种情况请按照下列顺序排查

- 如果您曾经卸载过push服务或者清空过push数据
	 - 您需要卸载并重新安装之前已经注册过的应用(比如微博，支付宝，淘宝之类的App)
	 - 常见情况就是所有应用都显示注册异常
	 - 所以请勿卸载重装PUSH

- 部分应用特性导致注册异常
	 - 比如酷安开启绿色纯净之后会导致注册异常
	 - 部分应用(如百度地图)在设置中关闭推送，会导致注册异常
	 - 部分应用(如云音乐)关闭后台运行权限后也会主动反注册，这种需要关闭相关优化
	 - 还有情况就是真的只是误报，这种情况请忽略

- 备份恢复过push数据
	 - 如果rom重刷之后恢复之前数据可能是无效的
	 - 如果有这种情况建议按照清空过push数据来处理

- 有的应用显示异常但是可以收到推送
	 - 目前出现在微博的情况比较多，暂时无视注册异常的标记
	 - 这种情况暂时认为是误报，请忽略

### <i id="死活注册不上"></i>4. 死活注册不上
- 查看自己是否使用隐藏应用列表勾选了推送服务将推送服务隐藏了起来

## <i id="配置文件"></i>配置文件
### <i id="配置文件说明"></i>1. 配置文件说明
-  原文：<u>[**`查看`**](https://github.com/NihilityT/MiPushConfigurations "查看")</u>
- 配置文件可以做到以下事情：
	 - 修改通知显示效果
	 - 通知到来时唤醒屏幕
	 - 通知到来时触发通知被点击效果（仅触发效果，未实际点击通知）

1. 配置文件名
配置名根据以下格式命名，共分为四个部分：

`${package}_${appname}_${optional_function}-${exclusive_description}.json`

不同部分的含义分别为：
- 必填：`${package}`，应用包名
    - 使用包名来区分配置针对的是什么应用
    - 特殊包名：
        - `0`，基础配置，必需下载
        - `1`，全局前置配置
        - `2`，全局后置配置
- 必填：`${appname}`，应用名
    - 用于快速识别配置针对的是什么应用
- 可选：`${optional_function}`，功能名
    - 一个应用的配置可以由多份配置文件组成，使用不同的 `${optional_function}` 来标识不同的能力
- 可选：`${exclusive_description}`，互斥能力描述
    - 配置中存在相同 `${optional_function}` 时，说明这一组配置文件为互斥配置，只有一个配置生效，此时应只保留其中一个配置文件
    - 互斥示例（`${optional_function}` = `群消息整形`）：
        - `com.tencent.mobileqq_QQ_群消息整形-群名标题前添加发送者.json`
        - `com.tencent.mobileqq_QQ_群消息整形-群名移动至 subtext.json`

2. 配置使用方式
	 1. 设置配置目录，入口位于：推送服务 - 设置
	 2. 下载所需配置放入该目录中
	 3. （可选）若需自定义通知图标，可以在配置目录下创建icon文件夹，将AndroidNotifyIconAdapt仓库的json文件放入其中
		 - <u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87 "云盘下载")</u>、<u>[**`GitHub下载`**](https://github.com/fankes/AndroidNotifyIconAdapt "GitHub下载")</u>

3. 配置类型
	 配置共分为两类：

- 主配置：具有实际包名的配置项（`${package}_${appname}.json`）

``` json
{
  "version": "0.1.0",
  "configs": {
    "com.coolapk.market": [
      "大图标显示成圆形"
    ]
  }
}

```

- 子配置：提供引用项被主配置引用（`${package}_${appname}_${optional_function}-${exclusive_description}.json`）

``` json
{
  "version": "0.1.0",
  "configs": {
    "大图标显示成圆形": [
      {
        "newMetaInfo": {
          "extra": {
            "__mi_push_round_large_icon": ""
          }
        },
        "stop": false
      }
    ]
  }
}

```

4. 配置执行流程
	 弹出通知时，通过会经过配置进行整行或忽略，执行流程如下：
- 执行 `^` 配置（`1_${appname}.json`）
- 执行应用配置，如 `com.coolapk.market`（`com.coolapk.market_酷安.json`）
- 执行 `$` 配置（`2_${appname}.json`）

在配置执行过程中，若遇到一个匹配成功的配置，其配置中未定义 `"stop": false`，则整个流程结束

### <i id="使用配置文件后无通知"></i>2. 使用配置文件后无通知
- 如果使用配置文件后，会话消息无法通知的话，打开相应软件的添加桌面快捷方式的权限试试
[![](img/uploadfile/202302/d8ce1676971871.jpg)](img/uploadfile/202302/d8ce1676971871.jpg)

### <i id="配置文件起名规范"></i>3. 配置文件起名规范
- 为什么配置文件里面包名不可以用|或其他字符进行连接？
	 - 那样的话就会出现这种情况：
	 > "a|b|c": { 忽略所有消息 }
	 > "a": { 弹出通话消息 }

- 这种情况就不知道应用顺序了，会有问题。

### <i id="配置文件有问题"></i>4. 配置有问题？
- 别人能实现的功能自己不能实现的
	 1. 请更新相应的配置文件（包括基础配置）
	 2. 推送服务和MiPush模块也需要更新到最新内测(在Q群内)

### <i id="配置文件使用介绍(非写法仅使用)"></i>5. 配置文件使用介绍(非写法仅使用)
> - 该篇主要讲解怎么使用配置文件，不介绍配置文件怎么写，有兴趣学习的可以自行了解
> - <u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/Download/MiPushConfigurations/xmsf-v0.3.9+(%E5%8F%AF%E4%BD%BF%E7%94%A8%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)"云盘下载")</u>

##### - <i id="基础配置"></i>基础配置
> PS1：基础配置由原来的一个拆分成了三个
> PS2：不需要懂他是什么原理，直接三个全用就行
> 1. `0_基础配置_工具.json`
> 2. `0_基础配置_开关.json`
> 3. `0_基础配置_消息样式相关.json`

##### - <i id="后置配置"></i>后置配置
- PS1：后置配置由可以理解为全局配置

> 1. `2_后置配置.json`
> 	- PS：使用任何`2_后置配置_xxx`都需要其配合其使用
> 2. `2_后置配置_自动提取意图.json`
> 	- PS1：提取意图即从数据里面提取出 intent[![](img/uploadfile/202307/47181688821935.png)](img/uploadfile/202307/47181688821935.png)
> 	- PS2：直观的效果是通知点击的处理逻辑不走推送服务，可以下拉小窗。
> 	- PS3：与`直接打开意图相`配合：【`直接打开意图是`让通知有可能通过小窗打开，`自动提取意图`是增加这个支持的范围】
> 3. `2_后置配置_移除通知副标题.json`
> 	- PS：如图[![](img/uploadfile/202307/74801688815746.png)](img/uploadfile/202307/74801688815746.png)
> 4. `2_后置配置_只显示一条消息-白名单.json`
> 	- PS：
> 5. `2_后置配置_点击时清理会话通知组-白名单.json`
> 	- PS：如QQ有3-4条MiPush消息，当点开QQ软件时，通知栏将清空QQ的消息
> 6. `2_后置配置_收到消息后台唤醒应用-白名单.json`
> 	- PS：收到消息的APP将拉起处理推送的服务
> 7. `2_后置配置_消息样式为每条消息添加起始标识.json`
> 	- PS：为每个消息的开头添加个`-`
> 8. `2_后置配置_堆叠同一会话的所有通知-白名单.json`
> 	- PS1：原MiPushFramework的按钮功能，新版改为配置文件实现功能
> 	- PS2：
> 9. `2_后置配置_将透传消息作为通知显示-白名单.json`
> 	- PS1：原MiPushFramework的按钮功能，新版改为配置文件实现功能
> 	- PS2：部分消息为透传消息[![](img/uploadfile/202307/16321688817897.png)](img/uploadfile/202307/16321688817897.png)
> 	- PS3：<u>[**`什么是透传消息`**](#什么是透传消息)</u>
> 10. `2_后置配置_屏蔽运营消息-白名单.json`
> 11. `2_后置配置_屏蔽运营消息-黑名单.json`
> 	- PS1：运营消息一般为广告通知，在MiPush中有专门的通知渠道，该配置将会把所有运营消息渠道的通知全部拦截，即MiPush不通知运营消息渠道的消息
> 	- PS2：该配置属于全局拦截，有些APP例如抖音等，运营消息发送的是正常的消息，所以，自行斟酌使用黑/白名单[![](img/uploadfile/202307/861e1688816846.jpg)](img/uploadfile/202307/861e1688816846.jpg)
> 12. `2_后置配置_直接打开意图-白名单.json`
> 13. `2_后置配置_直接打开意图-黑名单.json`
> 	- PS1：直接打开意图可以理解为使用小窗打开该APP
> 	- PS2：部分APP，例如闲鱼等，不使用该配置可能导致消息跳转白屏
> 	- PS3：与`自动提取意图`相配合：【`直接打开意图是`让通知有可能通过小窗打开，`自动提取意图`是增加这个支持的范围】
> 14. `2_后置配置_将标题相同的通知视为同一会话-白名单.json`
> 15. `2_后置配置_将标题相同的通知视为同一会话-黑名单.json`
> 	- PS1：按联系人分别显示最新一条消息（每个联系人或群组单独分组，不成堆，且只显示最新一条）
> 	- PS2：该配置属于全局配置，自行斟酌使用黑/白名单

##### - <i id="应用配置"></i>应用配置
- 1. QQ

> 1. `com.tencent.mobileqq_QQ.json`
> 2. `com.tencent.mobileqq_QQ_MessagingStyle.json`
> 3. `com.tencent.mobileqq_QQ_分离消息样式的个人、群组会话.json`
> 4. `com.tencent.mobileqq_QQ_含“-”符号的群昵称提取首词作为群昵称.json`
> 5. `com.tencent.mobileqq_QQ_群头像支持（不装则显示具体群友头像）.json`
> 	- PS：建议直接拉满

- 2. 飞书

> 1. `com.ss.android.lark_飞书.json`
> 2. `com.ss.android.lark_飞书_MessagingStyle.json`

- 3. 招商银行

> 1. `cmb.pb_招商银行.json`
> 2. `cmb.pb_招商银行_卡号替换.json`

- 4. 其他

> 1. `com.xingin.xhs_小红书.json`
> 2. `com.zhihu.android_知乎.json`
> 3. `com.taobao.idlefish_闲鱼.json`
> 4. `com.jingyao.easybike_哈啰.json`
> 5. `com.ss.android.ugc.aweme_抖音.json`
> 6. `com.alibaba.android.rimet_钉钉.json`

##### - <i id="群友配置"></i>群友配置

> 1. `1_全局单个应用通知成组.json`
> 2. `com.sina.weibo_微博.json`
> 3. `com.jingdong.app.mall_京东.json`
> 4. `com.tencent.wework_企业微信.json`
> 5. `com.autonavi.minimap_高德地图.json`
> 6. `com.eg.android.AlipayGphone_支付宝.json`

PS:待继续完善

## <i id="通知"></i>通知
### <i id="通知为什么没有头像"></i>1. 通知为什么没有头像
- 没有相应参数的时候就是用首字作为头像

### <i id="为什么我没有推送服务几个字"></i>2. 为什么我没有推送服务几个字
[![](img/uploadfile/202302/ff041676027895.png)](img/uploadfile/202302/ff041676027895.png)
- 没用MiPush模块勾选系统框架且能推送才这么显示

### <i id="通知渠道"></i>3. 通知渠道
- 只有推送过相关消息才有，不是注册完就有了，下图QQ为例
[![](img/uploadfile/202302/69a21676186811.jpg)](img/uploadfile/202302/69a21676186811.jpg)

### <i id="铃声/悬浮通知"></i>4. 铃声/悬浮通知
- 怎么开启悬浮通知(QQ群聊消息为例)
[![](img/uploadfile/202302/d3c11676187466.jpg)](img/uploadfile/202302/d3c11676187466.jpg)

- 操作方法
[![](img/uploadfile/202302/faad1676202329.jpg)](img/uploadfile/202302/faad1676202329.jpg)

### <i id="无法收到推送"></i>5. 无法收到推送
- 如果您无法收到任何推送，请按以下方法排查：
	 - 服务已启动（通知栏有运行中的通知）
	 - 事件页中有应用注册和收到通知的记录
	 - 请不要对推送服务进行优化，包括电池优化、MyAndroidTools、绿色守护、黑域 等优化措施，请确保推送服务未优化或在白名单中
	 - 如果您的设备启用了代理工具，请设置绕行推送服务，对推送服务启用代理可能会无法收到推送。
	 - 保证推送服务在后台运行
		 - <u>[**`保活操作`**](#自启动/保活 "保活操作")</u>

### <i id="丢失的不重要通知栏"></i>6. 丢失的不重要通知栏
- 缘由：
	 - 经过某些不知名的神奇操作后，可能会丢失不重要通知栏

- 起因：
	 1. MIUI会折叠一些推送消息，归类为不重要通知
	 2. 但是根据缘由来看，消息就 ≈ 无了
	 3. 所以要全部设置通知为重要
[![](img/uploadfile/202302/9fdd1676971050.jpg)](img/uploadfile/202302/9fdd1676971050.jpg)

- 操作：
	 1. 通知过滤规则设成重要
		 - 否则有推送消息记录，通知栏因缘由被吞
		 - 把不重要消息的收纳栏显示出来了
	 - 如果默认还不出现在通知栏。去MIUI原生通知图标那可以开启收纳栏
[![](img/uploadfile/202302/13651676970319.jpg)](img/uploadfile/202302/13651676970319.jpg)

### <i id="如何解决QQ只显示最新一条消息"></i>7. 如何解决QQ只显示最新一条消息
- **注意：下面两种办法不要同时使用，可能会有bug**
1. 推荐配置文件
	 - <u>[**`应用配置`**](#应用配置)</u>中的关于QQ的拉满，简单粗暴
2. 不推荐堆叠开关
	 - 在推送服务里面设置QQ堆叠(新版已将开关改为配置文件)
[![](img/uploadfile/202302/2a9a1676984515.png)](img/uploadfile/202302/2a9a1676984515.png)

### <i id="通知上的按钮"></i>8. 通知上的按钮
- 如果遇到下图中类似的按钮，且不喜欢的，请换用release版本
- 推送服务v0.3.9+，该功能做成开关了，默认关闭的
[![](img/uploadfile/202302/e6ca1675765006.jpg)](img/uploadfile/202302/e6ca1675765006.jpg)
[![](img/uploadfile/202302/2a611675765006.jpg)](img/uploadfile/202302/2a611675765006.jpg)

### <i id="关闭-推送服务运行中"></i>9. 关闭-推送服务运行中
- 进入推送服务的系统通知设置界面，关闭status组通知，强行停止推送服务
[![](img/uploadfile/202303/d0131679626729.jpg)](img/uploadfile/202303/d0131679626729.jpg)

### <i id="通知数量"></i>10. 通知数量
- 每个 APP 的通知同时显示数量是有上限的，可能是50，可能是24。
- 开了会话堆叠后，消息显示数量可能会暴涨，可能会导致推送服务在发出 50/24 条消息后再也发不出消息
- 因此没有经常看消息清消息的习惯的话，需要斟酌一下这个功能要不要使用。
- 注：MIUI 无此限制，会自动清理旧通知。

### <i id="运营消息(广告)过滤"></i>11. 运营消息(广告)过滤
- 根据<u>[**`后置配置`**](#后置配置)</u>中的屏蔽运营消息.json来使用

## <i id="通知图标"></i>通知图标
### <i id="MIUI通知图标"></i>1. MIUI通知图标
- 云盘下载MiPush模块，安装后勾选推荐应用并重启系统
- 对于不需要MiPush模块的人：<u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87/MIUI%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87 "云盘下载")</u>
	 1. 云盘下载MIUI专用-MIUI 原生通知图标，安装后在LSPosed中启用
	 2. 在安装的 APP 中启用通知栏中的图标强制显示为 APP 图标、启用通知图标优化名单自动更新

### <i id="自定义通知图标"></i>2. 自定义通知图标
- 对MIUI_CN和Flyme无作用
1. 安装推送服务最新版
2. 在配置目录下面创一个**icon**文件夹，把图标**.json**放进去;
3. 图标下载：<u>[**`GitHub下载`**](https://github.com/fankes/AndroidNotifyIconAdapt/blob/main/APP/NotifyIconsSupportConfig.json "下载")</u>、<u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87/%E8%87%AA%E5%AE%9A%E4%B9%89%E9%80%9A%E7%9F%A5%E5%9B%BE%E6%A0%87 "下载")</u>
4. 强行停止推送服务

## <i id="冲突"></i>冲突
### <i id="反复/多次重启"></i>1. 反复/多次重启
- 如果精简掉小米服务框架后，出现不断重启的情况，排查小爱同学
- 可选操作如下：
	 1. 把小爱同学也精简掉
	 2. 不使用语音唤醒
	 3. 使用语音唤醒，但需使用自定义唤醒词

### <i id="升级系统掉注册"></i>2. 升级系统掉注册
- 仅MIUI更新系统会掉推送服务的注册
	 - 原因：由于MIUI系统自带米推，更新系统时候需要关闭精简，所以注册文件大概率会被恢复成官方的导致掉注册
- 其他系统没有影响

### <i id="通知类软件"></i>3. 通知类软件
1. 目前已知，`通知滤盒`的ai功能会吃部分mipush消息，例如支付宝，酷安等
	- 解决办法，关闭ai过滤即可，该问题已反馈给滤盒作者，暂未回复

## <i id="权限"></i>权限
### <i id="修改权限"></i>1. 修改权限
[![](img/uploadfile/202302/50cd1676984120.png)](img/uploadfile/202302/50cd1676984120.png)

- 该目录里面的所有开关，无特殊需求，默认即可，无需开关
- 如果非要开关，除非你明白它的含义
- 优化请使用配置文件

### <i id="配置目录闪弹"></i>2. 配置目录闪弹
- 排查1：是不是把`文件`这个应用冻结或精简了，是的，请恢复
- 排查2：是不是把`外部存储设备`这个应用冻结或精简了，是的，请恢复

### <i id="电池优化"></i>3. 电池优化
- 可以考虑是否用了接管电池白名单的软件或模块
- 如果使用的是NoActive
> 1. 推送服务：用户应用
> 	 - 请将推送服务的白名单开关打开
> 2. 推送服务：系统应用
> 	 - 请将com.xiaomi.xmsf添加到/data/system/NoActive/whiteApp.conf名单里面

### <i id="情景模式(自启动/保活/重启)"></i>4. 情景模式(自启动/保活/重启)
- 所需应用-Thanox：<u>[**`酷安下载`**](https://www.coolapk.com/apk/github.tornaco.android.thanos "酷安下载")</u>
- 开机自启
> - 如果你的系统，无法在手机开机的时候，自动启动推送服务，可以尝试下面的情景模式
> - 缺点：手机打盹得时候，可能不会执行，最好把时间设置到睡觉时间之外得其它时间，或者你能让打盹不影响灭霸也行
> - 全局变量：boot_normal
> - 选取应用：推送服务
```
[
    {
        "name": "开机自启",
        "description": "启动mipush",
        "priority": 1,
        "condition": " systemReady == true",
        "delay": 3000,
        "actions": [
            "to_start = globalVarOf$boot_normal ;foreach (pkn : to_start) {activity.launchProcessForPackage(pkn);};"
        ]
    }
]
```

- 保活
> - 如果你的系统，经常杀死推送服务，可以尝试下面的情景模式
> - 全局变量：baohuo
> - 选取应用：推送服务
```
[
{
"name": "应用保活",
"description": "应用被杀死则启动其进程",
"priority": 1,
"condition": "pkgKilled == true && globalVarOf$baohuo.contains(pkgName)",
"delay": 1000,
"actions": [
"activity.launchProcessForPackage(pkgName)",
"ui.showShortToast(\"启动了:\" + pkgName)"
]
}
]
```

- 重启
> - 如果你的推送服务需要重启才能收到推送，可以尝试下面的情景模式
> - 作者：诗里沧海怨怼
> - 全局变量：timedApp
> - 使用方法：
> 	 1. 将该情景模式使用JSON粘贴好后，设置好全局变量及应用
> 	 2. 选择右上角的三个点，找到引擎，选择时间与日期
> 	 3. 左下角是定时时间，右下角是间隔时间，Tag写tmieKill，至于时间随你自己选了
> 		 - 定时时间想要设置多个，就需要改情景模式，将【"condition": "timeTick&&tag=='timeKill'",】改成【"condition": "timeTick&&(tag=='timeKill1' || tag=='timeKill2')",】如果还想要更多定时时间，则将右小括号前继续增加【 || tag=='timeKill？'】其中问好就是代表数字，自己往后加就行了，【||前有个空格，不要遗漏】
> 		 - 间隔时间就不需要改情景模式，直接在Tag写tmieKill就行
```
[
    {
        "name": "到点重启",
        "description": "by诗里沧海怨怼",
        "priority": -1,
        "condition": "timeTick&&tag=='timeKill'",
        "actions":["aMan=thanos.activityManager;for(app:globalVarOf$timedApp){if(aMan.isPackageRunning(app)&&!activity.frontAppPackage.equals(app)){aMan.forceStopPackage(app);}}Thread.sleep(5000);ui.showShortToast('到点重启');for(app:globalVarOf$timedApp){if(!aMan.isPackageRunning(app)){activity.launchProcessForPackage(app);}}"]
    }
]
```

### <i id="权限问题"></i>5. 权限问题
- 关于终端
> 1. Termux终端：<u>[**`官网`**](https://termux.dev/cn/index.html "官方")</u>
> 2. MT管理器：<u>[**`官网`**](https://mt2.cn/ "官网")</u>

- 冰箱解冻失败失败：
	 1. 检查推送服务的高级配置是否打开了冰箱解冻支持的开关；
	 2. 检查是否把冰箱加入了墓碑冻结名单；
	 3. 前两项无问题，重新打开冰箱解冻支持的开关；
	 4. 还是无法赋权，可尝试手动赋权，命令：
```
pm grant com.xiaomi.xmsf com.catchingnow.icebox.SDK
```

- 无法拉起QQ电话：
	 1. 给推送服务显示在其他应用上层的权限；
	 2. 加载配置文件。

- 安卓13卡使用情况权限界面。
	 1. 打开终端,输入su回车
	 - 授予使用情况访问权限
```
appops set com.xiaomi.xmsf android:get_usage_stats allow
```
	 - 授予显示在其他应用上层权限
```
appops set com.xiaomi.xmsf SYSTEM_ALERT_WINDOW allow
```

- 推送服务双开
：PS:指令仅在MIUI上通过测试
	 1. 打开终端,输入su回车
	 - 授予使用情况访问权限
```
appops set --user 999 com.xiaomi.xmsf android:get_usage_stats allow
```
	 - 授予显示在其他应用上层权限
```
appops set --user 999 com.xiaomi.xmsf SYSTEM_ALERT_WINDOW allow
```

## <i id="常用应用问题解决"></i>常用应用问题解决
### <i id="QQ注册"></i>1-1. QQ注册
- <u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/Download/MiPush "云盘下载")</u>
1. 安装MiPush、推送通知
2. 前往LSPosed中，勾选推荐作用域、QQ
3. 重启手机
4. 打开推送通知，查看QQ是使用系统推送，但尚未注册，还是注册失败
	 - 使用系统推送，但尚未注册
		 - 前往LSPosed中，取消勾选QQ，接着重启QQ，再勾选QQ重启QQ,这时再看QQ是什么注册情况，如果依旧尚未注册，可以尝试重启手机，再重复一次此操作
		 - 若一直为尚未注册，可以试试<u>[**`强制注册`**](#强制注册)</u>
		 - 则尝试使用MiPushDeviceFake或MiPushfaker，倘若变成注册失败，请看下面
	 - 注册失败
		 [![](img/uploadfile/202302/bb401675496609.png)](img/uploadfile/202302/bb401675496609.png)

### <i id="QQ注册-不推送"></i>1-2. QQ注册-不推送
- 注册成功后，需要重启注册应用，切记
1. 每次使用完QQ后，划卡QQ
2. 使用冰箱，将QQ冻结
3. 使用墓碑，来迫使QQ走推送通道
> - 如果使用墓碑后QQ推送，仍不走MiPush，可以使用Thanox或者Scene来配合
> 1. 使用`Thanox`情景模式中Process trim(可从`Thanox`的示例中导入)，然后添加全局变量：process_trim_list，并添加QQ的推送服务MSF，如图所示
> [![](img/uploadfile/202302/ecad1675932698.jpg)](img/uploadfile/202302/ecad1675932698.jpg)
> 2. 使用`Scene`的应用偏见，直接选择QQ即可

### <i id="QQ无法接通QQ推送电话"></i>1-3. QQ无法接通QQ推送电话
- 能收到正常推送消息，但收不到QQ推送电话的消息
	 - QQ版本问题，建议换8.9.18版本，测试推送后再覆盖安装回别的版本也能保持QQ电话推送）

- 能收到QQ电话推送，但是拉不起QQ；
> 1. 需排查是否加载了配置，如果成功加载了配置文件
> 2. 再排除是否给推送服务显示在其他应用上层的权限，如果无法给权限，终端命令赋权：
```
> su
> appops set com.xiaomi.xmsf SYSTEM_ALERT_WINDOW allow
```
> 3. 还有处于miui的QQ分身界面，有电话推送时也不会拉到QQ主应用
> 4. 位于QQ分身界面，也不会拉起主应用QQ

- 收到QQ电话推送且能拉起QQ，但是不能接通
> 1. 排查是否杀了QQ的msf进程，这个影响接通速度
> 2. 排查QQ的冷启动速度，QQ刚安装，没编译完成，冷启动太慢也会影响接通，解决办法就是手动给QQ编译，无法手动编译就杀死打开QQ多重复几轮，QQ就被系统编译了
> 3. 排查是否登着电脑QQ（电脑版tim不影响），这个会导致QQ无法接通

- 亮屏时能拉起QQ电话和接通电话，但是息屏状态时，能点亮屏幕且拉起QQ，但是无法弹出QQ电话那个界面
	 - justpush模块的影响，关掉重启就可以了。

### <i id="QQ下载"></i>1-4. QQ下载
- QQ下载的文件一般位于/storage/emulated/0/Android/data/ com.tencent.mobileqq/Tencent/QQfile_recv/
- 非root的情况可以通过<u>[**`保存副本`**](https://play.google.com/store/apps/details?id=app.rikka.savecopy)</u>复制一份到下载目录
- root也可以用<u>[**`QAuxiliary`**](https://github.com/cinit/QAuxiliary "QAuxiliary")</u>或者其他重定向至下载目录。

### <i id="酷安注册-不推送"></i>2-1. 酷安注册-不推送
- <u>[**`云盘下载`**](https://cloud.bzmshang.top/Software/MiPush/%E9%85%B7%E5%AE%89 "云盘下载")</u>
- MIUI
	 1. 卸载酷安，安装官方酷安12.4.2版
	 2. 先不要启动酷安
	 3. 对酷安隐藏root后再启动酷安进行注册推送和登录帐号

- 非MIUI（方法1）
	 1. 卸载酷安，安装官方酷安12.4.2版
	 2. 先不要启动酷安
	 3. 安装MiPushDeviceFake和mipush模块，两个模块都勾上酷安来进行伪装，然后再打开酷安进行注册登录

- 非MIUI(方法2)
	 1. 卸载当前酷安，安装13.0.1内置MiPushDeviceFake版的酷安
	 2. 先不要启动酷安
	 3. 安装MiPush模块，模块勾选酷安。
	 4. 然后再打开推送服务来强制注册

	- Tip1：酷安注册很容易，难就难在注册成功不推送，所以，使用内置版的13.0.1酷安注册成功后，划卡酷安，测试推送，如果刚注册完，划卡推送没用，重启手机并打开酷安，再次测试
	- Tip2：如果不行，也对酷安隐藏root后再启动酷安进行注册推送
	- Tip3：(需要ROOT)如果你推送没有问题了，且不喜欢修改的酷安，可以通过核心破解并打开图中功能，进行酷安版本切换
[![](img/uploadfile/202302/56f01676008356.jpg)](img/uploadfile/202302/56f01676008356.jpg)

### <i id="抖音注册-推送"></i>3-1. 抖音注册-推送
- 方法由群友<u>[wyj809](http://www.coolapk.com/u/21035894 "wyj809")</u>提出
- 使用MiPush模块对抖音进行伪装，抖音使用小米商店的版本（可以从这查看下载办法<u>[**`应用下载`**](#应用下载 "应用下载")</u>）
- 一般当天注册成功，第二天就可以推送了

## <i id="其他教程"></i>其他教程
### <i id="本机网络adb调试、Lspatch使用方法"></i>1. 本机网络adb调试、Lspatch使用方法
- 作者：oekkai
- 内容：<u>[**`查看`**](https://www.coolapk.com/feed/38114209?shareKey=MDc5YTgyZjI3ZmZhNjNjZTJlZmI~&shareFrom=com.coolapk.market_13.0.1 "查看")</u>

### <i id="免root安装hmspush享受hms推送教程"></i>2. 免root安装hmspush享受hms推送教程
- 作者：justxiami
- 内容：<u>[**`查看`**](https://www.coolapk.com/feed/38745542?shareKey=ODAwYjMxYTI4MjJkNjNjZTQwOTM~&shareUid=21035894&shareFrom=com.coolapk.market_13.0.1 "查看")</u>

### <i id="小米机型MIUI系统视频教程"></i>3. 小米机型MIUI系统视频教程
- 作者：极客无极
- 内容：<u>[**`查看`**](https://b23.tv/6vqqYWs "查看")</u>

### <i id="免root解决微信qq签名不一致的问题"></i>4. 免root解决微信qq签名不一致的问题
- 作者：本_为_以
- 内容：<u>[**`查看`**](https://www.coolapk.com/feed/45863471?shareKey=NWNlNzg0ZDdmM2VhNjQ1OWQ5YTM~&shareUid=7095588&shareFrom "查看")</u>