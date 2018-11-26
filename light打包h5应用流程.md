# 使用light打包h5应用App流程

[light相关文档链接](https://document.lightyy.com/lighting_ref/index.html)

## 一、light包准备

#### 直接使用h5包形式

1. h5项目打包获得dist文件
2. 按照文档安装依赖环境及lighting工具
3. 按照文档创建开发工程（light create -n AppDemo -t app）
4. 将dist目录下文件拷贝至AppDemo根目录下
5. 修改native——config.js文件中navBar属性（对应应用中导航栏的样式）
6. 按照文档编译和打包工程（light release --product -p）
7. 编辑解压后打包后文件夹中的Resource——gmu——web.gmu文件（navigationbar属性的show为true时，后续集成出来的应用才有导航栏）
8. 如上一步需要修改，则重新压缩为App包

#### 访问服务器形式

1. h5项目打包获得dist文件
2. 将h5包上传至对应服务器（访问地址为“url”）
3. 按照文档创建开发工程（light create -n AppDemo -t app）
4. 修改native——config.js文件中navBar属性（对应应用中导航栏的样式）
5. 按照文档编译和打包工程（light release --product -p）
6. 编辑解压后打包后文件夹中的Resource——gmu——web.gmu文件（pages——inputParams——startPage修改为“url” + “?open_by_device=true”，此为直接访问对应服务器地址）（navigationbar属性的show为true时，后续集成出来的应用才有导航栏）
7. 重新压缩为App包

## 二、构建应用

#### light平台集成
1. [登录light管理平台](https://passport.hscloud.cn/iuccasserver/login?service=https%3A%2F%2Flight.hscloud.cn%2Fportal%2Fhome&showLogo=true)
2. 在App模块中创建应用（如“新应用”）
3. 在“新应用”中创建版本并点击“上传包”，此包为App包。
4. 上传完成后选择集成（可设置App name），可同时选择iOS和Android两个版本
5. 集成完成后在“记录”中获取对应的安装二维码安装应用

## 三、更多操作
1. 修改应用图标：项目文件中修改native——assets——res——icon.png（引用配置在native——config.js文件中）
2. 修改启动页：项目文件中修改native——assets——res——launch.png（引用配置在native——config.js文件中）
3. 新增引导页：[参考文档](https://document.lightyy.com/app_dev_jsn/content/she_zhi_icon_yu_qi_dong_tu.html)

**备注**：
1. 由于light版本会更新，则代码结构可能会跟上述不一致
2. light升级命令（针对创建工程目录更改，集成失败）npm install -dg lighting、light pulgin -a type-vue、light plugin -a native


