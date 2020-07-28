## 一、插件介绍
此插件是 openinstall 为了方便 uni-app 集成使用 openinstall 功能而开发的，实现了携带参数安装，渠道统计，一键拉起全部功能。

`openinstall` 可帮助 Android/iOS 开发者精确的获取 App 每一次安装来源；在 App 安装或拉起后，直达指定场景，大大提高用户转化率和留存率。同时，openinstall 在精准的 app 安装来源跟踪的技术上，开发了免打包，跨平台的渠道统计功能，轻松创建与管理成千上万的渠道，实现线上线下全渠道覆盖。openinstall 统计数据完全独立于第三方平台，提供多维度的统计报表，实时客观地反映渠道效果。

## 使用前准备
前往 [openinstall 官网](https://www.openinstall.io/)，注册账户，登录管理控制台，创建应用后，跳过 "集成指引"，在 "应用集成" 的对应平台的 "应用配置" 中获取 `appkey` 和 `scheme` 以及 iOS 的关联域名。

针对使用了 渠道统计 功能中的 广告渠道 效果监测功能的集成，需要参考 [补充文档](./Ad.md)
## 二、配置

#### 配置appkey
在 mainfest.json 的 **App原生插件配置** 的 openinstall 插件配置框内配置 `openinstall` 分配给应用的 `AppKey`

#### 配置scheme
在 mainfest.json 的 **App常用其它配置** 中配置 `openinstall` 分配给应用的 `scheme`  

#### 配置universal links（iOS平台）

HBuilderX2.3.0开始云端打包支持配置XCode中的Capabilities [参考文档](https://ask.dcloud.net.cn/article/36393)  
在HBuilderX中的manifest.json中配置域名：  
在"plus" -> "distribute" -> "apple" -> "capabilities" -> "entitlements"节点（uni-app项目在"app-plus" -> "distribute" -> "ios" -> "capabilities" -> "entitlements"）下添加"com.apple.developer.associated-domains"字段，字段值为字符串数组，每个字符串为要关联的域名

``` xml
    "capabilities": {  
        "entitlements": {  
            "com.apple.developer.associated-domains": [  
                "openinstall分配给应用的关联域名"  
            ]  
        }  
    }
```

- openinstall完全兼容微信openSDK1.8.6以上版本的通用链接跳转功能，HBuilderX -> manifest.json -> App SDK配置，请在对应的微信登录/分享/支付中填入正确格式的universal link链接，参考 [iOS常见问题](https://www.openinstall.io/doc/ios_sdk_faq.html)

## 三、使用教程

#### 引用
``` js
const openinstall = uni.requireNativePlugin('openinstall-plugin');
```

#### 初始化
示例：  
在 `App.vue` 的 `onLaunch` 方法中进行初始化
``` js
openinstall.init();
```

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
在 `App.vue` 的 `onLaunch` 方法中注册拉起回调 
``` js
openinstall.registerWakeUp(function(result){
    console.log('getWakeup : channel=' + result.channelCode + ', data=' + result.bindData);
});
```
#### 注册量统计
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

## 四、导出apk/api包并上传
- 代码集成完毕后，需要导出安装包上传openinstall后台，openinstall会自动完成所有的应用配置工作。  
- 上传完成后即可开始在线模拟测试，体验完整的App安装/拉起流程；待测试无误后，再完善下载配置信息。
