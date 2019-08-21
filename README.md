## 插件介绍
此插件是 openinstall 为了方便 uni-app 集成使用 openinstall 功能而开发的，实现了携带参数安装，渠道统计，一键拉起全部功能。

`openinstall` 可帮助 Android/iOS 开发者精确的获取 App 每一次安装来源；在 App 安装或拉起后，直达指定场景，大大提高用户转化率和留存率。同时，openinstall 在精准的 app 安装来源跟踪的技术上，开发了免打包，跨平台的渠道统计功能，轻松创建与管理成千上万的渠道，实现线上线下全渠道覆盖。openinstall 统计数据完全独立于第三方平台，提供多维度的统计报表，实时客观地反映渠道效果。

## 使用前准备
前往 [openinstall 官网](https://www.openinstall.io/)，注册账户，登录管理控制台，创建应用后，跳过 "集成指引"，在 "应用集成" 的对应平台的 "应用配置" 中获取 `appkey` 和 `scheme` 以及 iOS 的关联域名。

## 配置

#### 配置appkey
在 mainfest.json 的 **App原生插件配置** 的 openinstall 插件配置框内配置 `openinstall` 分配给应用的 `AppKey`

#### 配置scheme
在 mainfest.json 的 **App常用其它配置** 中配置 `openinstall` 分配给应用的 `scheme`  

#### 配置universal links（iOS平台）
此插件暂不支持配置 **通用链接** ，如需实现通用连接功能请下载插件，修改 `openinstall-plugin` 本地插件下的 `package.json` 中的配置，通过离线打包来实现

## 使用教程

#### 引用
``` js
const openinstall = uni.requireNativePlugin('openinstall-plugin');
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
