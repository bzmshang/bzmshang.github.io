---
title: Thanox（情景模式）
permalink: Thanox_SceneModel.html
date: 2022-09-10
tags:
banner_img: img/uploadfile/202209/5a3e1663656468.jpg
index_img: img/uploadfile/202209/5a3e1663656468.jpg
---

# <i id="情景模式版本"></i>情景模式版本
```
ver=v0.3.0

latest-update=2023.11.11

以下内容全文为Thanox的情景模式收集，请告诉给有需要的人

```

## <i id="声明"></i>声明
> 1. 下面所有的情景模式皆为 JSON 格式
> 2. 全局变量操作方式
> 	1. 在情景模式中，点开右上角三个点
> 	2. 找到全局变量并点开后，点击右下角的添加
> 	3. 先在顶部输入框中输入全局变量名，然后点开右上角三个点
> 	4. 选取应用完成后，点击右下角的完成，再点击右下角的保存，即可
> 3. SU操作方式
> 	1. 在情景模式中，点开右上角三个点
> 	2. 找到引擎并点开，打开su API的开关后
> 	3. 给予Thanox自启动权限，即可
> 4. 如果发现有失效的情景模式，也请酷安反馈下
>
> - 更多：(加群，教程，文件)
> 	 - 手机版：请点击左上角三条杠查看更多
> 	 - 电脑版：请看顶部导航栏

# <i id="实用情景模式"></i>实用情景模式
## <i id="1"></i>1. 清理后台
### <i id="1-1"></i>1.1 划卡即清
- 注意事项
> 1. 非常适用于Color系统移除Athena（雅典娜）后，划卡无法清理后台
> 2. 全局变量为：HUAKA（将所有APP都加入全局变量即可）

```
[
    {
    "name": "划卡即清",
    "description": "划掉任务卡片直接杀死后台，全局变量HUAKA",
    "priority": 1,
    "condition": "taskRemoved == true && globalVarOf$HUAKA.contains(pkgName) ",
    "actions": [
    "killer.killPackage(pkgName);"
            ]
    }
]
```

### <i id="1-2"></i>1.2 自动划掉最近任务中没有运行的应用
- 注意事项
> 1. 即食即用

```
[
{
"name": "自动划卡",
"description": "自动划掉最近任务中没有运行的应用",
"priority": 1,
"condition": "pkgKilled == true",
"delay": 2000,
"actions": [ 
"if(thanos.activityManager.isPackageRunning(pkgName) == false && task.hasTaskFromPackage(pkgName) == true){task.removeTasksForPackage(pkgName) ;}"
]
}
]
```

### <i id="1-3"></i>1.3 返回桌面移除任务卡片
- <u>[**`原帖`**](https://www.coolapk.com/feed/45419920?shareKey=NmEzNjg2ODU1Y2YxNjQ0NGU5MDc~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "返回桌面移除任务卡片",
"description": "返回桌面(变量HomeApp)移除全局变量Task列表中的应用任务卡片",
"priority": 1,
"condition": " frontPkgChanged == true && globalVarOf$HomeApp.contains(to) && globalVarOf$Task.contains(from) && task.hasTaskFromPackage(from)",
"actions": [
"task.removeTasksForPackage(from)",
"ui.showShortToast(\"🌪️移除\" + thanos.pkgManager.getAppInfo(from).appLabel +\"任务🌪️\")"
]
}
] 
```

### <i id="1-4"></i>1.4 息屏关数据，清理后台
- <u>[**`原帖`**](https://www.coolapk.com/feed/35805896?shareKey=MGU1YjZlMjcwZThlNjQ0ODhjZTI~&shareUid=2521387&shareFrom "**`原帖`**")</u>

```
[
{
"name": "息屏关数据，清理后台",
"description": "息屏5分钟后再判断是否息屏，若息屏则关数据，清理后台，300000毫秒=300秒=5分钟，可自行修改，酷安@梦霓华裳 ",
"priority": 1,
"condition": "screenOff == true",
"delay": 300000,
"actions": [
"if(screenOn == false){data.setDataEnabled(false)}",
"if(screenOn == false){clearBackgroundTasks()}"
]
}
]
```

### <i id="1-5"></i>1.5 返回桌面或灭屏清理后台
- NoActive群友提供

```
[
{
"name": "返回桌面或灭屏清理后台",
"description": "返回桌面或灭屏清理全局变量BKILL列表中的应用",
"priority": 2,
"condition": "frontPkgChanged == true && to == \"com.miui.home\" || screenOff == true",
"actions": [
"for (String s : globalVarOf$BKILL){if( task.hasTaskFromPackage(s)){ui.showShortToast(\"关闭\" +s);killer.killPackage(s);ui.showShortToast(\"移除\" +s);task.removeTasksForPackage(s)}};"
]
}
]
```

### <i id="1-6"></i>1.6 5分钟未前台自动杀后台

```
[
{
"name": "5分钟未前台自动杀后台",
"description": "挂后台杀应用，应用名单来自全局变BackStill，5分钟之后杀死",
"priority": 1,
"condition": "frontPkgChanged == true && globalVarOf$BackStill.contains(from) ",
"actions": [
"Thread.currentThread().sleep(300000);",
"if(activity.getFrontAppPackage() != from)[ui.showShortToast(\"杀后台\"+from)];",
"if(activity.getFrontAppPackage() != from)[killer.killPackage(from)];"
]
}
]
```

### <i id="1-7"></i>1.7 移除通知杀后台
- <u>[**`原帖`**](https://www.coolapk.com/feed/22576717?shareKey=ZjgwNzRhYjg4ODgxNjQ0MjJiMDE~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "移除通知杀后台",
"description": "移除通知杀应用，应用名单来自全局变量notikill。酷安@幻觉",
"priority": 1,
"condition": "notificationRemoved == true && globalVarOf$notikill.contains(pkgName)",
"actions": [
"killer.killPackage(pkgName);"
]
}
]
```

### <i id="1-8"></i>1.8 移除通知杀后台优化版
- 作者：<u>[**`苹果技术开发人员`**](http://www.coolapk.com/u/20080849 "苹果技术开发人员")</u>
- 优化：前台应用推送消息时清除通知不杀应用

```
[
{
"name": "移除通知杀后台",
"description": "移除通知杀应用，应用名单来自全局变量notikill,前台应用推送消息时清除通知不杀应用。酷安@幻觉",
"priority": 1,
"condition": "notificationRemoved == true && globalVarOf$notikill.contains(pkgName) && activity.getFrontAppPackage()!=pkgName",
"actions": [
"killer.killPackage(pkgName);"
]
}
]
```

### <i id="1-9"></i>1.9 应用被杀死，则移除卡片
- <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=NzQ1YTJkMWQwZTk2NjRjNjNjZTM~&shareUid=3119350&shareFrom "**`原帖`**")</u>
- 应用被后台杀死，记录被杀死的应用和被杀死的时间到日志文件里。该情景模式不记录你自己划走卡片杀死的应用，只记录在最近任务里被后台杀死的应用。如何查看？日志文件路径"/data/system/thanos_ynokiMHEVHZWWdDH/profile_user_io/kill/killLog.txt"，打开日志文件即可查看。代码如下，请用情景模式json编写。
```
[
{
"name": "应用被杀死，则移除卡片",
"description": "应用被后台杀死时，移除卡片，历史写入日志",
"priority": 1,
"condition": "pkgKilled == true && globalVarOf$s_app.contains(pkgName) && task.hasTaskFromPackage(pkgName)",
"actions": [
"task.removeTasksForPackage(pkgName);",
"io.writeAppend(\"kill/killLog.txt\",pkgName+\"应用被后台杀死，移除卡片，移除时间:\"+ new java.util.Date() +\"\\r\")"
]
}
]
```

### <i id="1-10"></i>1.10 开机工作
- <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=NzQ1YTJkMWQwZTk2NjRjNjNjZTM~&shareUid=3119350&shareFrom "**`原帖`**")</u>
- 开机划掉没有运行的卡片，并且删除旧日志，同时后台开启一些需要自启动的应用。使用方法，全局变量: all_app 是开机后需要在最近任务划掉的应用集; ziqidong 是开机需要后台启动的应用集。代码如下:

```
[
{
"name": "开机工作",
"description": "开机自启动一些程序，来自变量名ziqidong，划掉除去变量all_app的卡片，此变量除去了微信，删除一些日志",
"priority": 3,
"condition": "systemReady == true",
"delay": 1000,
"actions": [
"foreach (pkn : globalVarOf$all_app){task.removeTasksForPackage(pkn);}",
"ui.showShortToast(\"清理卡片完成\")",
"foreach (zq : globalVarOf$ziqidong){activity.launchProcessForPackage(zq)}",
"ui.showShortToast(\"自启动应用完成\")",
"io.write(\"kill/killLog.txt\", \"本次启动时间:\" + new java.util.Date() + \"\\r\")",
"ui.showShortToast(\"重置日志完成\")"
]
}
]
```

### <i id="1-11"></i>1.11 游戏时清理部分后台
- <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=NzQ1YTJkMWQwZTk2NjRjNjNjZTM~&shareUid=3119350&shareFrom "**`原帖`**")</u>
- 游戏时清理一些顽固后台，变量notNitian_apps放需要被杀的应用，注意该应用如果在最近任务，将不会被杀。变量qlapps是你要打卡的游戏应用或其他应用。代码如下:

```
[
{
"name": "游戏时清理部分后台",
"description": "游戏时清理部分不在卡片的后台",
"priority": 2,
"condition": "frontPkgChanged=true && globalVarOf$qlapps.contains(to)",
"actions": [
"ui.showShortToast(\"清理一些后台中\");",
"foreach (pkn : globalVarOf$notNitian_apps) {if (su.exe('pidof ' + pkn).out != [] && ! task.hasTaskFromPackage(pkn)) {su.exe(\"kill -15 $(ps -ATf | grep \" + pkn + \" | grep -v grep | awk '{print $2}' | sort | uniq)\");};}"
]
}
]
```

### <i id="1-12"></i>1.12 应用被杀，自动复活
- 来自酷友提供<u>[**`eriswu`**](http://www.coolapk.com/u/21207768 "**`eriswu`**")</u>

```
[
{
"name": "All Start app process",
"description": "检测到应用被杀死时，启动该应用进程,全局变量JCFH，作者eriswu提供代码",
"priority": 1,
"condition": "pkgKilled == true && globalVarOf$JCFH.contains(pkgName)",
"delay": 5000,
"actions": [
"ui.showShortToast(\"Process resurrection👻\");",
"activity.launchProcessForPackage(pkgName)"
]
}
]
```

## <i id="2"></i>2. 保活
### <i id="2-1"></i>2.1 应用保活(方案1)
- 注意事项
> 1. 把需要保活的应用放在 全局变量里，当应用被杀死后，该应用就会重新启动
> 2. 全局变量为：baohuo
> 3. 作者：默认2234077 <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=ZjUxZmQyOTExYjdjNjMxYzdmMDU~&shareUid=3571197&shareFrom "原帖")</u>

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

### <i id="2-2"></i>2.2 应用保活(方案2)
- <u>[**`原帖`**](https://www.coolapk.com/feed/39717161?shareKey=M2ZmZmI5M2ZmZWQxNjQ0MzU4OTA~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "应用保活",
"description": "拉起主进程(但是有时候无法拉起特定服务,或者其子进程)",
"priority": 1,
"condition": "pkgKilled",
"actions": [
"if(thanos.getProfileManager().isGlobalRuleVarByNameExists(\"keepAlive\")){if(globalVarOf$keepAlive.size()==0){ui.showShortToast(\"请为全局变量添加内容\")}else{foreach(proc:globalVarOf$keepAlive){if(!thanos.getActivityManager().isPackageRunning(proc)){activity.launchProcessForPackage(proc);Thread.sleep(6000);if(thanos.getActivityManager().hasRunningServiceForPackage(proc)){ui.showShortToast(\"拉起\"+proc);}else{ui.showLongToast(\"请手动打开:\"+proc);}}}}}else{thanos.getProfileManager().disableRuleByName(\"应用保活\");thanos.getProfileManager().addGlobalRuleVar(\"keepAlive\",new String[0]);ui.showShortToast(\"全局变量不存在!\");}"
]
}
]
```

### <i id="2-3"></i>2.3 保活APP
- <u>[**`原帖`**](https://www.coolapk.com/feed/45419920?shareKey=NmEzNjg2ODU1Y2YxNjQ0NGU5MDc~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "保活APP",
"description": "APP被杀死时，重新启动，全局变量keepAlive",
"priority": 1,
"condition": "pkgKilled == true && globalVarOf$keepAlive.contains(pkgName)",
"actions": [
"activity.launchProcessForPackage(pkgName)",
"if(thanos.activityManager.hasRunningServiceForPackage(pkgName)){ui.showShortToast(\"👻\" + thanos.pkgManager.getAppInfo(pkgName).appLabel +\"启动成功\")}else{activity.launchProcessForPackage(pkgName)}"
]
}
]
```



### <i id="2-4"></i>2.4 微信保活
- <u>[**`原帖`**](https://www.coolapk.com/feed/45338524?shareKey=MzA4ZTQ5ZDdmMWI0NjQ0MzRkZjU~&shareUid=3571197&shareFrom "原帖")</u>

```
[
{
"name": "微信保活",
"description": "微信被杀死时，启动微信进程",
"priority": 1,
"condition": "pkgKilled == true && pkgName==\"com.tencent.mm\"",
"actions": [
"ui.showShortToast(\"保活微信启动\");",
"activity.launchProcessForPackage(\"com.tencent.mm\")"
]
}
]
```

### <i id="2-5"></i>2.5 QQ保活
- <u>[**`原帖`**](https://www.coolapk.com/feed/35808050?shareKey=ZTczOTFmYWE5Y2ZkNjQ0MzViMWQ~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "QQ保活",
"description": "QQ被杀保活",
"priority": 1,
"condition": "pkgKilled == true && (pkgName == \"com.tencent.mobileqq\")",
"actions":
[
"su.exe('am start-foreground-service com.tencent.mobileqq/com.tencent.mobileqq.msf.service.MsfService');",

"ui.showShortToast(\"[已启动]\")"
]
}
]
```
### <i id="2-6"></i>2.6 无障碍保活

```
[
{
"name": "无障碍保活",
"description": "保活应用，启动无障碍 全局变量wza",
"priority": 2,
"condition": "pkgKilled == true && globalVarOf$wza.contains(pkgName)",
"delay": 2000,
"actions": [
"su.exe('settings put secure enabled_accessibility_services 无障碍服务名1:服务名2:服务名3');",
"ui.showShortToast(\"[无障碍已启动]\")"
]
}
]
```

### <i id="2-7"></i>2.7 一键启动很多应用
- <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=NzQ1YTJkMWQwZTk2NjRjNjNjZTM~&shareUid=3119350&shareFrom "**`原帖`**")</u>
- 一键启动很多很多应用，每隔4秒启动一个应用，把需要启动的应用放在变量 hd 里面，新建一个qdyy 的快捷应用连接，快捷名字随意。代码如下:

```
[
{
"name": "一键启动很多应用",
"description": "监听一个快捷方式启动事件",
"priority": -1,
"condition": "shortcutLaunched == true && shortcutValue == \"qdyy\"",
"actions": [
"foreach (pkn : globalVarOf$hd){ui.showShortToast(\"启动中\");Thread.sleep(4000);activity.launchMainActivityForPackage(pkn)}"
]
}
]
```

## <i id="3"></i>3. 定位
### <i id="3-1"></i>3.1 自动GPS
- <u>[**`原帖`**](https://www.coolapk.com/feed/45564367?shareKey=Mjc5YTBlMDU1YjE0NjQ0YjZjNDk~&shareUid=3044819&shareFrom "原帖")</u>
- 全局变量分两个
	 - GPSApp
	 1. 只写包名：进入应用打开GPS，回到桌面关闭GPS
	 2. 包名后面+空格+#，例如微信：【com.tencent.mm #】则当微信被杀死才关闭GPS
	 - GPSUI
		 - 填写：目标界面组件名(就是Activity)
- 须知：如果退出前台GPS应用，就会轮询是否有后台GPS应用在运行，如果有就不会关闭，没有就关闭，UI同理

```
[

    {

        "name": "Auto GPS",

        "description": "by诗里沧海怨怼 | 打开指定应用,打开GPS;[关闭|切换]指定应用,关闭GPS",

        "priority": 1,

        "condition": "if(pkgKilled){curs=su.exe(\"dumpsys activity activities|grep ResumedActivity|awk '{print $3}'\").out[0].split('/');if(curs[1].charAt(curs[1].length()-1)=='}'){curs[1]=curs[1].substring(0,curs[1].length()-1);}for(acti:globalVarOf$GPSUI){if(acti!=null&&acti.startsWith(curs[0])&&acti.endsWith(curs[1])){return;}}for(app:globalVarOf$GPSApp){if(app!=null&&!app.startsWith(pkgName)){if(app.startsWith(curs[0])||app.charAt(app.length()-1)=='#'&&thanos.activityManager.isPackageRunning(app.split(' ')[0])){return;}}}if(hw.isLocationEnabled()){su.exe('settings put secure location_mode 0');}}else if(activityResumed){pName=componentNameAsString.split('/')[0];gapps=globalVarOf$GPSApp ;if(gapps.contains(pName)){if(!hw.isLocationEnabled()){su.exe('settings put secure location_mode 3');}}else if(globalVarOf$GPSUI.contains(componentNameAsString)){if(!hw.isLocationEnabled()){su.exe('settings put secure location_mode 3');}}else{for(app:gapps){if(app!=null){if(app.charAt(app.length()-1)=='#'&&thanos.activityManager.isPackageRunning(app.split(' ')[0])){if(!hw.isLocationEnabled()){su.exe('settings put secure location_mode 3');}return;}}}if(hw.isLocationEnabled()){su.exe('settings put secure location_mode 0');}}}","actions": [""]

    }

]
```

### <i id="3-2"></i>3.2 自动开关GPS
- 注意事项
> 1. 全局变量为：GPSapp
> 2. 作者：默认2234077 <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=ZjUxZmQyOTExYjdjNjMxYzdmMDU~&shareUid=3571197&shareFrom "原帖")</u>

```
[
    {
    "name": "自动开关GPS",
    "description": "变量应用自动开GPS,直到最近任务没有需要gps的应用才会关闭。",
    "priority": 1,
    "condition": "int c = 0 ;if(frontPkgChanged == true && globalVarOf$GPSapp.contains(to)){ui.showShortToast(\"GPS打开\" + (hw.enableLocation() ? \"成功\" : \"失败\")) ;}else if(pkgKilled == true && globalVarOf$GPSapp.contains(pkgName)){foreach (pkn : globalVarOf$GPSapp){if(task.hasTaskFromPackage(pkn) == true) {c++ ;break ;} ;}if(c != 0){break ;}else{hw.disableLocation() ;break ;}break ;}",
    "actions": [
    ""
        ]
    }
]
```
### <i id="3-3"></i>3.3 微信位置共享
- <u>[**`原帖`**](https://www.coolapk.com/feed/20272935?shareKey=ZTdlNjhmZTU0NGFkNjQ0MjJiOTM~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "位置共享",
"description": "打开微信共享位置或者定位界面时打开GPS，返回聊天界面关闭GPS。酷安@幻觉",
"priority": 1,
"condition": "if(activityResumed == true && hw.isLocationEnabled() == false && componentNameAsShortString == \"com.tencent.mm/.plugin.location_soso.SoSoProxyUI\") { (hw.enableLocation() ?) + ui.showShortToast(\"开启GPS\")} else if ( activityResumed == true && hw.isLocationEnabled() == true && componentNameAsShortString == \"com.tencent.mm/.ui.LauncherUI\") { (hw.disableLocation() ?) + ui.showShortToast(\"关闭GPS\")}",
"actions": [""
]
}
]
```

## <i id="4"></i>4. 打盹
### <i id="4-1"></i>4.1 息/亮屏进/退打盹
- 注意事项
> 1. 下面两个分开，且都要刷
> 2. 以下2个情景模式摘自 作者：l奋斗的小青年
> 3. <u>[**`原帖`**](https://www.coolapk.com/feed/31085113?shareKey=ZjIyMDI1YzE0MWI5NjMxYzg3ZWI~&shareFrom "原帖")</u>

1. 息屏进入打盹

```
[
    {
    "name": "Doze打盹：息屏进入打盹",
    "description": "息屏进入深度打盹模式Doze",
    "priority": 1,
    "condition": "screenOff == true",
    "actions": [
    "sh.exe(\"dumpsys deviceidle enable deep\");",
    "sh.exe(\"dumpsys deviceidle force-idle deep\");"
        ]
    }
]
```

2. 亮屏退出打盹

```
[
    {
    "name": "Doze打盹：亮屏退出打盹",
    "description": "亮屏退出深度打盹模式",
    "priority": 1,
    "condition": "screenOn == true",
    "actions":
    [
    "sh.exe(\"dumpsys deviceidle disable all\");",
    "sh.exe(\"dumpsys deviceidle unforce\");"	,
    "sh.exe(\"dumpsys battery reset\");"
    ]
    }
]
```

### <i id="4-2"></i>4.2 息/亮屏进/退打盹进阶
- <u>[**`原帖`**](https://www.coolapk.com/feed/39314021?shareKey=YmRmY2UwNWM3OTYzNjQ0MzVjY2Y~&shareUid=3571197&shareFrom "**`原帖`**")</u>

1. 息屏进入打盹

```
[
{
"name": "深度doze：息屏进入打盹",
"description": "息屏进入深度打盹模式Doze并屏蔽传感器",
"priority": 1,
"condition": "screenOff == true",
"actions": [
"su.exe(\"dumpsys deviceidle enable deep\");",
"su.exe(\"dumpsys deviceidle force-idle deep\");",
"su.exe(\"service call sensor_privacy 8 i32 1\");"
]
}
]
```

2. 亮屏退出打盹

```
[
{
"name": "深度doze：亮屏退出打盹",
"description": "亮屏退出深度打盹模式并恢复传感器",
"priority": 1,
"condition": "screenOn == true",
"actions":
[
"su.exe(\"dumpsys deviceidle disable all\");",
"su.exe(\"dumpsys deviceidle unforce\");" ,
"su.exe(\"service call sensor_privacy 8 i32 0\");",
"su.exe(\"dumpsys battery reset\");"
]
}
]
```

### <i id="4-3"></i>4.3 深度doze并屏蔽传感器延迟版
- <u>[**`原帖`**](https://www.coolapk.com/feed/40016015?shareKey=NTg3MjAwMWM5OGI1NjQ0MzVlYTQ~&shareUid=3571197&shareFrom "**`原帖`**")</u>
- 安卓13要把8改9才能用

```
[
{
"name": "深度doze：亮屏退出打盹",
"description": "亮屏退出深度打盹模式并恢复传感器",
"priority": 1,
"condition": "screenOn == true",
"actions":
[
"su.exe(\"dumpsys deviceidle disable all\");",
"su.exe(\"dumpsys deviceidle unforce\");" ,
"su.exe(\"service call sensor_privacy 8 i32 0\");",
"su.exe(\"dumpsys battery reset\");"
]
}
]
```
```
[
{
"name": "深度doze：息屏进入打盹（延迟版）",
"description": "息屏1分钟后进入深度打盹模式Doze并屏蔽传感器",
"priority": 2,
"delay": 60000,
"condition": "screenOff == true",
"actions": [
"su.exe(\"dumpsys deviceidle enable deep\");",
"su.exe(\"dumpsys deviceidle force-idle deep\");",
"su.exe(\"service call sensor_privacy 8 i32 1\");"
]
}
]
```
```
[
{
"name": "深度doze：亮屏退出打盹(延迟版)附加模块",
"description": "解决延迟后反复开关屏产生bug的问题",
"priority": 1,
"delay": 61000,
"condition": "screenOn == true",
"actions":
[
"su.exe(\"dumpsys deviceidle disable all\");",
"su.exe(\"dumpsys deviceidle unforce\");" ,
"su.exe(\"service call sensor_privacy 8 i32 0\");",
"su.exe(\"dumpsys battery reset\");"
]
}
]
```

### <i id="4-4"></i>4.4 开机关闭打盹
- <u>[**`原帖`**](https://www.coolapk.com/feed/37649574?shareKey=NTVmNGI4YTEzZTg3NjQ0MzVmOTQ~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "开机关闭打盹",
"description": "开机关闭打盹",
"priority": 1,
"condition": "systemReady == true",
"delay": 60000,
"actions": [ "su.exe('dumpsys deviceidle disable');",
"ui.showShortToast(\"😘已关闭打盹\")"
]
}
]
```

## <i id="5"></i>5. WIFI
### <i id="5-1"></i>5.1 WiFi与移动数据网络的自动切换
- 注意事项
> 1. 下面三个情景模式是一套的，都需要刷入
> 2. 开发者模式中的 “始终开启移动数据网络” 需要关闭，如果开启，情景模式将失效
> 3. 作用：为了实现wifi与移动网络的全自动/半自动切换。使用wifi时自动关闭移动网络，wifi断开以后立即自动开启移动网络，并且一定时限内未重新连接上wifi热点自动关闭wlan。简单说就是有啥用啥并且关闭另一个网络（wifi/数据），降低网络耗电的同时保证随时有网络连接。
> 4. 以下3个情景模式摘自 作者：第二次用户名违规
> 5. <u>[**`原帖`**](https://www.coolapk.com/feed/37658594?shareKey=YmRmYzJmODgyMzViNjMxZDg4OTU~&shareUid=2810379&shareFrom "原帖")</u>

1. 数据和wifi自动切换

```
[
{
"name": "数据和wifi自动切换",
"description": " 数据和wifi自动切换开关 ",
"priority": 1,
"condition": "if( wifiStateChanged == true && wifiState.ssid == null && data.isDataEnabled() == false) {( ui.showShortToast(\"打开数据流量\") + data.setDataEnabled(true))} else if ( wifiStateChanged == true && wifiState.enabled == true && wifiState.ssid != null && data.isDataEnabled()== true) { (ui.showShortToast(\"关闭数据流量\") + data.setDataEnabled(false) )}",
"actions": [""
]
}
]
```

2. 监测WiFi是否与热点断开

```
[
{
"name": "监测WiFi是否与热点断开",
"description": " 若WiFi与当前热点断开连接，等待60秒后发送通知，判断是否重新连上WiFi热点",
"delay":60000,
"priority": 2,
"condition": "wifiStateChanged == true && hw.isWifiEnabled== true ",
"actions": ["ui.showNotification('wf', 'WiFi连接状态', context.getSystemService('wifi').getConnectionInfo().getSSID(), false);"
]
}
]
```

3. 判断是否关闭WiFi

```
[
{
"name": "判断是否关闭WiFi",
"description": "获取当前WiFi连接状态，若未连接到任何热点则关闭WLAN并关闭通知，否则不进行任何操作并关闭通知",
"priority": 2,
"condition": "if(notificationAdded==true && notificationContent == '<unknown ssid>') {hw.disableWifi();ui.showShortToast('WiFi已关闭');ui.cancelNotification('wf');} else if(notificationAdded==true && notificationContent !=( '<unknown ssid>')) {ui.cancelNotification('wf')} ",
"actions": [
" "
]
}
]
```

### <i id="5-2"></i>5.2 自动开关WIFI
- <u>[**`原帖`**](https://www.coolapk.com/feed/23374387?shareKey=YWQyYTI5ODc2YThkNjQ0MjJhN2E~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "自动开关WIFI",
"description": "息屏5秒后关闭WIFI，解锁1秒后打开WIFI，酷安@幻觉",
"priority": 2,
"condition": "if( hw.isWifiEnabled() == true && screenOff==true) { (Thread.sleep(5000) + hw.disableWifi() ) } else if ( hw.isWifiEnabled() == false && userPresent==true) { (Thread.sleep(1000) + hw.enableWifi() ) }",
"actions": [
""
]
}
]
```

### <i id="5-3"></i>5.3 数据和wifi
- <u>[**`原帖`**](https://www.coolapk.com/feed/45338524?shareKey=MzA4ZTQ5ZDdmMWI0NjQ0MzRkZjU~&shareUid=3571197&shareFrom "原帖")</u>

```
[
{
"name": "数据和wifi",
"description": "连上wifi，关闭数据，断开连接，开启数据",
"priority": 1,
"condition": "if( wifiStateChanged == true && wifiState.ssid == null && data.isDataEnabled() == false) { data.setDataEnabled(true) } else if ( wifiStateChanged == true && wifiState.enabled == true && wifiState.ssid != null && data.isDataEnabled()== true) { data.setDataEnabled(false) }",
"actions": [""
]
}
]
```

## <i id="6"></i>6. FCM
### <i id="6-1"></i>6.1 墓碑模式FCM推送
- 来自NoActive群友投稿
- <u>[**`索尼CBO`**](http://www.coolapk.com/u/427700 "**`索尼CBO`**")</u>
- 该情景模式只适配了FreezeV2 UID模式

1. 一代

```
[
  {
    "name": "墓碑模式FCM推送",
    "description": "对于支持FCM推送的应用，当其收到推送并且处于后台时，唤醒它5秒",
    "priority": -1,
    "condition": "fcmPushMessageArrived == true",
    "actions": [
      "def freeze(pkg) { su.exe(\"pm list packages -U | grep \" + pkg + \" | awk -F '[:,]' '{print $3; print $4;}' | while read uid; do ls /sys/fs/cgroup/uid_$uid | grep pid_ | while read pid; do echo 1 > /sys/fs/cgroup/uid_$uid/$pid/cgroup.freeze; done; done\"); } def unfreeze(pkg) { su.exe(\"pm list packages -U | grep \" + pkg + \" | awk -F '[:,]' '{print $3; print $4;}' | while read uid; do ls /sys/fs/cgroup/uid_$uid | grep pid_ | while read pid; do echo 0 > /sys/fs/cgroup/uid_$uid/$pid/cgroup.freeze; done; done\"); } def allowWakeLock(pkg) { su.exe(\"appops set \" + pkg + \" WAKE_LOCK default\"); } def denyWakeLock(pkg) { su.exe(\"appops set \" + pkg + \" WAKE_LOCK ignore\"); } def isVisible(pkg) { return activity.getFrontAppPackage() == pkg || thanos.windowManager.hasVisibleWindows(pkg); } if (!isVisible(pkgName)) { unfreeze(pkgName); allowWakeLock(pkgName); Thread.sleep(5000); denyWakeLock(pkgName); freeze(pkgName); }"
    ]
  }
]
```

2. 2代
- <u>[**`原帖`**](https://www.coolapk.com/feed/45826044?shareKey=ODRjMmZiZWQwN2RkNjQ1OWIxYTc~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
  {
    "name": "墓碑模式FCM推送v2",
    "description": "对于支持FCM推送的应用，当其收到推送并且处于后台时，唤醒它5秒。\n仅适配了FreezerV2(uid)控制器，使用NoActive测试正常，理论上与各墓碑模块兼容。",
    "priority": -1,
    "condition": "fcmPushMessageArrived == true",
    "actions": [
      "def formatMsg(msg) { return \"🚀 \" + msg; } def getRunningPkgList() { return thanos.getActivityManager().getRunningAppPackages(); } def getProcessListByPkg(pkg) { return thanos.getActivityManager().getRunningAppProcessForPackage(pkg); } def getPkgListByPkgName(pkgName) { res = []; foreach(pkg : getRunningPkgList()) { if (pkg.getPkgName() == pkgName) { res.add(pkg); } } return res; } def getProcessFrozenStatus(process) { return su.exe(\"cat /sys/fs/cgroup/uid_\" + process.getUid() + \"/pid_\" + process.getPid() + \"/cgroup.freeze\").out[0]; } def setProcessFrozenStatus(process, status) { su.exe(\"echo \" + status + \" > /sys/fs/cgroup/uid_\" + process.getUid() + \"/pid_\" + process.getPid() + \"/cgroup.freeze\"); } def main() { log.log(formatMsg(\"收到 FCM 推送: \" + pkgName)); pkgList = getPkgListByPkgName(pkgName); if (pkgList.size() == 0) { log.log(formatMsg(\"未找到正在运行的进程\")); return; } else { log.log(formatMsg(\"正在运行的进程列表: \" + (pkgName in pkgList))); } processList = []; foreach(pkg : pkgList) { foreach(process : getProcessListByPkg(pkg)) { if (getProcessFrozenStatus(process) == \"1\") { processList.add(process); } } } if (processList.size() == 0) { log.log(formatMsg(\"未找到已冻结的进程\")); return; } else { log.log(formatMsg(\"已冻结的进程列表: \" + (processName in processList))); } log.log(formatMsg(\"开始解冻进程\")); foreach(process : processList) { setProcessFrozenStatus(process, \"0\"); } log.log(formatMsg(\"解冻进程完成\")); Thread.sleep(5000); log.log(formatMsg(\"开始冻结进程\")); foreach(process : processList) { setProcessFrozenStatus(process, \"1\"); } log.log(formatMsg(\"冻结进程完成\")); } main();"
    ]
  }
]
```

3. 2.1代
- <u>[**`原帖`**](https://www.coolapk.com/feed/46462851?shareKey=NDMzMWVmMzhiYjQwNjQ3OWYyYmM~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
  {
    "name": "墓碑模式FCM推送v2.1",
    "description": "！！！Thanox需要更新到v4.1.9或更高版本！！！\n对于支持FCM推送的应用，当其收到推送并且处于后台时，唤醒它5秒。\n仅适配了FreezerV2(uid)控制器，使用NoActive测试正常，理论上与各墓碑模块兼容。",
    "priority": -1,
    "condition": "fcmPushMessageArrived == true",
    "actions": [
      "def debug(msg) { log.log(\"🚀 \" + msg); } def getUserIdList() { userList = context.getSystemService(context.USER_SERVICE).getUsers(); userIdList = (identifier in (userHandle in userList)); return userIdList; } def getPkgByNameAndUserId(pkgName, userId) { pkg = Pkg.newPkg(pkgName, userId); return pkg; } def isValidPkg(pkg) { return thanos.getPkgManager().getAppInfo(pkg) != null; } def getPkgListByPkgName(pkgName) { userIdList = getUserIdList(); res = []; foreach(userId: userIdList) { pkg = getPkgByNameAndUserId(pkgName, userId); if (isValidPkg(pkg)) { res.add(pkg); } } return res; } def getProcessListByPkgList(pkgList) { processList = []; foreach(pkg : pkgList) { processList.addAll(thanos.getActivityManager().getRunningAppProcessForPackage(pkg)); } return processList; } def isPkgVisible(pkg) { return thanos.getActivityManager().isAppForeground(pkg) || thanos.getWindowManager().hasVisibleWindows(pkg); } def getProcessFrozenStatus(process) { return su.exe(\"cat /sys/fs/cgroup/uid_\" + process.getUid() + \"/pid_\" + process.getPid() + \"/cgroup.freeze\").out[0]; } def setProcessFrozenStatus(process, status) { su.exe(\"echo \" + status + \" > /sys/fs/cgroup/uid_\" + process.getUid() + \"/pid_\" + process.getPid() + \"/cgroup.freeze\"); } def main() { debug(\"收到 FCM 推送: \" + pkgName); pkgList = getPkgListByPkgName(pkgName); processList = getProcessListByPkgList(pkgList); if (processList.size() == 0) { debug(\"应用尚未运行，尝试启动应用\"); isLaunched = activity.launchProcessForPackage(pkgName); if (!isLaunched) { debug(\"启动应用失败\"); return; } else { debug(\"启动应用成功\"); } debug(\"等待应用启动完成\"); Thread.sleep(3000); processList = getProcessListByPkgList(pkgList); debug(\"需要冻结的进程列表: \" + (processName in processList)); } else { debug(\"正在运行的进程列表: \" + (processName in processList)); frozenProcessList = []; foreach(process : processList) { if (getProcessFrozenStatus(process) == \"1\") { frozenProcessList.add(process); } } if (frozenProcessList.size() == 0) { debug(\"未找到已冻结的进程\"); return; } else { debug(\"已冻结的进程列表: \" + (processName in frozenProcessList)); } debug(\"开始解冻进程\"); foreach(process : frozenProcessList) { setProcessFrozenStatus(process, \"0\"); } debug(\"解冻进程完成\"); processList = frozenProcessList; } Thread.sleep(5000); foreach(pkg : pkgList) { if (isPkgVisible(pkg)) { debug(\"应用正在前台运行，跳过冻结\"); return; } } debug(\"开始冻结进程\"); foreach(process : processList) { setProcessFrozenStatus(process, \"1\"); } debug(\"冻结进程完成\"); } main();"
    ]
  }
]
```

## <i id="7"></i>7. QW优化
### <i id="7-1"></i>7.1 微信FCM推送
1. 一代
- <u>[**`原帖`**](https://www.coolapk.com/feed/43891503?shareKey=ZGEzOTUyODY5MjE4NjQxYjAzM2E~&shareUid=23911084&shareFrom "**`原帖`**")</u>

```
[
{
"name": "微信进程优化",
"description": "优化保留双进程，同时开启fcm服务",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mm\"",
"actions": [
"Thread.sleep(3000);",
"su.exe(\"ps -ef|grep com.tencent.mm:|grep -v :push|grep -v grep|awk '{print $2}'|xargs kill -9\")",
"su.exe(\"am start-foreground-service -n com.tencent.mm/com.tencent.mm.plugin.fcm.WCFirebaseMessagingService\")"
]
}
]
```

2. 二代
- <u>[**`原帖`**](https://www.coolapk.com/feed/44407705?shareKey=MThiMzk0MjVjZGE2NjQzZTkzMGE~&shareUid=10892111&shareFrom "**`原帖`**")</u>

```
[
{
"name": "微信进程优化",
"description": "优化保留双进程，同时开启fcm推送",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mm\"",
"actions": [
"Thread.sleep(3000);",
"su.exe(\"ps -ef|grep com.tencent.mm:|grep -v :push|grep -v grep|awk '{print $2}'|xargs kill -9\")",
"su.exe(\"am stopservice -n com.tencent.mm/com.tencent.mm.ipcinvoker.wx_extension.service.PushProcessIPCService\")"
]
}
]
```

### <i id="7-2"></i>7.2 QQ/微信后台优化
- <u>[**`原帖`**](https://www.coolapk.com/feed/42184751?shareKey=Y2EzZWNiMTdjYjU4NjQ0MjFlYmI~&shareFrom "**`原帖`**")</u>
- QQ：

```
[
{
"name": "QQ进程优化",
"description": "优化保留双进程和通话",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mobileqq\"",
"actions": [
"Thread.sleep(3000);",
"su.exe(\"ps -ef|grep com.tencent.mobileqq:|grep -v :MSF|grep -v :video|grep -v grep|awk '{print $2}'|xargs kill -9\")",
"ui.showShortToast(\"已清理QQ\")"
]
}
]
```

- 微信：

```
[
{
"name": "微信进程优化",
"description": "优化保留双进程",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mm\"",
"actions": [
"Thread.sleep(3000);",
"su.exe(\"ps -ef|grep com.tencent.mm:|grep -v :push|grep -v grep|awk '{print $2}'|xargs kill -9\")",
"ui.showShortToast(\"已清理微信\")"
]
}
]
```

### <i id="7-3"></i>7.3 QQ进程优化
- 一位不愿透露姓名的群友提供由<u>[**`QQ/微信后台优化`**](#QQ/微信后台优化)</u>改版

```
[
{
"name": "QQ进程优化",
"description": "优化保留双进程和通话（配合mitlite+mipush），切换应用、后台1秒或者锁屏杀了msf和其他进程，强制走mipush",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mobileqq\" || screenOff == true",
"actions": [
"Thread.sleep(1000);",
"su.exe(\"ps -ef|grep com.tencent.mobileqq:|grep -v :video|grep -v grep|awk '{print $2}'|xargs kill -9\")"
]
}
]
```

### <i id="7-4"></i>7.4 AutoKill-QQMSF
- 一代：
	 - <u>[**`原帖`**](https://www.coolapk.com/feed/45019763?shareKey=YTcyMzg0OWM4NjJkNjQ0MjIzMzI~&shareFrom "**`原帖`**")</u>

```
[
{
"name": "AutoKill-QQMSF",
"description": "QQ被切入后台三秒将自动结束MSF服务及其他无用进程，强制QQ使用MiPush或其他系统级推送服务。\n原作者酷安@星澪Star_ZER0 \n修改者酷安@微软Mic",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mobileqq\"",
"actions": [
"Thread.sleep(3000);",
"su.exe(\"ps -ef|grep com.tencent.mobileqq:|grep :MSF|grep -v :video|grep -v grep|awk '{print $2}'|xargs kill -9\")"
]
}
]
```

- 二代
	 - <u>[**`原帖`**](https://www.coolapk.com/feed/45421288?shareKey=ZGJmMmFjMWVmMjBiNjQ0NGU4MGY~&shareUid "**`原帖`**")</u>

```
[
{
"name": "AutoKill-QQMSF",
"description": "QQ被切入后台将自动结束MSF服务\n强制QQ使用MiPush或其他系统级推送\n作者酷安@微软Mic\n当前版本: 0.2",
"priority": 1,
"condition": "frontPkgChanged",
"actions": [
"foreach(process : thanos.activityManager.getRunningAppProcessForPackage(from)) { log.log(process.processName); if (process.processName == \"com.tencent.mobileqq\") { thanos.activityManager.killProcessByName(\"com.tencent.mobileqq:MSF\");}}"
]
}
]
```


### <i id="7-5"></i>7.5 QQ进程优化双进程
- <u>[**`原帖`**](https://www.coolapk.com/feed/35808050?shareKey=ZTczOTFmYWE5Y2ZkNjQ0MzViMWQ~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "QQ进程优化双进程",
"description": "从bili离开后 结束多余的进程",
"priority": 2,
"condition": "frontPkgChanged == true && from == \"com.tencent.mobileqq\"",
"actions": [
"Thread.sleep(5000);",
"su.exe(\"ps -ef|grep com.tencent.mobileqq:|grep -v :MSF|grep -v grep|awk '{print $2}'|xargs kill -9\")",
"ui.showShortToast(\"😘清理QQ\")"
]
}
]
```

### <i id="7-6"></i>7.6 QQ微信电池优化管理
- <u>[**`原帖`**](https://www.coolapk.com/feed/37649574?shareKey=NTVmNGI4YTEzZTg3NjQ0MzVmOTQ~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "QQ微信电池优化管理",
"description": "QQ微信电池优化选项，解锁时「优化」，熄屏时「不优化」",
"priority": 1,
"condition":"if(screenOff == true) {Thread.sleep(5000);(su.exe(\"dumpsys deviceidle whitelist +com.tencent.mm\"));(su.exe(\"dumpsys deviceidle whitelist +com.tencent.mobileqq\"))} else if (userPresent == true) {Thread.sleep(2000);(su.exe(\"dumpsys deviceidle whitelist -com.tencent.mm\"));(su.exe(\"dumpsys deviceidle whitelist -com.tencent.mobileqq\"))}",
"actions": [
""
]
}
]
```


## <i id="8"></i>8. 充电
### <i id="8-1"></i>8.1 充电控制(**无效**)
- 充电恢复

```
[
{
"name": "充电恢复",
"description": "电量低至85，恢复充电",
"priority": 1,
"condition": "batteryChanged == true && batteryLevel == 85 && isCharging == false ",
"actions": [ "su.exe(\"echo 0 > /sys/class/power_supply/battery/input_suspend\");",
"ui.showShortToast(\"充电已恢复\");"
]
}
]
```

- 充电停止
```
[
{
"name": "充电停止",
"description": "电量达到90后，延时10秒，停止充电",
"priority": 1,
"delay":10000,
"condition": "isCharging==true && batteryChanged == true && batteryLevel == 90",
"actions": [ "su.exe(\"echo 1 > /sys/class/power_supply/battery/input_suspend\");",
"ui.showShortToast(\"充电已停止\");"
]
}
]
```

### <i id="8-2"></i>8.2. 充电停止
> 下面两种充电停止自己选一个就行
> 来自酷友提供

- 充电90停止

```
[
{
"name": "充电停止",
"description": "电量达到90后，延时30秒，停止充电",
"priority": 1,
"delay":30000,
"condition": "isCharging==true && batteryChanged == true && batteryLevel == 90",
"actions": [ "su.exe(\"echo 1 > /sys/class/power_supply/battery/input_suspend\");",
"ui.showShortToast(\"充电已停止\");"
]
}
]
```

- 充电100停止

```
[
{
"name": "充电停止",
"description": "电量达到100，60秒后停止充电",
"priority": 1,
"delay":60000,
"condition": "isCharging==true && batteryChanged == true && batteryLevel == 100",
"actions": [ "su.exe(\"echo 1 > /sys/class/power_supply/battery/input_suspend\");",
"ui.showNotification(\"充电\",\"已停止充电\",\"当前电量为：\" + batteryLevel + \"，现在开始，随时都可以拔电啦！\",false)"
]
}
]
```


## <i id="9"></i>9. NFC/蓝牙
### <i id="9-1"></i>9.1 自动开关NFC
- <u>[**`原帖`**](https://www.coolapk.com/feed/20776215?shareKey=NmZkM2NhN2VhZGJjNjQ0MjJiNTU~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "自动开关NFC",
"description": "打开应用开NFC，关闭应用关NFC。应用名单来自全局变量nfc。酷安@幻觉",
"priority": 2,
"condition": "if( hw.isNfcEnabled() == false && frontPkgChanged == true && globalVarOf$nfc.contains(to)) { hw.enableNfc() } else if ( hw.isNfcEnabled() == true && pkgKilled == true && globalVarOf$nfc.contains(pkgName)) { hw.disableNfc() }",
"actions": [
""
]
}
]
```

### <i id="9-2"></i>9.2 自动开关蓝牙
- <u>[**`原帖`**](https://www.coolapk.com/feed/20061143?shareKey=OTM1OGI3MmVmNzUxNjQ0MjJlOTU~&shareUid=3571197&shareFrom "**`原帖`**")</u>

```
[
{
"name": "自动开关蓝牙",
"description": "使用了全局变量bt。酷安@幻觉",
"priority": 2,
"condition": "if(frontPkgChanged == true && globalVarOf$bt.contains(to)) { (hw.enableBT() ?) } else if ( pkgKilled == true && globalVarOf$bt.contains(pkgName)) { (hw.disableBT() ?) }",
"actions": [
""
]
}
]
```

### <i id="9-3"></i>9.3 蓝牙断开自动关闭
- <u>[**`原帖`**](https://www.coolapk.com/feed/45338524?shareKey=MzA4ZTQ5ZDdmMWI0NjQ0MzRkZjU~&shareUid=3571197&shareFrom "原帖")</u>

```
[
{
"name": "蓝牙断开自动关闭",
"description": "蓝牙连接断开，自动关闭蓝牙",
"priority": 2,
"condition":"btConnectionStateChanged == true && btConnectionStateDisconnected == true",
"actions": [
"ui.showShortToast(\"蓝牙关闭\" + (hw. disableBT() ? \"成功\" : \"失败\"));"
]
}
]
```


## <i id="10"></i>10. 核心控制
### <i id="10-1"></i>10.1 亮/息屏开关核心
- <u>[**`原帖`**](https://www.coolapk.com/feed/30684555?shareKey=NDM0MjdhYzI4YjQ4NjQ0MzVjMjM~&shareUid=3571197&shareFrom "原帖")</u>
- 息屏关核

```
[
{
"name": "屏幕熄灭关闭核心",
"description": "当屏幕被熄灭时，关闭核心",
"priority": 1,
"condition": "screenOff == true",
"actions": [
"su.exe(\"echo 0 > /sys/devices/system/cpu/cpu2/online&&echo 0 > /sys/devices/system/cpu/cpu3/online&&echo 0 > /sys/devices/system/cpu/cpu1/online&&echo 0 > /sys/devices/system/cpu/cpu5/online&&echo 0 > /sys/devices/system/cpu/cpu6/online&&echo 0 > /sys/devices/system/cpu/cpu7/online\")"
]
}
]
```

- 亮屏开核

```
[
{
"name": "屏幕点亮打开核心",
"description": "当屏幕被点亮时，打开被关闭的核心",
"priority": 1,
"condition": "screenOn == true",
"actions": [
"su.exe(\"echo 1 > /sys/devices/system/cpu/cpu2/online&&echo 1 > /sys/devices/system/cpu/cpu3/online&&echo 1 > /sys/devices/system/cpu/cpu1/online&&echo 1 > /sys/devices/system/cpu/cpu5/online&&echo 1 > /sys/devices/system/cpu/cpu6/online&&echo 1 > /sys/devices/system/cpu/cpu7/online\")"
]
}
]
```

### <i id="10-2"></i>10.2 吃大核锁小核
- 注意事项：
> 1. 该情景模式来自酷友投稿
> 2. 全局变量：background
> 	 - 把需要的软件加进去
> 3. 适用情景：有的app，用起来的电流超级高，可以给它分配小核，这样使用可能有点卡，但是相对省电很多（也要分情况看什么app,有的只用小核，反而耗电量会增加）
> 4. 其中，框住的地方就是你【线程分配.sh】的路径，可以自定义
> [![](https://bzmshang.top/content/uploadfile/202304/50711682307838.jpg)](https://bzmshang.top/content/uploadfile/202304/50711682307838.jpg)
> 5. 你可以修改boost background改成boost audio-app，进程就分配成audio-app的cpu了
> 	 - cpuset有top-app、background、foreground、restricted、system-background、audio-app等
> [![](https://bzmshang.top/content/uploadfile/202304/ebda1682308150.jpg)](https://bzmshang.top/content/uploadfile/202304/ebda1682308150.jpg)
> 6. 当你在全局变量添加的包名后，比如你添加了微信，那么你打开微信后，过了5秒后，就会变成你想要的background了。你可以把scene开启小窗模式，然后查看微信进程的cpuset。一般前台应用的命令行是/foreground，如果成功，/foreground会变成你想要的模式


1. 首先创建一个【线程分配.sh】,文件内容如下：

```
function boost()
{
sleep 4
chmod -R 777 /dev/cpuset
pgrep -f $2 | while read pid; do
#echo $pid > /dev/cpuset/$1/tasks
echo $pid > /dev/cpuset/$1/cgroup.procs
done
}
```

2. Thannox中创建情景模式，内容如下：

```
[
{
"name": "cpuset变成background",
"description": "打开程序时，变成background，分配cpu1-3",
"priority": 2,
"condition": "frontPkgChanged == true && globalVarOf$background.contains(to)",
"actions": [
"su.exe(\"source /storage/emulated/0/Android/线程分配.sh&&boost background \" +to);"
]
}
]
```

## <i id="11"></i>11. 通知
### <i id="11-1"></i>11.1 弹幕通知
- 全局变量：danmu_app
- 在变量里面的软件才会进行弹幕通知
- 来自酷友提供

```
[
  {
    "name": "弹幕通知V2(danmu_app)",
    "description": "在全局danmu_app里面的包,收到通知时用弹幕展示",
    "priority": 1,
    "condition":
"thanos.profileManager.isGlobalRuleVarByNameExists(\"danmu_app\") ==false||(notificationAdded && !notification.isForegroundService && pkgName != \"android\"&&globalVarOf$danmu_app.contains(pkgName))",
    "actions": [
"if(thanos.profileManager.isGlobalRuleVarByNameExists(\"danmu_app\") == false){thanos.profileManager.addGlobalRuleVar(\"danmu_app\",{\"com.tencent.mm\",\"com.tencent.mobileqq\",\"com.coolapk.market\"});ui.showDanmu(\"app://\" + \"github.tornaco.android.thanos\",\"创建danmu_app成功\");su.exe(\"if [ ! -f /data/message.sh ];then touch /data/message.sh;echo 'xx=`cat /data/message.txt`;echo ${xx#*:} > /data/message.txt' > /data/message.sh;fi;if [ ! -f /data/message.txt ];then touch /data/message.txt;fi\")}",
"if(thanos.profileManager.isGlobalRuleVarByNameExists(\"danmu_app\") == true){su.exe(\"echo \"+notificationContent+\" > /data/message.txt\");p=su.exe(\"echo \"+notificationContent).out[0];su.exe(\"sh /data/message.sh\");a=su.exe(\"cat /data/message.txt\").out[0];if(a!=\"\"){count=su.exe(\"echo \"+notificationContent+\"|cut -d '[' -f2|cut -d ']' -f1\").out[0];if(count!=\"\"&&(pkgName==\"com.tencent.mm\")&&count!=a&&count!=p){count=\"(\"+count+\"新消息)\"}else {count=\"\"};ui.showDanmu(\"app://\" +pkgName,notificationTitle+count+\": \"+a)}else{p=su.exe(\"echo \"+notificationContent).out[0];b=su.exe(\"echo \"+notificationContent+\" > /data/message.txt\").out[0];su.exe(\"echo \"+b+\"> /data/message.txt \");su.exe(\"sh /data/message.sh\");a=su.exe(\"cat /data/message.txt\").out[0];count=su.exe(\"echo \"+b+\" |cut -d '[' -f2|cut -d ']' -f1\").out[0];if(count!=\"\"&&(pkgName==\"com.tencent.mm\")&&count!=a&&count!=p){count=\"(\"+count+\"新消息)\"}else {count=\"\"};if(pkgName==\"com.tencent.mobileqq\"){q=\"\"};ui.showDanmu(\"app://\" +pkgName,notificationTitle+count+\": \"+a+\"...[多行]\")}}"
    ]
  }
]
```

### <i id="11-2"></i>11.2 弹幕通知
- <u>[**`原帖`**](https://www.coolapk.com/feed/51054436?shareKey=ODJmNTJjOWZiYWQ0NjU0ZjNiM2Y~&shareUid=3571197&shareFrom "原帖")</u>

```
[
{
"name": "弹幕通知",
"description": "收到通知或通知更新时时用弹幕展示;设置全局变量danmuapps，添加需要弹幕显示其通知的应用包名;设置全局变量gameapps，添加当该应用在前台不显示弹幕的应用包名;",
"priority": 1,
"condition": "(notificationRemoved == false && (notificationAdded || notificationUpdated)) && !notification.isForegroundService && pkgName != \"android\" && globalVarOf$danmuapps.contains(pkgName)&&(! globalVarOf$gameApps.contains(activity.getFrontAppPackage()))",
"actions": [
"ui.showDanmu(\"app://\" + pkgName,thanos.pkgManager.getAppInfo(pkgName).appLabel + \" :\" + notificationTitle + \":\" + notificationContent);"
]
}
]
```

## <i id="12"></i>12. Google
### <i id="12-1"></i>12.1 开启谷歌系软件，解冻谷歌服务和框架
- <u>[**`原帖`**](https://www.coolapk.com/feed/33693395?shareKey=Y2FjYjNiYTYyMTM0NjQ0OTFjMDE~&shareUid=2521387&shareFrom "原帖")</u>
- 如果需要自己添加启动某个应用时解冻只要在||to == \"com.google.android.youtube\"后面再加||to == \"xxx应用的包名\"就行

- 谷歌冻结

```
[
{
"name": "谷歌冻结 ",
"description": "冻结gp服务",
"priority": 1,
"condition": "pkgKilled == true &&( pkgName == \"com.android.vending\"||pkgName == \"com.google.android.youtube\")",
"actions": [
"pkg.disableApplication(\"com.google.android.gms\")","pkg.disableApplication(\"com.google.android.gsf\")"
]
}
]
```

- 谷歌解冻

```
[
{
"name": "谷歌解冻 ",
"description": "解冻gp服务",
"priority": 1,
"condition": "frontPkgChanged == true &&( to == \"com.android.vending\"||to == \"com.google.android.youtube\")",
"actions": [
"pkg.enableApplication(\"com.google.android.gms\")","pkg.enableApplication(\"com.google.android.gsf\")"
]
}
]
```
### <i id="12-2"></i>12.2 开启谷歌系软件，解冻谷歌服务和框架进阶版
- 全局变量：Google
- 全局变量里面选择谷歌系软件，当打开那个软件时，解冻谷歌服务
```
[
{
"name": "解冻谷歌服务和框架",
"description": "全局变量Google",
"priority": 1,
"condition": "frontPkgChanged == true &&globalVarOf$Google.contains(to)",
"actions": [
"pkg.enableApplication(\"com.google.android.gms\")",
"pkg.enableApplication(\"com.google.android.gsf\")"
]
}
]
```

## <i id="13"></i>13. 电池
### <i id="13-1"></i>13.1 电池白名单
- <u>[**`原帖`**](https://www.coolapk.com/feed/44585473?shareKey=ODc3MmQ5MDFmYzIyNjQ0OTFmMzE~&shareUid=3571197&shareFrom "原帖")</u>
- MIUI系统在重启或者某些情况下，电池优化白名单中的企鹅等流氓等应用会被恢复。每次启动或熄屏自动删除电池优化白名单，重新设置为自己的，使MIUI无机会恢复厂家设置的流氓名单：来自Jinamin
- 全局变量：PowerWhiteList 添加你想要保后台的应用

```
[
{
"name": "电池优化白名单",
"description": "开机或灭屏设置电池优化白名单，全局变量PowerWhiteList",
"priority": 1,
"condition": "systemReady == true || screenOff == true",
"actions": [
"su.exe(\"for i in `dumpsys deviceidle whitelist | cut -d ',' -f 2 `;do dumpsys deviceidle whitelist -$i; done \")",
"for (String p : globalVarOf$PowerWhiteList) {su.exe(\"dumpsys deviceidle whitelist +\" +p)}"
]
}
]
```

## <i id="14"></i>14. 到点执行
### <i id="14-1"></i>14.1 到点重启
- 作者：诗里沧海怨怼
- 全局变量：timedApp
- 缺点：手机打盹得时候，可能不会执行，最好把时间设置到睡觉时间之外得其它时间，或者你能让打盹不影响灭霸也行
- 使用方法：
> 1. 将该情景模式使用JSON粘贴好后，设置好全局变量及应用
> 2. 选择右上角的三个点，找到引擎，选择时间与日期
> 3. 左下角是定时时间，右下角是间隔时间，Tag写tmieKill，至于时间随你自己选了
> 	 - 定时时间想要设置多个，就需要改情景模式，将【"condition": "timeTick&&tag=='timeKill'",】改成【"condition": "timeTick&&(tag=='timeKill1' || tag=='timeKill2')",】如果还想要更多定时时间，则将右小括号前继续增加【 || tag=='timeKill？'】其中问好就是代表数字，自己往后加就行了，【||前有个空格，不要遗漏】
> 	 - 间隔时间就不需要改情景模式，直接在Tag写tmieKill就行

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

### <i id="14-2"></i>14.2 到点杀死
- 作者：诗里沧海怨怼
- 全局变量：timedApp
- 缺点：手机打盹得时候，可能不会执行，最好把时间设置到睡觉时间之外得其它时间，或者你能让打盹不影响灭霸也行
- 使用方法：
> 1. 将该情景模式使用JSON粘贴好后，设置好全局变量及应用
> 2. 选择右上角的三个点，找到引擎，选择时间与日期
> 3. 左下角是定时时间，右下角是间隔时间，Tag写tmieKill，至于时间随你自己选了
> 	 - 定时时间想要设置多个，就需要改情景模式，将【"condition": "timeTick&&tag=='timeKill'",】改成【"condition": "timeTick&&(tag=='timeKill1' || tag=='timeKill2')",】如果还想要更多定时时间，则将右小括号前继续增加【 || tag=='timeKill？'】其中问好就是代表数字，自己往后加就行了，【||前有个空格，不要遗漏】
> 	 - 间隔时间就不需要改情景模式，直接在Tag写tmieKill就行

```
[
    {
        "name": "到点杀死",
        "description": "by诗里沧海怨怼",
        "priority": 1,
        "condition": "timeTick&&tag=='timeKill'",
        "actions":["aMan=thanos.activityManager;for(app:globalVarOf$timedApp){if(aMan.isPackageRunning(app)&&!activity.frontAppPackage.equals(app)){aMan.forceStopPackage(app);}}ui.showShortToast('到点杀死');"]
    }
]
```

## <i id="15"></i>15. 冻结
### <i id="15-1"></i>15.1 智能冻结
- <u>[**`原帖`**](https://www.coolapk.com/feed/45832695?shareKey=MTY5NTcyYmNlOWFjNjQ1ODkxOGI~&shareUid=2929582&shareFrom "**`原帖`**")</u>
- 智能冻结，thanox提供有两种冻结方式，如果使用PM suspend方式冻结，对于流氓软件，虽然图标变成了灰色，但其组件仍然在后台运行。所以针对这类软件，我的方法仍是情景模式，直接使用pm disable 甚至叠加pm hide的方式冻结。注意！情景模式冻结应用不能并列在智能冻结列表，冻结的应用要启动可以thanox设置该应用智能触冻，桌面启动图标可以用快挗方式软件设置快挗方式。

```
[
{
"name": "返回桌面停用",
"description": "返回桌面停用全局变量DS列表中的应用",
"priority": 2,
"condition": "frontPkgChanged == true && globalVarOf$HomeApp.contains(to) || screenOff == true",
"actions": [
"for (String d : globalVarOf$DS){if(pkg.isApplicationEnabled(d)){killer.killPackage(d);su.exe(\"pm disable \" + d);ui.showShortToast(\"❄️\" + thanos.pkgManager.getAppInfo(d).appLabel +\"⭕\");}}"
]
}
]
```

## <i id="16"></i>16. 屏幕
## <i id="16-1"></i>16.1 开关屏日志
- <u>[**`原帖`**](https://www.coolapk.com/feed/39045897?shareKey=NzQ1YTJkMWQwZTk2NjRjNjNjZTM~&shareUid=3119350&shareFrom "**`原帖`**")</u>
- 开关屏日志，该情景模式记录你开屏和关屏的时间到日志里，这样你可以更直观的知道应用是在亮屏时候杀死的还是灭屏后杀死的。日志文件路径"/data/system/thanos_ynokiMHEVHZWWdDH/profile_user_io/kill/killLog.txt"，打开日志文件即可查看。代码如下，请用情景模式json编写。

```
[
{
"name": "开关屏日志",
"description": "冻结对象来自dongjie变量",
"priority": 1,
"condition": "if(screenOff == true){io.writeAppend(\"kill/killLog.txt\",\"手机在此时间关屏:\"+ new java.util.Date() +\"\\r\")}else if(screenOn== true || userPresent == true){io.writeAppend(\"kill/killLog.txt\",\"手机在此时间打开屏幕:\"+ new java.util.Date() +\"\\r\")}",
"actions": [
""
]
}
]
```


## <i id="17"></i>17. 管家流
### <i id="17-1"></i>17.1 多后台管家
群里有教程
<details>
<summary>点击查看</summary>
加群获取最新：528265424
<br>答案:诗里沧海怨怼
</details>

# <i id="墓碑"></i>墓碑
- 通用须知
> 1. 墓碑情景需搭配<u>[**`NoANR`**](https://github.com/myflavor/NoANR "NoANR")</u>使用，墓碑情景需打开灭霸 **SU开关**
> 2. 以下墓碑皆来自 作者：aa 所优化作者：myflavor 所写的<u>[**`墓碑情景模式`**](https://www.myflv.cn/other/281.html "墓碑情景模式")</u>
> 3. BUG未知，自测

## <i id="乖巧名单版"></i>1. 乖巧名单版
- 注意事项
> 1. 刷入下方的冻结和解冻
> 2. 关闭Thanox乖巧模式总开关
> 3. 开启下方单独应用的乖巧开关
> 4. 开启乖巧的应用就会对该应用进行墓碑
> 5. 该情景模式需要2条，一条冻结，一条解冻(Color的冻结刷优化版)
> 6. 1-1只能二选一

### <i id="冻结"></i>1-1(1). 冻结

```
[
    {
        "name": "NoActive(音频版)冻结",
        "description": "应用进入后台冻结进程,白名单为全局变量whiteApps",
        "priority": 1,
        "delay": 700,
        "condition": "frontPkgChanged && thanos.getActivityManager().isPkgSmartStandByEnabled(from)&& !globalVarOf$whiteApps.contains(from)",
        "actions": [ "if(thanos.getAudioManager().hasAudioFocus(from)){su.exe(\"dumpsys deviceidle whitelist +\"+from)}else{su.exe(\"dumpsys deviceidle whitelist\"+from);if(activity.getFrontAppPackage()!=from && !thanos.windowManager.hasVisibleWindows(from) &&!thanos.getActivityManager().hasTopVisibleActivities(from)){activity.setInactive(from);su.exe(\"appops set \" + from + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ from +\"`\")};} ;"
        ]
    }
]
```

### <i id="冻结(Color优化版)"></i>1-1(2). 冻结(Color优化版)

```
[
    {
        "name": "NoActive(全局变量白名单)冻结",
        "description": "应用进入后台冻结进程,白名单为全局变量whiteApps",
        "priority":1,
        "delay":700,
        "condition": "frontPkgChanged && thanos.getActivityManager().isPkgSmartStandByEnabled(from)&& !globalVarOf$whiteApps.contains(from)",
        "actions": [
            "if(thanos.getAudioManager().hasAudioFocus(from)){su.exe(\"dumpsys deviceidle whitelist +\"+from)}else{su.exe(\"dumpsys deviceidle whitelist\"+from);if(activity.getFrontAppPackage()!=from && !thanos.windowManager.hasVisibleWindows(from) &&!thanos.getActivityManager().hasTopVisibleActivities(from)){activity.setInactive(from);su.exe(\"appops set \" + from + \" WAKE_LOCK ignore\");su.exe(\"psef|grep \"+ from +\"|grepv grep|awk '{print $2}'|xargs kill19\")};} ;"
                ]
    }
]
```

### <i id="解冻"></i>1-2. 解冻

```
[
    {
        "name": "NoActive解冻",
        "description": "解冻被冻结进程",
        "priority":1,
        "condition": "frontPkgChanged && thanos.getActivityManager().isPkgSmartStandByEnabled(to)&&!globalVarOf$whiteApps.contains(to)",
        "actions": [ "su.exe(\"appops set \" +to + \" WAKE_LOCK default\");","su.exe(\"killCONT `pgrepf \"+ to + \"`\");"
        ]
    }
]
```

## <i id="黑名单版"></i>2. 黑名单版
- 注意事项
> 1. 黑名单为乖巧名单但乖巧总开关可不开
> 2. 全局变量为：blackapps
> 3. 黑名单应用需关闭应用 “唤醒锁(Thanox的唤醒锁拦截)”，否则概率性息屏无法休眠
> 4. 该情景模式只有1条

### <i id="墓碑۩(主情景)黑名单版"></i>2-1墓碑۩(主情景)黑名单版

```
[
    {
        "name": "墓碑۩(主情景)黑名单版",
        "description": "应用进入后台冻结进程，应用回到前台解冻进程,黑名单为全局变量blackapps",
        "priority":1,
        "delay":1000,
        "condition": "if(frontPkgChanged){if(globalVarOf$blackapps.contains(to)){su.exe(\"appops set \" +to + \" WAKE_LOCK default\");su.exe(\"killCONT `pgrepf \"+ to + \"`\")};return true}",
        "actions": [
            "if(su.exe(\"find /data/system/name window\").out==[]){io.write(\"/window\",\"\")}","if(io.read(su.exe(\"find /data/system/name window\").out[0]).contains(\".\")&&activity.getFrontAppPackage()!=io.read(su.exe(\"find /data/system/name window\").out[0]) && !thanos.windowManager.hasVisibleWindows(io.read(su.exe(\"find /data/system/name window\").out[0])) &&!thanos.getActivityManager().hasTopVisibleActivities(io.read(su.exe(\"find /data/system/name window\").out[0]))){su.exe(\"appops set \" + io.read(su.exe(\"find /data/system/name window\").out[0]) + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ io.read(su.exe(\"find /data/system/name window\").out[0]) +\"`\");io.write(\"/window\",\"\")};",
            "if(globalVarOf$blackapps.contains(from)){if(activity.getFrontAppPackage()!=from && !thanos.windowManager.hasVisibleWindows(from) &&!thanos.getActivityManager().hasTopVisibleActivities(from)){if(!thanos.getAudioManager().hasAudioFocus(from)){activity.setInactive(from);su.exe(\"appops set \" + from + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ from +\"`\")}}else{io.write(\"/window\",from)};} ;"
        ]
    }
]
```

## <i id="优化版"></i>3. 优化版
- 注意事项
> 1. 第一次使用，添加 ——> 首次使用，打开后切换一次应用后 关闭 ——> 首次使用
> 2.  墓碑۩（主情景）
> 	 2-1 添加墓碑۩（主情景），单独添加主情景可以满足基本墓碑需求，应用进入后台冻结进程，应用回到前台解冻进程,
> 	 2-2 白名单全局变量：whiteApps。
> 3. 墓碑۩(通知)
> 	 3-1 添加墓碑۩(通知)，例如下载类或播放器应用，可实现存在通知忽略墓碑，移除通知自动墓碑。
> 	 3-2 全局变量为：noti

### <i id="首次使用"></i>3-1. 首次使用

```
[
    {
        "name": "首次使用",
        "description": "打开之后，切换一次应用，之后关闭",
        "priority": 2,
        "condition": " frontPkgChanged == true  ",
        "actions": [ 
                "if(su.exe(\"find /data/system/name window\").out==[]){io.write(\"/window\",\"\")}"
                ]
    }
]
```

### <i id="墓碑۩(主情景)"></i>3-2. 墓碑۩(主情景)

```
[
    {
        "name": "墓碑۩(主情景)",
        "description": "应用进入后台冻结进程，应用回到前台解冻进程,黑名单为乖巧名单,白名单为全局变量whiteApps",
        "priority":1,
        "delay":1000,
        "condition": "if(frontPkgChanged){if(thanos.getActivityManager().isPkgSmartStandByEnabled(to) && !globalVarOf$whiteApps.contains(to)){su.exe(\"appops set \" +to + \" WAKE_LOCK default\");su.exe(\"killCONT `pgrepf \"+ to + \"`\")}return true}",
        "actions": [
            "if(io.read(su.exe(\"find /data/system/name window\").out[0]).contains(\".\")&&activity.getFrontAppPackage()!=io.read(su.exe(\"find /data/system/name window\").out[0]) && !thanos.windowManager.hasVisibleWindows(io.read(su.exe(\"find /data/system/name window\").out[0])) &&!thanos.getActivityManager().hasTopVisibleActivities(io.read(su.exe(\"find /data/system/name window\").out[0]))){su.exe(\"appops set \" + io.read(su.exe(\"find /data/system/name window\").out[0]) + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ io.read(su.exe(\"find /data/system/name window\").out[0]) +\"`\");io.write(\"/window\",\"\")};",
            "if(thanos.getActivityManager().isPkgSmartStandByEnabled(from) && !globalVarOf$whiteApps.contains(from)){if(activity.getFrontAppPackage()!=from && !thanos.windowManager.hasVisibleWindows(from) &&!thanos.getActivityManager().hasTopVisibleActivities(from)){if(!thanos.getAudioManager().hasAudioFocus(from)){activity.setInactive(from);su.exe(\"appops set \" + from + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ from +\"`\")}}else{io.write(\"/window\",from)};} ;"
            ]
    }
]
```

### <i id="墓碑۩(通知)"></i>3-3. 墓碑۩(通知)

```
[
  {
    "name": "墓碑۩(通知)",
    "description": "创建全局变量noti,添加应用，应用存在通知则忽略墓碑,移除通知后移动墓碑，",
    "priority": 1,
    "condition": "if(notificationAdded==true&&globalVarOf$noti.contains(pkgName)){if(thanos.getActivityManager().isPkgSmartStandByEnabled(pkgName)){thanos.getActivityManager().setPkgSmartStandByEnabled(pkgName,false)}};if(notificationRemoved==true&&globalVarOf$noti.contains(pkgName)){if(!thanos.getActivityManager().isPkgSmartStandByEnabled(pkgName)){thanos.getActivityManager().setPkgSmartStandByEnabled(pkgName,true)};if(activity.getFrontAppPackage()!=pkgName && !thanos.windowManager.hasVisibleWindows(pkgName) &&!thanos.getActivityManager().hasTopVisibleActivities(pkgName)){activity.setInactive(pkgName);su.exe(\"appops set \" + pkgName + \" WAKE_LOCK ignore\");su.exe(\"killSTOP `pgrepf \"+ pkgName +\"`\")};}",
    "actions": [
      ""
    ]
  }
]
```

PS：欢迎向我投稿更多实用的情景模式哦，可以在这里告诉我：[**`投稿`**](https://www.coolapk.com/feed/45352543?shareKey=MjM5NmVlNTIyN2IxNjQ0MzUwYzM~&shareUid=3571197&shareFrom "**`投稿`**")