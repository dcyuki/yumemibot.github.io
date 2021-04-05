### 简单实例

!> 这里是正在施工的 **插件开发** 文档，不定时更新

- 在编写模块前，你需要先在`plugins`目录下，创建一个文件夹来存放模块文件  
- 例如`plugins/hello`，`hello`为你的插件名，命名为 **a-z** 无其它严格限制 ~~你自己看得懂就行~~  
- 创建成功后在当前目录下创建`index.js`，此名为固定命名，不可更改，否则需要 **手动** 加载

以下为简单示例：

```javascript
// plugins/hello/index.js
module.exports = ctx => {
  const { group_id } = ctx;

  bot.sendGroupMsg(group_id, 'hello world')
}
```

然后在`config/cmd.yml`文件下添加以下参数：

```yaml
# 此处的 key 值必须与插件文件夹名保持一致
hello: ^hello$
```

> 现在，**重启** 项目后，在群内发送`hello`，即可收到`hello world`的回复  
> 该模块会在 bot 启动时自动加载，无需修改添加其他参数

### 流程介绍

> 在开始之前，先让我们来了解一下从发送 hello 到接收 hello world 这段时间内到底发生了什么

#### 发生了什么？ <!-- {docsify-ignore} -->

- 在项目启动时，Yumemi 会开始加载插件
  + 遍历整个`plugins`目录，并识别该目录下的`index.js`文件，并加载
- 待全部模块加载完毕后，便开始监听群消息
  + 若收到群消息，则会匹配`config/cmd.yml`下的正则公式
  + 匹配成功，就会将该正则的`key`值识别为所要执行模块名

#### 有什么问题？ <!-- {docsify-ignore} -->

在刚刚的简单示例中，我们介绍了如何编写属于自己的第一个插件，但是这其中又存在着很多的问题  
首先我们要知道，什么是插件（模块）？  
就像抽屉一样，我们想要存放什么东西，需要根据不同的物品摆放到不同的位置，bot 也是如此  
例如我需要实现一个涩图功能，又想做一个十连，这两个模块肯定不能同时写在一起  
因为这样既不方便后续代码维护，也不利于业务逻辑的管理  
所以，我们需要将自己想实现的功能，创建不同的文件夹将其一一拆分  
回过头来，刚刚编写的插件有什么问题？  
我仅仅是要实现一个 say hello 的功能，就单独创建了一个 hello 模块  
那是不是以后，我突然想写一个 say goodbay ，又需要创建新的文件夹？  
这样显然是不合理的  
因此，我们需要将 **拥有相同功能** 的插件二次拆分，这样可以有效的减少代码冗余  
例如以下例子：

```javascript
// 我们将 hello 模块改名为 say
// plugins/say/index.js
module.exports = ctx => {
  const { group_id， serve } = ctx;

  switch (serve) {
  case 'hello':
    bot.sendGroupMsg(group_id, 'hello world');
    break;
  
  case 'bay':
    bot.sendGroupMsg(group_id, 'goodbay');
    break;
  }
}
```

```yaml
# 此处的 key 值必须与插件文件夹名保持一致
say:
  # 此处的 key 值对应 ctx 的 serve 属性
  hello: ^hello$
  bay: ^bay$
```

> 现在，**重启** 项目后，在群内发送`hello`，可收到`hello world`，发送`bay`，又能收到`goodbay`的回复

#### 参数说明

- `bot`是 QQ 的实例对象，以下为几个较常用的 API
  + 私发 sendPrivateMsg(user_id, message)
  + 群发 sendGroupMsg(group_id, message)
  + 踢人 setGroupKick(group_id, user_id)
  + 禁言 setGroupBan(group_id, user_id)

查看更多 API 可访问：[oicq 相关文档](https://github.com/takayama-lily/oicq/blob/master/docs/api.md)

- `ctx`为发送消息的实例对象，以下为几个较常用的参数
  + 群号 group_id
  + 群名 group_name
  + Q 号 user_id
  + 网名 nickname
  + 消息 raw_message

!> `ctx`，正是在插件执行时传入的，**并非必须接收**  
下面这段代码是在编写时 **必不可少** 的格式，花括号内外都可随意编写你的代码

```javascript
// ... 此处的代码只会在项目启动时执行一次
module.exports = ctx => {
  // ... 花括号内的代码仅在每次调用时执行
}
```

!> 当然，若你编写的插件不需要接收群内发送的消息，例如每日的买药提示，仅仅是执行固定操作，可以不传入 ctx 对象

```javascript
module.exports = () => {
  console.log(`卖个萌 (*/ω＼*)`);
}
```

这个时候你可能就要问了，我没有群号该如何发送消息呢？  
别着急，yumemi 封装了一套 tools 类方法，下面将为你介绍相关 api