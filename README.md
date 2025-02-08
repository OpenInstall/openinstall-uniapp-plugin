## 一、插件介绍
此插件是 openinstall 为了方便 uni-app 集成使用 openinstall 功能而开发的，实现了携带参数安装，渠道统计(H5渠道、广告平台渠道、ASA渠道等)，一键拉起全部功能。

**如有集成问题，请联系技术支持QQ：800853853**

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
1、开启Associated Domains服务  
需要在苹果开发者后台开启 [苹果开发者后台](https://developer.apple.com)  

2、在HBuilderX里面配置关联域（Associated Domains）  
示例如图：  
![配置关联域名](https://res.cdn.openinstall.io/doc/uniapp-applinks.png)  

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
        console.log('getInstall : channel=' + result.channelCode + ', data=' + result.bindData 
            + ', shouldRetry=' + result.shouldRetry);
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

#### 效果点明细统计
`reportEffectPoint(pointId, pointValue, extras)`
- `pointId` : 效果点ID
- `pointValue` : 效果点值，数值类型
- `extras` : 效果点自定义参数和值,key和value都必须是string类型

示例：
``` js
var extras = {
    "key1": "value1",
    "key2": "value2",
}
openinstall.reportEffectPoint("effect_detail", 1, extras);
```

#### 裂变分享上报
`reportShare(shareCode, sharePlatform, callback)`
- `shareCode` : 分享标识码（用户标识）
- `sharePlatform` : 分享平台，参考官网定义的平台字符串
- `callback` : 上报回调函数
示例：
``` js
openinstall.reportShare("10001", "QQ", function(ret) {
    console.log('shouldRetry = ' + ret.shouldRetry + ', message = ' + ret.message);
    if(ret.shouldRetry){
        console.log('上报失败，请重试');
    }
});
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

针对广告平台接入，新增配置接口，需要在调用 init 之前调用。参考 [广告平台对接Android集成指引](https://www.openinstall.io/doc/ad_android.html)
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
| ~~macDisabled~~ | bool | （废弃）是否禁止 SDK 获取 mac 地址 |
| ~~imeiDisabled~~ | bool | （废弃）是否禁止 SDK 获取 imei |
| imei | string | 通过系统api获取到的 imei，SDK 将不再获取 |
| macAddress | string | 通过系统api获取到的 macAddress，SDK 将不再获取 |
| gaid | string | 通过 google api 获取到的 advertisingId，SDK 将不再获取gaid |
| oaid | string | 通过移动安全联盟获取到的 oaid，SDK 将不再获取oaid |

只要将 `adEnabled` 设置为 `true`，SDK 内部会获取广告需要的设备信息；一旦传入对应的参数，SDK 则会使用传入的参数

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

插件1.5.4及以上版本增加了ASA渠道统计功能，已经集成在SDK内部，只需配置增加少量代码。开发者可以通过`getInstall`方法获取`channelCode`对应的值，即为ASA渠道编号。  
区分ASA渠道和H5渠道及其它渠道，可在openinstall开发者平台创建渠道时，创建不同的渠道编号来做区分，如`asa+广告系列ID`，例子：`asa_1234567890`。

``` js
//配置asa,也可同时配置idfa
//isDev:"1"是测试模式，可以测试1234567890渠道下的统计数据，正式环境下请去掉该设置
var options = {
    asa: "1",
    isDev: "1",
    //idfa: null,
}
openinstall.initAdsAttribution(options);
```

如何在openinstall开发者平台创建ASA渠道，并实现统计功能和上架前测试，请看图文文档 [ASA渠道使用指南](https://www.openinstall.io/doc/asa.html)
