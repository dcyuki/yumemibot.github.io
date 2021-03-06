### 指令管理 <!-- {docsify-ignore} -->

前面，我们在 [项目部署](install/) 一栏有提到如何启用一个服务模块  
但是要注意，并不是所有人都能够直接对插件进行管理

### 等级制度
在 yumemi 看来，任何发送消息都是成员都是有等级的

- level 0 群成员（随活跃度提升）
- level 1 群成员（随活跃度提升）
- level 2 群成员（随活跃度提升）
- level 3 管　理
- level 4 群　主
- level 5 主　人（你填写的 masters）
- level 6 维护组

只有达到 **level 3** 及以上才能修改相关配置，锁定插件需要达到 **leve 4**

> 接下来，我们将在这里为你一一介绍插件的相关指令与操作

### 关闭启用
- 举个栗子
  + 输入`>enable xxx`和`>disable xxx`来管理插件
  + 发送`list`可以查看所有插件状态，`△`和`○`分别对应禁用与启用
- 正则公式
  + `^>(enable|disable)[\s][a-z]+$`
- 补充说明
  + 如果你不习惯使用这种类似命令行的指令，可以使用`打开 xxx`或`关闭 xxx`的语法糖来替代
  + 关键字有`(开启|启用|打开|关闭|禁用)`

### 我全都要
- 举个栗子
  + 如果你嫌一个个打开太麻烦，可以输入`开启群服务`来启用所有插件
  + 你也可以逆向操作来禁用所有插件
- 正则公式
  + `(开启|启用|打开|关闭|禁用)群服务`
- 补充说明
  + 注意，部分插件需要填写 api 才能正常使用，你没填开了插件也用不了

### 插件锁定
- 举个栗子
  + 输入`锁定 xxx`和`解锁 xxx`来限制其他人对插件进行操作
- 正则公式
  + `^(锁定|解锁)\s?[a-z]+$`
- 补充说明
  + 若插件锁定为关闭状态，发送`开启群服务`也不会启用该插件
  + 可是，如果插件锁定启用状态，发送`关闭群服务`将不会受限，因为关闭群内所有监听的权重是 **最高的**

### 参数修改
- 举个栗子
  + 每个插件都有其设置选项，你可以发送`setting`查看
  + 修改参数的指令为`>update <plugin> <setting> <param>`
  + 例如修改发送的色图不为闪图`>update setu flash false`
- 正则公式
  + `^>update[\s][a-z]+[\s][a-z0-9]+[\s][a-z0-9]+$`
- 补充说明
  + 除非你知道自己在做什么，不要修改一些奇怪的参数，不然可能会导致插件无法正常运行

```
# 目前已有的语法糖

打开 r18     # 修改 setu 参数
关闭 flash   # 修改 setu 参数
关闭 pcr_bl  # 修改 bilibili 参数
关闭 pcr_bl  # 修改 bilibili 参数
打开群服务　　# 修改插件状态

# 除以上参数，其它任何内容都将作为插件名识别
```

除了插件的相关管理，还有关于机器人的指令

!> 注意，以下指令私聊也是可以触发的，别傻fufu的跑群里去输自己的账号密码

### 状态查询
- 举个栗子
  + 发送`>bot`可以查看当前所有机器人状态
- 正则公式
  + `^>bot(\s(add|off|on|del)\s\d+)?$`
- 补充说明
  + yumemi 支持多账号管理，并不仅限于登录一个账号
  
### 账号添加
- 举个栗子
  + 发送`>bot add <qq>`可以添加一个新的账号
- 补充说明
  + 根据提示输入密码及对应验证即可成功登录
  + 只有首次登录才需要输入密码，后续启动项目将会自动登录
  + 无须担心，保存在本地的密码文件都经过加密

### 状态切换
- 举个栗子
  + 发送`>bot off <qq>`或`>bot on <qq>`来控制机器人上下线

### 账号删除
- 举个栗子
  + 发送`>bot del <qq>`即可删除账号相关信息
- 补充说明
  + 不建议频繁删除账号，每一次删除重新登录都将会生成新的设备文件

### 关闭重启
- 举个栗子
  + 发送`>shutdown`或`>restart`来控制 **整个项目** 的关闭和重启
- 补充说明
  + 若是在控制台启用的服务，项目重启后原控制台窗口将不会再指向线程服务（如果有大佬能解决可以来提 pr）

### 运行状态
- 举个栗子
  + 发送`state`可以查看当前服务的运行情况

### 退出群聊
- 举个栗子
  + 发送`>quit`后机器人将自动退出群聊