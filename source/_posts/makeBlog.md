---
title: 博客搭建 - Hexo+Git+腾讯云
date: 2019-10-14 01:17:33
description: 为梳理所学知识，决定使用Hexo+Git+腾讯云搭建博客以作记录，另外使用webhook实现自动化构建。该文章主要阐述该博客的搭建过程，希望给大家带来帮助。
tags:
---
# 前言
为梳理所学知识，决定使用Hexo+Git+腾讯云搭建博客以作记录，另外使用webhook实现自动化构建。该文章主要阐述该博客的搭建过程，希望给大家带来帮助。

# 搭建流程

## 购买腾讯云

1. 注册腾讯云账号

2. 购买云服务器。
carlson用的是学生优惠套餐，费用为120元一年。如果不能享用该套餐，建议找找优惠活动页，相同配置价格也不会贵很多。购买后可通过ssh登陆root用户，密码可查看腾讯云消息中心的邮件。默认系统为CentOS7.6，购买后可重装至7.5。
[腾讯云学生版链接](https://cloud.tencent.com/act/campus)
![云服务器配置](/img/云服务器配置.png)

3. 购买域名
域名非必需，可不购买。目前最低费用为20元一年，注册后均需备案，填写好个人资料后隔天会有电话来访，如实回答即可，整个流程大概1个月。购买后找到域名管理页面，点击解析并添加服务器的外网IP。
[腾讯云域名注册链接](https://dnspod.cloud.tencent.com)

4. 配置安全策略
在个人云服务器菜单页面，创建一个安全组。入站为外网访问服务器，可暂时放通22（用于ssh连接）、80（博客端口）；出站为服务器访问外网，可放通所有。最后，关联到服务器实例。
![服务器安全组配置](/img/服务器安全组配置.png)

5. 关闭防火墙（腾讯云安全策略代替）
检查防火墙状态`systemctl status firewalld`，关闭`systemctl  stop firewalld`，服务器环境准备完成。

## 搭建hexo

1. 安装nodejs环境

	1.1. 登陆服务器创建目录，下载安装包并查看版本。该文章所有命令仅作参考，目录一般可自定义。
```
mkdir -p /carlson/component/nodejs/	
wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-x64.tar.xz 	
tar -xf node-v12.13.0-linux-x64.tar.xz 
cd node-v12.13.0-linux-x64/	
./bin/node -v 
```

	1.2. 修改全局变量`vi ~/.bash_profile`使npm、node相关命令可在任意目录下执行，即PATH=$PATH:$HOME/bin:/carlson/component/nodejs/node-v12.13.0-linux-x64/bin，保存后重新加载变量`source ~/.bash_profile`。

	
2. 发布hexo应用
	
	2.1. 安装hexo依赖，`npm install -g hexo-cli`。

	2.2. 创建目录，初始化工程并以80端口启动
```
mkdir  /carlson/service/blog/
hexo init
hexo server -p 80
```

3. 通过外网IP或域名访问，成功访问则代表博客环境准备完成

## 安装git

1. 安装git环境

	1.1. 安装必要依赖`yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel`。

	1.2. 创建目录，下载安装包并安装。
```
mkdir -p /carlson/component/git/	
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.23.0.tar.gz
tar -zxf git-2.23.0.tar.gz 
cd git-2.23.0/
make prefix=/usr/local all
make prefix=/usr/local install
```

2. 推送至github

	2.1. 注册github账号[https://github.com/](https://github.com/)，并创建仓库
	![创建github仓库](/img/创建github仓库.png)

	2.2. 在服务器中创建ssh公钥私钥`ssh-keygen -t rsa -b 4096 -C "任意邮箱"`，提示Enter passphrase时输入相同密码或默认回车两次。

	2.3. 打开公钥文件`cat /root/.ssh/id_rsa.pub`，复制内容至github
	![github添加SSHKey](/img/github添加SSHKey.png)

	2.4. 进入服务器博客根目录，推送至仓库进行绑定，若有密码则需输入，推送至github完成
```
git init
git commit -m "first commit"
git remote add origin https://github.com/13631372596/Blog.git
git push -u origin master
```

## 搭建github-webhook

1. 安装github-webhook

	1.1. 安装依赖`npm install -g github-webhook-handler`。[github源码地址](https://github.com/rvagg/github-webhook-handler)

	1.2. 在自定义目录下创建脚本deploy.js
```
mkdir -p /carlson/service/webhook-handler
cd /carlson/service/webhook-handler
vi deploy.js
```

	1.3. 在deploy.js中输入以下代码，配置服务端口及密码，注意服务端口需在腾讯云入站规则中放行
```
var http = require('http')
var createHandler = require('/carlson/component/nodejs/node-v10.16.3-linux-x64/lib/node_modules/github-webhook-handler')
var handler = createHandler({ path: '/webhook', secret: '你的密码' })

function run_cmd(cmd, args, callback) {
  var spawn = require('child_process').spawn;
  var child = spawn(cmd, args);
  var resp = "";

  child.stdout.on('data', function(buffer) { resp += buffer.toString(); });
  child.stdout.on('end', function() { callback (resp) });
}

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(服务端口)

handler.on('error', function (err) {
  console.error('Error:', err.message)
})

handler.on('push', function (event) {
  console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref);
  run_cmd('sh', ['./start_blog.sh'], function(text){ console.log(text) });
})
```

	1.4. 同级目录下start_blog.sh代码如下
```
cd /carlson/service/blog
git pull
hexo server -p 80
```

	1.5. 安装forever依赖`npm install forever -g`

	1.6. 发布webhook服务`forever start deploy.js`或`forever start -l forever.log -o out.log -e err.log deploy.js`

	1.7. 在github仓库中添加推送回调，当收到push时会调用上方脚本进行自动化构建。此时，整个博客已经搭建完成。
	![仓库添加webhook回调](/img/仓库添加webhook.png)

# 开发流程
完成搭建后，已可在任意环境下拉取代码、编写以及推送，下面开发流程只作简单介绍

## 编写文章

1. 安装git依赖

2. 克隆仓库`git clone https://github.com/13631372596/TestBlog.git`

3. 绑定github仓库`git remote add origin https://github.com/13631372596/Blog.git`

4. 安装npm及hexo依赖

5. 创建新文章`hexo new test`，文件会在"/博客目录/source/_posts/"中生成，可查看参考文献中的《hexo语法》

6. 对test.md进行编写，可查看参考文献中的《Markdown基本语法》

7. 本地发布服务`hexo server -p 80`，可同时修改test.md并访问地址localhost进行测试

8. 编写完成后推送至github
```
git add .
git commit -m "提交信息"
git push 
```

## 调整博客样式
可查看参考文献中的《hexo-theme-butterfly安裝文檔》

1. 修改主题为Butterfly
	1.1. 下载主题`git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly`

	1.2. 复制Butterfly整个文件夹至"博客目录/themes/"下，然后修改"博客目录/_config.yml"中的主题"theme: Butterfly"，重新发布后即可看到效果。

2. 一些推荐修改的配置（后续更新）
	

# 参考文献
1. [程序员如何搭建自己的个人博客 - 方志朋](https://www.cnblogs.com/forezp/p/9852069.html)
2. [最全Hexo博客搭建 - 遇见西门](https://www.simon96.online/2018/10/12/hexo-tutorial/)
3. [hexo语法 - 官网](https://hexo.io/zh-cn/docs/commands.html)
4. [Markdown基本语法 - 高鸿祥](https://www.jianshu.com/p/191d1e21f7ed)
5. [hexo-theme-butterfly安裝文檔 - jerryc（该blog主题作者）](https://docs.jerryc.me/)