### 简单实例

!> 这里是正在施工的 **插件开发** 文档，不定时更新  
开发文档仅适用于 0.4.x 及以下版本，项目近期重构，仅供参考

- 在编写模块前，你需要先在`plugins`目录下，创建一个文件夹来存放模块文件  
- 例如`plugins/hello`，`hello`为你的插件名，命名规则为`^[a-z]$`，无其它严格限制 ~~你自己看得懂就行~~  
- 创建成功后在当前目录下创建`index.js`，此名为固定命名，不可更改，否则需要 **手动** 加载

以下为简单示例：

```javascript
// plugins/hello/index.js
module.exports = ctx => {
  // 获取群号
  const { group_id } = ctx;

  // 发送群聊消息
  bot.sendGroupMsg(group_id, 'hello world');
}
```

然后在`config/cmd.yml`文件下添加以下参数：

```yaml
# 此处的 key 值必须与插件文件夹名保持一致
hello: ^你好$
```

> 现在，**重启** 项目后，在群内发送`你好`，即可收到`hello world`的回复

### 插件优化

> 在刚刚的简单示例中，我们介绍了如何编写属于自己的第一个插件，但是，这其中又存在着很多的问题

- 首先，我们要了解，什么是插件（模块）？
  + 就像抽屉一样，我们想要存放什么东西，需要根据不同的物品摆放到不同的位置，bot 也是如此
  + 例如我需要实现一个涩图功能，又想做一个十连，这两个模块肯定不能同时写在一起
  + 这样既不方便后续代码维护，也不利于业务逻辑的操作
  + 所以，我们需要将自己想实现的功能，创建不同的文件夹将其一一拆分

- 回过头来，刚刚编写的插件有什么问题？
  + 我仅仅是要实现一个 say hello 的功能，就单独创建了一个 hello 模块
  + 那是不是以后，我突然想写一个 say goodbay ，又需要创建新的文件夹？

![这河里妈](../public/images/emoji/这河里妈.jpg)

```
plugins
├─ bay               bay   模块
│  └─ index.js
└─ hello
   └─ index.js       hello 模块
```

![这真布河里](../public/images/emoji/这真布河里.png)

> 因此，我们需要将 **拥有相同功能** 的插件二次拆分，这样可以有效的减少代码冗余

我们将 hello 文件夹改名为 say

```javascript
// plugins/say/index.js
module.exports = ctx => {
  // 获取群号
  const { group_id， serve } = ctx;

  // 发送消息
  switch (serve) {
  case 'bay':
    bot.sendGroupMsg(group_id, 'bay bay');
    break;

  case 'hello':
    bot.sendGroupMsg(group_id, 'hello world');
    break;
  }
}
```

```yaml
# 此处的 key 值必须与插件文件夹名保持一致
say:
  # 此处的 key 值对应 ctx 的 serve 属性
  bay: ^再见$
  hello: ^你好$
```

> 现在，**重启** 项目后，在群内发送`hello`，可以收到`hello world`，发送`bay`，又能收到`bay bay`

### 流程介绍

> 你已经熟练掌握了应该如何创建一个属于自己的插件  
接下来让我们了解一下，从发送 hello 指令，到接收 hello world 这段时间内到底发生了什么

- 发生了什么？
  + 在项目启动时，Yumemi 会开始加载插件
  + 遍历整个`plugins`目录，并识别该目录下的`index.js`文件
  + 待全部模块加载完毕后，便开始监听群消息
  + 若收到群消息，则会匹配`config/cmd.yml`下的正则公式
  + 匹配成功，就会将该正则的`key`值识别为所要执行模块名

下面这段代码是在编写时 **必不可少** 的格式，花括号内外都可随意编写你的代码

```javascript
// ... 此处的代码只会在项目启动时执行一次
module.exports = ctx => {
  // ... 花括号内的代码仅在每次调用时执行
}
```

> 当然，若你编写的插件不需要被动触发，可以不用接收`ctx`

以下为买药助手简单示例：

```javascript
// plugins/buy/index.js
// 每6小时执行定时器
tools.scheduleJob('0 0 0/6 * * ?', () => {
  // 获取群聊信息
  const group = tools.getYAML('group');

  // 遍历群聊获取群号
  for (const group_id in group) {
    bot.sendGroupMsg(group_id, `(/▽＼) 该去买药啦~`);
  }
});
```

我们可以看到，因为不需要主动触发，所以不用编写 module.exports 来暴露模块，也不用配置 cmd.yml

### 参数说明

> yumemi 封装了一套 tools 类，在刚刚的示例中我们有使用到，下面将为你介绍相关 api

#### tools

以下为 tools 中较常用的参数

```javascript
// node-schedule 定时器
// https://github.com/node-schedule/node-schedule
tools.scheduleJob(cron, callback)

// 获取 YAML 配置文件信息
// 不传入 path 默认 yumemi/config 目录
tools.getYAML(fileName[, path])

// 写入 YAML 配置文件数据
// 不传入 path 默认 yumemi/config 目录
tools.setYAML(fileName, data[, path])

// 发送 https 请求
// options 默认 { timeout: 5000 }
async tools.getHttps(url[, options])

// 执行 sql ，基于 node-sqlite3 封装
// https://github.com/mapbox/node-sqlite3
new tools.SQL(sql[, param])
```

SQL 简单示例：

```javascript
const sql = `SELECT * FROM fight_view WHERE group_id = ${group_id} AND user_id = ${user_id}`;
// or
const sql = "SELECT * FROM fight_view WHERE group_id = $group_id AND user_id = $user_id";
const param = {
  $group_id: group_id,
  $user_id: user_id,
};
const sql = new SQL(sql, param);

sql.all()
  .then(rows => {
    console.log(rows);
  })
  .catch(err => {
    throw err;
  });
```

#### ctx

以下为 ctx 中较常用的参数

```javascript
group_id      // 群号
group_name    // 群名
user_id       // Q 号
nickname      // 昵称，若群名片未设置则返回网名
raw_message   // 消息
```


#### bot

`bot`是 QQ 的实例对象，以下为几个较常用的 API

```
async sendPrivateMsg(user_id, message)   // 私发
async sendGroupMsg(group_id, message)    // 群发
async setGroupKick(group_id, user_id)    // 踢人
async setGroupBan(group_id, user_id)     // 禁言
```

查看更多 API 可访问：[oicq 相关文档](https://github.com/takayama-lily/oicq/blob/master/docs/api.md)

### 静态模块

> 在查看源码时，你可能已经发现了，在 plugins 目录下有部分是使用下划线开头的插件

- 前面我们说过，插件的命名规范无严格限制，满足`^[a-z]$`即可
  + 需要注意的是，有部分插件名字为`^__[a-z]*$`格式
  + 只要是以 __ 开头的为静态模块 ~~姑且我自己是这么叫的~~
  + 和普通插件的区别在于，该类型模块默认开启，且 **无法关闭**