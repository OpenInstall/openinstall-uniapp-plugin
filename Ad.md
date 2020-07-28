
### Android平台

（1） 在 `manifest.json` 中声明权限，在 “App模块权限配置” 的 “Android打包权限配置” 勾选上 `<uses-permission android:name="android.permission.READ_PHONE_STATE"/>`

（2）在 `manifest.json` 中设置，关闭 `uni-app` 自动获取 `android.permission.READ_PHONE_STATE` 权限
``` json
{
    "app-plus" : {
        "distribute" : {
            "android" : {
                "permissionPhoneState" : {
                    "request" : "none",
                    "prompt" : ""
                },
            }
        }
    }
}
                
```
（3）在 `App.vue` 的 `onLaunch` 方法中进行初始化，在初始化之前申请权限
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
        // 权限申请成功，不管用户是否同意，都需要做初始化
        openinstall.init();
    }, function(event) {
        // 权限申请错误
    })
}
```

### iOS平台

（1）需在manifest.json的 “App常用其它设置” 中配置勾选 “使用广告标识（IDFA）”

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
