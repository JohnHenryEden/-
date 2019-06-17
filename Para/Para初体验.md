# Para后端框架研究 ----为忙碌程序员设计的后端框架 之一 初体验

## **什么是Para？**

Para是一个用于对象持久化和获取的简单并且模块化的后端框架。它能通过处理后端操作来帮助你更快构建原型和应用。它可以是你的基于JVM的应用的一部分或者可以是一个单独部署的API服务器，供多个应用和客户端使用。

Para在保加利亚语中意为“蒸汽”，正如蒸汽一样它被用来驱动东西，你可以用Para来驱动你的移动或Web端应用后端。

[Para与其它后端的比较](https://erudika.com/blog/2015/10/21/backend-frameworks-usergrid-loopback-para-baasbox-deployd-telepat/)

**功能**

- 使用亚马逊的V4签名算法加密的RESTful JSON API
- 数据库无关，可以使用流行的可扩展数据源（Cassandra，MongoDB等）
- 全文搜索（Elasticsearch）
- 分布式和本地对象缓存
- 多租赁技术 - 每个应用都有自己的表，索引和缓存
- IoT支持以及与AWS和Azure的结合
- 基于Spring Security的灵活的安全机制（LDAP，SAML，社交网络登陆，CSRF防护等）
- 使用JWT的无状态客户端验证
- 简单而有效的客户资源许可控制
- 基于JSR-303和Hibernate Validator的健壮的约束验证机制
- 持久层，索引和缓存中的对每个对象的控制
- 支持乐观锁和事务控制
- 先进的序列化/反序列化能力
- 监控和诊断应用的各种工具（Dropwizard）
- 使用Google Guice驱动的模块化设计和对插件的支持
- 用于翻译语言和处理汇率的国际化工具
- 带有嵌入的Jetty的单独可执行JAR
- [Para Web Console](https://console.paraio.org/hello.html) - 管理员图形用户界面

## **快速开始**

在Para包的目录下创建`application.conf`文件，以下是一个示例配置：

```ini
# the name of the root app
para.app_name = "Para"
# or set it to 'production'
para.env = "embedded"
# if true, users can be created without verifying their emails
para.security.allow_unverified_emails = false
# if hosting multiple apps on Para, set this to false
para.clients_can_access_root_app = true
# if false caching is disabled
para.cache_enabled = true
# root app secret, used for token generation, should be a random string
para.app_secret_key = "b8db69a24a43f2ce134909f164a45263"
# enable API request signature verification
para.security.api_security = true
# the node number from 1 to 1024, used for distributed ID generation
para.worker_id = 1
```
创建完后：

1. [下载最新的可执行JAR](https://github.com/Erudika/para/releases)
2. 使用命令`java -jar -Dconfig.file=./application.conf para-*.jar`执行
3. 为根应用获取accessKey和secretKey，启动JAR后调用`curl localhost:8080/v1/_setup`
4. 安装`para-cli`工具来更简单地访问（可选，`npm install -g para-cli`）
5. 创建一个新的“子”应用（可选）
```bash
# run setup and set endpoint to either 'http://localhost:8080' or 'https://paraio.com'
$ para-cli setup
$ para-cli new-app "myapp" --name "My App"
```
6. 打开[Para Web Console](https://console.paraio.org/hello.html)即可看到新建的应用。
   
与Para交互的最快方法就是使用`para-cli`。另外地，可以用`Para.newApp()`或者向API做一个经过验证的请求`GET /v1/_setup/{app_name}`

```bash
$ npm install -g para-cli
$ para-cli setup
$ para-cli ping
$ echo "{\"type\":\"todo\", \"name\": \"buy milk\"}" > todo.json
$ para-cli create todo.json --id todo1 --encodeId false
$ para-cli read --id todo1
$ para-cli search "type:todo"
```

注意:现在似乎para-cli有问题，运行`para-cli setup`会报错，已经提交issue向开发者反馈

绕过出错的方法：

1. 打开`C:\Users\{user}\AppData\Roaming\npm\node_modules\para-cli\node_modules\conf\index.js`
2. 修改第58行为`options.projectName = 'para-cli';`
3. 执行`para-cli setup`