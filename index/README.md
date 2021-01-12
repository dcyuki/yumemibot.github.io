### 项目介绍

> 本项目是一个基于 [OneBot](https://github.com/howmanybots/onebot) 协议，使用 [JavaScript](https://www.javascript.com/) 语言开发的 [QQ](https://im.qq.com/) 机器人  
> 在这之前我从未接触过 [Nodejs](https://nodejs.org) ，故此项目所写的东西可能会让大家血压升高  
> 代码全由个人编写维护，源码写的就跟 **Shit** 一样，处于`能 跑 就 行`的状态，日后会不断优化

- Yumemi 出自于日语 **ユメミ**，一时兴起起得名字，毕竟我经常做白日梦嘛（
- 暂时没有中文名字，**夢見**、**結女実**、**優芽美**．．．汉字有好多好多啦，想叫啥都行
- 图片来自 [Pixiv](https://www.pixiv.net)，无意间在首页看到的，觉得挺好看的就拿来当做 **Yumemi** 的头像了，id`77959674`，侵删

![电波传达不到哦...](../public/images/avatar/yumemi.jpg "id:77959674")

### 开源须知

!> 使用本项目请遵循 [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.txt) 开源许可协议

#### 什么是 GNU General Public License v3.0 ?（以下简称 GPL） <!-- {docsify-ignore} -->

- 必须开源
  + 一旦使用了这个协议，如果他人想要进行分发、修改，那么他们修改后的源代码也必须开源。这是开源的核心保障，如果没有这条规定，就没有人愿意持续公开自己的源码了
- 保留协议和版权
  + 保留对协议和版权的叙述
- 不允许更换协议
  + 如果有人修改了一些源码，觉得自己改得还挺多的，想要换一个 MIT 或者什么协议，这是不允许的。一旦最原始的源码使用了 GPL ，其衍生的所有代码都必须使用 GPL 。这也是开源保障之一
- 声明变更
  + 对于代码的变更需要有文档进行说明改了哪些地方

ps： Linux 就是使用的 GPL 协议，如果在使用中遇到什么问题或者有意见和建议都可以 [进群反馈](https://jq.qq.com/?_wv=1027&k=3hcWCnhq)

- 遇到 bug
  + 提 issues ×
  + 水群 √

### 使用框架

> 由衷感谢以下开源框架，感谢 dalao 们的无私奉献

- [oicq](https://github.com/takayama-lily/oicq)
  + 使用至今，以后 ~~大概率~~ 不会更换其他框架了
  + 得益于此项目才让 Yumemi 实现真正的简单轻便、开箱即用
- [mirai-ts](https://github.com/YunYouJun/mirai-ts)  
  + 由于不可描述的原因，个人使用 koishi 开发不太理想，故更换框架
- [koishi](https://github.com/koishijs/koishi)
  + 因为不会 Python ，开发插件比较苦手，所以选择移植到 JavaScript 平台框架
- [nonebot](https://github.com/nonebot/nonebot)
  + 初版 bot 所使用的框架，当时项目名还叫 Kokkoro 来着，太容易撞名所以才改成 Yumemi （笑）