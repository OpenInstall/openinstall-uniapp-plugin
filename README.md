## 一、插件介绍
此插件是 openinstall 为了方便 uni-app 集成使用 openinstall 功能而开发的，实现了携带参数安装，渠道统计(H5渠道、广告平台渠道、ASA渠道等)，一键拉起全部功能。

`openinstall` 可帮助 Android/iOS 开发者精确的获取 App 每一次安装来源；在 App 安装或拉起后，直达指定场景，大大提高用户转化率和留存率。同时，openinstall 在精准的 app 安装来源跟踪的技术上，开发了免打包，跨平台的渠道统计功能，轻松创建与管理成千上万的渠道，实现线上线下全渠道覆盖。openinstall 统计数据完全独立于第三方平台，提供多维度的统计报表，实时客观地反映渠道效果。

## 二、集成准备
前往 [openinstall 官网](https://www.openinstall.io/)，注册账户，登录管理控制台，创建应用后，跳过 "集成指引"，在 "应用集成" 的对应平台的 "应用配置" 中获取 `appkey` 和 `scheme` 以及 iOS 的关联域名。

![获取appkey和scheme](https://res.cdn.openinstall.io/doc/ios-appkey.png)

针对使用了 渠道统计 功能中的 广告平台渠道/ASA渠道 效果监测功能的集成，需要参考 [补充文档](#ad)

> **注意：** 为了适配苹果最新的ASA渠道统计API，插件版本1.5.4及以上版本中，iOS插件编译环境为Xcode 12.3。

#### 配置appkey
在 mainfest.json 的 **App原生插件配置** 的 openinstall 插件配置框内配置 `openinstall` 分配给应用的 `AppKey`  
![设置appkey](https://res.cdn.openinstall.io/doc/uniapp-appkey.jpg)
#### 配置scheme
在 mainfest.json 的 **App常用其它配置** 中配置 `openinstall` 分配给应用的 `scheme`    
![设置scheme](https://res.cdn.openinstall.io/doc/uniapp-scheme.jpg)
#### 配置universal links（iOS平台）

HBuilderX2.3.0开始云端打包支持配置XCode中的Capabilities [参考文档](https://ask.dcloud.net.cn/article/36393)  
在HBuilderX中的manifest.json中配置关联域名：  
``` xml
"capabilities": {  
    "entitlements": {  
        "com.apple.developer.associated-domains": [  
            "openinstall分配给应用的关联域名"  
        ]  
    }  
}
```
示例如图：  
![配置关联域名](https://res.cdn.openinstall.io/doc/uniapp-ulink.png)

## 三、使用教程

#### 引用
``` js
const openinstall = uni.requireNativePlugin('openinstall-plugin');
```

#### 初始化
`init()`    
示例：在 `App.vue` 的 `onLaunch` 方法中进行初始化
``` js
openinstall.init();
```
>**注意：** 1.3.2版本加入，必须先进行初始化，才能调用其他api

#### 获取安装数据
`getInstall(seconds, callback)`
- `seconds` : 回调超时时间
- `callback` : 数据回调函数

示例：
``` js
openinstall.getInstall(
    8,
    function(result) {
        console.log('getInstall : channel=' + result.channelCode + ', data=' + result.bindData);
    }
);
```

#### 获取拉起数据
 
`registerWakeUp(callback)`  
- `callback` : 数据回调函数  

示例：
在 `App.vue` 的 `onLaunch` 方法中注册拉起回调（在初始化之后调用 ） 
``` js
openinstall.registerWakeUp(function(result){
    console.log('getWakeup : channel=' + result.channelCode + ', data=' + result.bindData);
});
```
#### 注册量统计
`reportRegister()`  
示例：
``` js
openinstall.reportRegister();
```

#### 效果点统计
`reportEffectPoint(effectPointId, effectPointValue)`
- `effectPointId` : 效果点ID
- `effectPointValue` : 效果点值，数值类型

示例：
``` js
openinstall.reportEffectPoint("effect_test", 1);
```

## 四、导出apk/ipa包并上传
- 代码集成完毕后，需要导出安装包上传openinstall后台，openinstall会自动完成所有的应用配置工作。  
![上传安装包](https://res.cdn.openinstall.io/doc/upload-ipa-jump.png)
- 上传完成后即可开始在线模拟测试，体验完整的App安装/拉起流程；待测试无误后，再完善下载配置信息。
![在线测试](https://res.cdn.openinstall.io/doc/js-test.png)



## 如有疑问

若您在集成或使用中有任何疑问或者困难，请 [咨询openinstall客服](https://www.openinstall.io/)。 


----- 

<a id="ad"></a>

## 广告接入补充文档

### Android平台

（1）针对广告平台接入，新增配置接口，在调用 init 之前调用。参考 [广告平台对接Android集成指引](https://www.openinstall.io/doc/ad_android.html)
``` js
var options = {
    adEnabled: true,
}
openinstall.configAndroid(options);
```
options 参数如下：

| 参数名| 参数类型 | 描述 |  
| --- | --- | --- |
| adEnabled| bool | 广告平台接入开关（必须） |
| macDisabled | bool | 是否禁止 SDK 获取 mac 地址 |
| imeiDisabled | bool | 是否禁止 SDK 获取 imei |
| gaid | string | 通过 google api 获取到的 advertisingId，SDK 将不再获取gaid |
| oaid | string | 通过移动安全联盟获取到的 oaid，SDK 将不再获取oaid |


（2） 为了精准地匹配到渠道，需要获取设备唯一标识码，因此需要在 `manifest.json` 中声明权限，在 “App模块权限配置” 的 “Android打包权限配置” 勾选上 `<uses-permission android:name="android.permission.READ_PHONE_STATE"/>`

（3）在 `manifest.json` 中设置，关闭 `uni-app` 自动获取 `android.permission.READ_PHONE_STATE` 权限  
``` json
    "permissionPhoneState" : {
        "request" : "none",
        "prompt" : ""
    },        
```
示例如图：  
![设置权限](https://res.cdn.openinstall.io/doc/uniapp-permission.png)

（4）在 `App.vue` 的 `onLaunch` 方法中进行初始化，先申请权限，之后进行配置和初始化
``` js
if (plus.os.name == "Android") {
    plus.android.requestPermissions(["android.permission.READ_PHONE_STATE"], function(event) {
        if(event.granted){
            console.log(event.granted);
        }
        if(event.deniedPresent){
            console.log(event.deniedPresent);
        }
        if(event.deniedAlways){
            console.log(event.deniedAlways);
        }
        // 配置初始化，设置 OAID 和 GAID
        var options = {
            adEnabled: true,
		    oaid: null,
		    gaid: null,
        }
        openinstall.configAndroid(options);
        // 权限申请成功，不管用户是否同意，都需要做初始化
        openinstall.init();
        // 初始化完成后，才能做其他api调用
        openinstall.registerWakeUp(function(result) {
            console.log('wakeup : channel=' + result.channelCode + ', data=' + result.bindData);
        });
    }, function(event) {
        // 权限申请错误
    })
}
```

### iOS平台
包括广告平台渠道统计和ASA渠道：

**广告平台渠道：**

openinstall后台配置及上架指引可参考文档 [广告平台对接iOS集成指引](https://www.openinstall.io/doc/ad_ios.html)

（1）需在`manifest.json`的 “App常用其它设置” 中配置勾选 “使用广告标识（IDFA）”

（2）下载官方插件[iOS平台获取idfa](https://ext.dcloud.net.cn/plugin?id=726)，并保存到目录下，如js_sdk/dc-idfa/idfa.js

（3）在 `App.vue` 中引用此模块，如下：
``` js
import idfa from "@/js_sdk/dc-idfa/idfa.js"
```

（4）在 `App.vue` 的 `onLaunch` 方法中进行初始化，如下：
``` js
if('iOS' == plus.os.name){
    var fid = "";
    fid = idfa.value();//如果需要idfa则开启
    openinstall.init(fid);
    console.log(fid);
};
```

**ASA渠道：**

插件1.5.4及以上版本增加了ASA渠道统计功能，已经集成在SDK内部，不需要增加额外代码。开发者可以通过`getInstall`方法获取`channelCode`对应的值，即为ASA渠道编号。  
区分ASA渠道和H5渠道及其它渠道，可在openinstall开发者平台创建渠道时，创建不同的渠道编号来做区分，如`asa+广告系列ID`，例子：`asa_1234567890`。

如何在openinstall开发者平台创建ASA渠道，并实现统计功能和上架前测试，请看图文文档 [ASA渠道使用指南](https://www.openinstall.io/doc/asa.html)
