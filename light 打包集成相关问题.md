# Light 打包集成相关问题

> 此文档是 [使用Light打包h5应用App流程](https://github.com/cloud-templates/blog/blob/master/light%E6%89%93%E5%8C%85h5%E5%BA%94%E7%94%A8%E6%B5%81%E7%A8%8B.md?1543386430740) 一个补充。

相关的 Light 文档：
[Light](https://document.lightyy.com/app_dev_jsn/index.html)

## 创建工程

```
$ light create -n <your project> -t app
```

创建好的工程目录如下：

```
├─native
│  └─res
└─view
```
*由于是混合式开发，因此前端关心的主要就是 `/native/config.js` 里的配置，尤为重要*

`config.js` 里的主要配置如下：

```
//配置文件的定义
module.exports = {
    res:{
        logo:"/native/res/icon.png",
        launch:"/native/res/launch.png",
    },
    menuBar:{
        menus:[{
            view:"index"
        }]
    },
    navBar:{
        backgroundColor:"#de302f",
        titleColor:"#ffffff",
        buttonColor:"#ffffff"
    },
    views:{
        "index":{
            url:"index.html"
        }
    }
};
```

这里包含了相关静态资源文件路径，如 logo，启动图，引导图等，及菜单栏，导航栏，视图的相关配置，详情可见相关文档，[传送门](https://document.lightyy.com/app_dev_jsn/content/she_zhi_introduction.html)

下面以 **太平洋投顾端APP** 为例来说明，[SVN路径地址](https://192.168.57.168/BrokerNet/VIPSTU/trunk/Sources/web/ifsstu/light-app-manage-pse)

- 利用壳子内置对象 `LightJSBridge` 提供的相关桥接方法（可以参考 `web/ifsstu/h5-app-manage-pse/src/modules/LightJSBridge.js` d的封装），做到H5与原生端进行交互。 如
```
LightJSBridge.call("head.setBackgroundColor", params || {}, null);
```

*这个对象提供的相关方法，在 Light 官方文档上并没有找到说明。现在开发可以用提供的 `light-sdk` 详见[文档](https://document.lightyy.com/app_jssdk_ref/index.html)*

- 需要配置**标题栏（navbar）**左右图标按钮

- 需要调用**扫一扫**的功能，**推送**，及**摄像头**的功能

最终 `config.js` 配置如下：

```
//配置文件的定义
module.exports = {
    // 相关静态资源路径
    res:{
        logo:"/native/res/icon.png",
        launch:"/native/res/launch.png",
        
        // 引导图
        guide:["/native/guide/0.png","/native/guide/1.png", "/native/guide/2.png"],
        
        // 配置插件的 icon，如标题栏左右两边自定义的图标
        plugins:[
            "/native/icon/add.png",
            "/native/icon/add@2x.png",
            "/native/icon/back.png",
            "/native/icon/back@2x.png",
            "/native/icon/menu.png",
            "/native/icon/menu@2x.png",
            "/native/icon/qr-code.png",
            "/native/icon/qr-code@2x.png",
            "/native/icon/question.png",
            "/native/icon/question@2x.png",
            "/native/icon/search.png",
            "/native/icon/search@2x.png",
            "/native/icon/username.png",
            "/native/icon/username@2x.png",
            "/native/icon/wen.png",
            "/native/icon/wen@2x.png",
        ]
    },
    menuBar:{
        menus:[{
            // text 尽量不为空
            text: '首页',
            view:"index"
        }]
    },
    navBar:{
        backgroundColor:"#3F4650",
        titleColor:"#ffffff",
        // 配置的 icon 图标的颜色
        buttonColor: "#ffffff",
        // 标题栏及状态栏都会显示
        type: "1"
    },
    views:{
        "index":{
            // 项目部署的绝对路径，不随壳子一起打包
            url:"http://10.26.170.48:3000/#/homePage?open_by_device=true"
        }
    },
    plugins: {
        "scanning":{
            "inputParams": {
                "title":"扫一扫"
            },
            "outputParam": [

            ],
            "navigationbar": {
                "show": false,
                "left_item": {
                },
                "right_item": {
                }
            },
            "menu": {
                "show": false
            },
            "style": {

            },
            "config": {
            }
        },
        "push": {
            "config": {
                "jpush_appKey": "70a6753960b82b8081a76f32"  //极光推送平台的AppKey
            }
        },
        // 调用摄像头，是需要这个配置的
        "imageacquisition": {}
    }
};
```


## 编译打包

```
$ light release --product -p
```
简单介绍下关键的地方：

+ `App/Info/` 下存放的是多个尺寸的安装的应用图标及启动页。
+ `App/Resource/` 下有两个目录 `gumu` 和 `www`

  - `gmu` 目录存放的是生成的相关协议文件，是根据 `config.js` 里的各个属性配置生成的。
    
    ```
    │  imageacquisition.gmu    // plugins -> imageacquisition 调用摄像头生成
    │  jsnative.gmu           // 默认生成
    │  main.gmu              // 默认生成
    │  push.gmu             // plugins -> push 推送
    │  scanning.gmu        // plugins -> scanning 扫一扫
    │  web.gmu            // 默认生成  
    │                        
    └─gmu_icon          // gmu 相关图标，根据 res -> plugins 生成     
            add.png          
            add@2x.png       
            back.png         
            back@2x.png      
            menu.png         
            menu@2x.png      
            qr-code.png      
            qr-code@2x.png   
            question.png     
            question@2x.png  
            search.png       
            search@2x.png    
            username.png     
            username@2x.png  
            wen.png          
            wen@2x.png       
    ```
  
  - `www` 目录存放的是H5的项目工程
  
  ```
    │  app.js
    │  index.html
    │  project.json
    │
    └─splash      // 这里是放置的引导图，对应的是 res -> guide
            0.png
            1.png
            2.png
  ```
  