### 项目安装

本项目不用安装任何第三方软件，非常简单轻便，仅需三行命令即可运行

> 首先 clone 项目仓库（在此之前你需要安装并掌握 [nodejs](https://nodejs.org) 的相关知识）

```
// 国内用户建议使用 Gitee
git clone https://github.com/dcyuki/yumemi_bot.git

git clone https://gitee.com/dc_yuki/yumemi_bot.git
```

> 使用 npm 安装 node 依赖模块

```
npm install
```

### 目录结构

> 修改`data/db/yumemi.db.example`数据库文件为`yumemi.db`  
> 修改`config_example`目录名称为`config`，并正确添加`bot.yml`文件所对应的参数

bot.yml 配置参数
```yaml
# bot 账号
qq:
  # 维护组 QQ 账号
  admin: 2225151531
  # 主  人 QQ 账号
  master: <your master qq number>
  # 机器人 QQ 账号
  account: <your bot qq number>
  # 机器人 QQ 密码
  password: <your bot qq password or 32bit md5>

# web 服务
web:
  # 端口号
  port: 80
  # 域  名
  domain: <you domain name or ip address>
```

- **什么是 YAML**
  + YAML 是一种专攻配置的语言，可读性高（JSON 有时确实让人眼花缭乱不是么？）
  + 本项目所有的配置文件均使用 YAML 编写，你也可以查看 [YAML 入门教程](https://www.runoob.com/w3cnote/yaml-intro.html) 获取相关信息

项目文件结构
```
YumemiBot
├─ yumemi                     bot 目录
│  ├─ config                  配置信息
│  │  ├─ api.yml              aip 配置
│  │  ├─ boss.yml             会战信息
│  │  ├─ bot.yml              基本参数（QQ 号、群号等信息）
│  │  ├─ chat.yml             bot 词库
│  │  ├─ cmd.yml              正则配置
│  │  ├─ group.yml            群聊配置（群内有人发送消息后会自动添加，无需手动配置）
│  │  └─ param.yml            插件参数（模块多参数配置文件）
│  ├─ data                    资源目录
│  │  ├─ db                   数据库文件
│  │  └─ images               图片资源
│  ├─ plugins                 插件目录（存放编写好的插件）
│  ├─ serve.js                消息监听处理
│  └─ tools.js                自定义工具类
└─ app.js                     程序主入口（用于登录 QQ）
```

> 如上述步骤无误  
> bot 启动成功后会在`yumemi`目录下自动生成`data`目录存放缓存  
> 当然，如果你有`js`的相关知识，随时都可以编写自己的插件，详情可在 [插件开发](develop/) 一栏查看

### 启动程序

```
npm start
```

!> 群消息默认 **不会监听处理** ，在群内发送`开启群服务`即可打开，部分模块 **默认关闭** ，可发送`list`查看服务列表自行启用  

![version](../public/images/demo/version.png)

- 开启模块
  + `开启 | 关闭` + `模块名`
- 正则公式
  + `^(开启|启用|打开|关闭|禁用)[\s]?[a-z]+(18)?$`

![enable](../public/images/demo/enable.png)