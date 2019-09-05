# <font  color='#03BC49' >PART3.数据层</font>

<p style='color:#03BC49;font-size:20px;font-weight:bold'>CH1.Redux  <a style="color:#1293D8;font-size:12px;" href="https://redux.js.org/">链接</a></p>

* Redux是JavaScript应用程序的可预测状态容器

<p style='color:#03BC49;font-size:20px;font-weight:bold'>CH2.GraphQL  <a style="color:#1293D8;font-size:12px;" href="https://graphql.org/">链接</a></p>

* GraphQL是API的查询语言
* 在单个请求中获取许多资源
* 应用：自2012年以来，Facebook的移动应用程序一直由GraphQL提供支持。一个GraphQL规范于2015年开源，现在可在许多环境中使用，并由各种规模的团队使用

<p style='color:#03BC49;font-size:20px;font-weight:bold'>CH3.Apollo <a style="color:#1293D8;font-size:12px;" href="https://www.typescriptlang.org/index.html">链接</a></p>

* Apollo 是一个功能完善、生产环境友好、集成了缓存机制的 GraphQL 客户端。它可以与几乎所有的前端框架和支持 GraphQL 的服务器搭配使用。

<p style='color:#03BC49;font-size:20px;font-weight:bold'>CH4.MobX (v5)<a style="color:#1293D8;font-size:12px;" href="https://cn.mobx.js.org/">链接</a></p>

* 通过透明的函数响应式编程(transparently applying functional reactive programming - TFRP)使得状态管理变得简单和可扩展

<p style='color:#03BC49;font-size:20px;font-weight:bold'>CH5.Relay/Relay Modern <a style="color:#1293D8;font-size:12px;" href="https://github.com/facebook/relay">链接</a></p>

* Relay 是一个用于构建数据驱动型的React应用的JavaScript框架.
* 声明性：不再使用命令式API与数据存储进行通信。只需使用GraphQL声明您的数据要求，然后让Relay确定如何以及何时获取数据。
* 主机托管：查询位于依赖于它们的视图旁边，因此您可以轻松推理您的应用。中继将查询聚合到有效的网络请求中，以仅获取所需内容。
* 突变： Relay允许您使用GraphQL突变来改变客户端和服务器上的数据，并提供自动数据一致性，乐观更新和错误处理。

<p style='color:#03BC49;font-size:20px;font-weight:bold'>结论 </p>

* 曾几何时，一切都是那么的简单和纯粹：数据就存放在数据库中，服务器先在这里获取数据，然后把数据放到模板里，最后再发送给客户端。

* 时代不同了，一切都不再那么简单。现如今，客户端都需要自己直接获取数据。由于目前，主流方案是把模板和组件放在客户端；相应地，数据组装到模板这一过程也从发生在服务端过渡到了客户端。

* 毫无疑问，Redux 是目前最流行的相关工具。今年，它凭借 82% 的满意度巩固了自己在这类工具中的统治地位。

* 不过，这一切可能很快会被 GraphQL 改变。近两年，GraphQL 用户从 5% 增长至 20%，而且大部分选择了 Apollo 这个平台。最新版本的 Apollo 也将 Redux 设为可选项。因此，明年的结果就算变化很大也不稀奇。
