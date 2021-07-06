### 项目安装

本项目不用安装任何第三方软件，非常简单轻便，仅需三行命令即可运行

> 首先 clone 项目仓库（在此之前你需要安装并掌握 [nodejs](https://nodejs.org) 的相关知识）

```
git clone https://github.com/dcyuki/yumemi_bot.git

// 若网速较慢建议使用 Gitee
git clone https://gitee.com/dc_yuki/yumemi_bot.git
```

> 使用 npm 安装 node 依赖模块

```
npm i

// 若网速较慢建议使用 cnpm
cnpm i
```

### 目录结构

> 修改`yumemi_bot/data/db/yumemi.db.example`数据库文件为`yumemi.db`  
> 修改`yumemi_bot/config_example`目录名称为`config`，并正确添加`bots/bot_example.yml`文件所对应的参数

bot.yml 配置参数
```yaml
# bot 账号
qq:
  # 主  人 QQ 账号
  masters: <your master qq number array>
  # 机器人 QQ 账号
  uin: <your bot qq number>
```

!> 修改完成之后你可以将文件名修改任意数值 ~~主要是为了你自己方便识别~~  
一般建议用 qq 号作为唯一标识，例如：`123456789.yml`

- **什么是 YAML**
  + YAML 是一种专攻配置的语言，可读性高（JSON 有时确实让人眼花缭乱不是么？）
  + 本项目所有的配置文件均使用 YAML 编写，你也可以查看 [YAML 入门教程](https://www.runoob.com/w3cnote/yaml-intro.html) 获取相关信息

项目文件结构
```
yumemi_bot
├─ config                  配置信息
│  ├─ bots                   bot 目录
│  │  └─ xxx.yml               QQ 号等信息（可存放多个）
│  ├─ groups                 群聊目录（插件参数等配置）
│  │  └─ xxx.yml               账号登录成功时会自动校验生成，无需手动配置
│  ├─ api.yml                aip 配置
│  ├─ boss.yml               会战信息
│  ├─ chat.yml               bot 词库
│  ├─ cmd.yml                正则配置
│  ├─ info.yml               版本信息
│  ├─ params.yml             插件参数（插件默认参数配置文件）
│  ├─ sql.yml                sql 语句
│  └─ web.yml                端口配置
├─ data                    资源目录
│  ├─ db                     数据库文件
│  ├─ dynamic                动态资源
│  └─ images                 图片资源
├─ plugins                 插件目录（存放编写好的插件）
└─ services                服务目录
```

### 启动程序

```
npm start
```

> 如上述步骤无误，根据控制台的提示输入密码及对应验证即可成功登录  
> bot 登录成功后会在`data`目录下，自动生成`oicq`文件夹存放 QQ 账号相关缓存文件  
> 当然，如果你有`js`的相关知识，随时都可以编写自己的插件，详情可在 [插件开发](develop/) 一栏查看

### 你好世界

登录成功后你将会收到一条私信（你填写的 masters）

![wellcome](../public/images/demo/wellcome.png)

!> 所有插件 **默认不会启用** ，在群内发送`开启群服务`即可全部打开，若只想启用部分模块，可发送`list`查看服务列表自行启用  

![join](../public/images/demo/join.png)

你可以输入`>enable <模块名>`来启用插件  
当然，如果你不习惯使用命令行的方式来操作，也可以输入`打开 <模块名>`

>更多关于插件的控制可以查看 [插件管理](plugin/) 一栏

![enable](../public/images/demo/enable.png)