### 简单实例

!> 这里是正在施工的 **插件开发** 文档，不定时更新

- 在编写模块前，你需要先在`plugins`目录下，创建一个文件夹来存放模块文件  
- 例如`plugins/hello`，`hello`为你的插件名，命名无规范限制 ~~你自己看得懂就行~~  
- 创建成功后在当前目录下创建`index.js`，此名为固定命名，不可更改，否则需要 **手动** 加载引入

以下为简单示例：

```javascript
// plugins/hello/index.js
module.exports = (messageData, setting) => {
   api.sendGroupMsg(messageData.group_id, '你好世界')
}
```

然后在`config/command.yml`文件下添加以下参数：

```yaml
# 此处的 key 值需要与插件文件夹名保持一致
hello:
 - ^你好$
```

> 现在，**重启** 项目后，在群内发送`你好`，即可收到`你好世界`的回复  
> 该模块会在 bot 启动时自动加载，无需修改添加其他参数

### 流程介绍

#### 发生了什么？ <!-- {docsify-ignore} -->

- 在项目启动时，Yumemi 会开始加载插件
  + 遍历整个`plugins`目录，并识别该目录下的`index.js`文件  
- 待全部模块加载完毕后，便开始监听群消息
  + 若收到群消息，则会匹配`config/command.yml`下的正则公式
  + 匹配成功，就会将该正则的`key`值识别为所要执行模块名

#### 参数说明

!> `messageData`、`setting`，这两个参数正是在插件运行时传入的，也是 **必须接收** 的  
所以下面这段代码是在编写时 **必须遵守** 的格式，花括号中可随意编写你的插件逻辑

```javascript
module.exports = (messageData, setting) => {
  // 此处可随意编写你的插件逻辑
}
```

- `api`是 QQ 的实例对象，以下为几个较常用的 API
  + 私发 sendPrivateMsg(user_id, message)
  + 群发 sendGroupMsg(group_id, message)
  + 踢人 setGroupKick(group_id, user_id)
  + 禁言 setGroupBan(group_id, user_id)

查看更多 API 可访问：https://github.com/takayama-lily/oicq/blob/master/docs/api.md

- `messageData`为发送消息的实例对象，以下为几个较常用的参数
  + 群号 group_id
  + 群名 group_name
  + sender
    + Q 号 user_id
    + 昵称 nickname
  + 消息 raw_message