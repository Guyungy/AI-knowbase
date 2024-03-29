# 使用相关

使用 ChatGPT 时将网页翻译关闭，避免报错。 经常需要人机确认，或者网络响应慢 这是正常的。

Not available，OpenAI‘s service are not available in your country 节点有问题，指向欧洲/日本

### 无法访问

原因主要有两个：网络问题和 OpenAI 限流 目前 OpenAI 的限制是全球性的，付费计划的出现导致现在所有的服务资源都要向付费版倾斜，免费版有点苟延残喘的味道。在网络封锁，和网站限流的双重打击下，往往会造成页面各种崩溃报错。主要有以下常见问题：

1. 无访问权限，服务满负荷
2. 无法登录，提示国家地区不支持
3. 聊天返回红色字体信息
4. 出现各种错误页面
5. 发送的消息被清除
6. 请一小时后再尝试

首先需要解决的问题是网络，因为 OpenAI 对地区和 IP 进行了安全校验，所以即使挂了代理，也可能属于被封范围。

造成的结果就是别人可以访问，我不可以；有时可以访问，有时不可以。

自身网络，代理设置，节点所处的地区都会影响网络的连接。如果网络正常，还要面临的是 OpenAI 的限流（服务满载，请一小时后重试等等）。

所以遇到无法访问的问题，不要怀疑，也不要焦虑，因为倒霉的远远不止你一个。有很多人正在面临和你一样的情况，不光国人会遇到，国外一大批人也在遇到一样的情况（我混迹在 OpenAI 的 Discord 频道里，每天可以看到各种 BUG 反馈，基本一大半问题都是在说网络出错）。 所有的访问出错，服务拒绝，归根结底就是网络和限流。

如果要问有什么办法可以解决，也只能通过尝试切换节点的地区来缓解（多人共享的节点被屏蔽可能性更大）。

它并不总是有效的，所以这种情况属于无解状态。付费版会稳定很多，但它不在讨论范围，需要付费的请自行了解。 聊天时，当你发送一句中文，但是 AI 却回了你一句英文。

这时你可以通过发送：请讲中文，来让 ChatGPT 使用中文和你进行对话。

聊天的内容如果过长，ChatGPT 经常会将消息截断，这时你可以通过发送：继续，来让 ChatGPT 继续发送剩余内容。

ChatGPT 是通过条件约束来工作的，如果你想要得到更专业的回答结果，你需要对它进行训练，通过约束条件来不断地对它进行强化。
