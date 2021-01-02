### 插件开发

!>在编写模块前，你需要先在`plugins`目录下，创建一个文件夹来存放模块文件  
由于 yumemi 还在初期开发阶段，项目结构随时都有可能大改，暂不详细介绍，此处仅给出简单示例

例如`plugins/hello`，`hello`为你的插件名，命名无规范限制 ~你自己看得懂就行~  
创建成功后在当前目录下创建`index.js`，此名为固定命名，不可更改，否则需要手动加载引入

以下为简单示例：

```javascript
// plugins/hello/index.js
module.exports = (bot, messageData, setting) => {
	
	if (messageData.raw_message === '你好') {
        bot.sendGroupMsg(messageData.group_id, '你好世界')
    }
}
```

> 现在，在群内发送`你好`，即可收到`你好世界`的回复  
> 该模块会在 bot 启动时自动加载，无需修改添加其他参数
