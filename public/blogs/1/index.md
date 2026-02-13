# 为什么还要建一个Bitwarden服务器？
虽然像[Vaultwarden](https://github.com/dani-garcia/vaultwarden)这样的项目提供了优秀的自托管解决方案，但它们仍然需要您管理服务器或 VPS。
这可能会很麻烦，而且如果您忘记支付服务器费用，您可能会丢失密码访问权限。

Warden旨在利用Cloudflare Workers生态系统解决这个问题。
通过将Warden部署到Cloudflare Worker并使用Cloudflare D1进行存储，您可以拥有一个完全免费、无服务器且维护成本低的Bitwarden服务器。


## 准备工作
- github账号
- Cloudflare账号
- 托管在CF的域名（国内无法访问CF Workers提供的`.workers.dev`域名）
# 开始
1. 前往[warden-worke](https://github.com/qaz741wsd856/warden-worker)项目
2. 复刻项目到你的账号
3. 密钥获取

| 可以打开记事本拷贝此列           | 备注        |
| --------------------- | --------- |
| CLOUDFLARE_API_TOKEN  | CF账号token |
| CLOUDFLARE_ACCOUNT_ID | CF账号ID    |
| D1_DATABASE_ID        | D1数据库ID   |
## CF账号Token获取方式
打开[Cloudflare](https://dash.cloudflare.com/)

进入右上角**配置文件**

![](/blogs/1/f4c85049353dfbfe.png)

进入左边列表的**API令牌**

![](/blogs/1/c65c4f1b2225edcc.png)

点击页面中的**创建令牌**

![](/blogs/1/2dcac2dc21894c24.png)

选择使用**编辑 Cloudflare Workers**模板

![](/blogs/1/ff51170e52392d10.png)

单独添加**D1**编辑权限

账户资源选择你的账户
区域资源选择你后面要绑定的域名
![](/blogs/1/41e9325a2795954e.png)
划到下面点击蓝色按钮**继续以显示摘要**
![](/blogs/1/2f77bc605370dc11.png)
确认没问题点击**创建令牌**
![](/blogs/1/1ef13a5bbd78cf93.png)
点击复制Token备用

## CF账号ID获取方式
在CF页面点击左上角的小黄云后，在主页链接的/home前面
![](/blogs/1/fe0d049dadb48341.png)
## D1数据库ID获取方式

展开左边列表**构建**下的**存储和数据库**，进入**D1 DQL数据库**
![](/blogs/1/e89bf4a46a94444a.png)
创建数据库
![](/blogs/1/b75ea2e6e066df12.png)
名称填`vault1`后创建
![](/blogs/1/6e2463a3cfb51476.png)
点击名称后面的复制图标备用。

## GithubAction
4. github前往你复刻的项目，并添加密钥
![](/blogs/1/25e27a42544a98b3.png)
点击**New repository secret**创建密钥。
根据前面的表格分别创建3个密钥
![](/blogs/1/2e91eb5d3a52e6a0.png)
 5. 运行Actions
 打开项目的Actions选项卡。

点击绿色按钮**I understand my workflows, go ahead and enable them**
  6. 运行**Build**等待完成
![](/blogs/1/bff9a452ee38c5e6.png)
## CloudFlareWorker
 7. 运行完成后打开workers项目
![](/blogs/1/ca2f05683f3a4b54.png)
 8. 添加变量和机密
![](/blogs/1/a42756e827782477.png)
 

| 名称                 | 用途        | 备注           |
| ------------------ | --------- | ------------ |
| ALLOWED_EMAILS     | 允许注册的邮箱地址 | *@qq.com     |
| JWT_SECRET         | 访问令牌      | 32到256位随机字符串 |
| JWT_REFRESH_SECRET | 刷新令牌      | 同上           |
![](/blogs/1/98a38d56392dc625.png)

这样添加自定义域名后注册就可以访问使用了。

# 常见问题

## 无法注册或登陆？

访问链接后面的`/#/login`改为`/#/signup`

注册后就可以登陆了