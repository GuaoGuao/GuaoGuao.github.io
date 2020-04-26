---
title: 一步步搭博客
date: 2018-10-25 22:04:12
tags:
---

# 写啊写博客
最近雪茄烟这边事情不多，瞎鼓捣鼓捣，看到网上有人做的[博客](https://www.cnblogs.com/yyhh/p/5140852.html#4092793)十分精美（其实是页面上的小人儿太好玩了），想着自己也做一个，先写这篇博客试试水。

# [Hexo](https://hexo.io/zh-cn/docs/)
## 关于安装
首先hexo肯定是必须的了，首先通过npm或者下载安装包安装，npm全局安装稍微慢一些但比较方便，命令为`$ npm install -g hexo-cli`注意hexo的安装之前最好安装了Node.js和git，不然就感觉后面都无法展开。

然后在github上建个仓库，为了方便，一般起名为`账户名.github.io`

下一步就建个文件夹，然后执行`hexo init`初始化你的博客，不过实话说这一步时间蛮长的，然后`npm install`安装hexo的依赖，这时候你的博客就安装完成了，心急的话想看看效果，可以执行`hexo g`，在执行`hexo s`，此时访问`localhost:4000`端口就能看到你的页面了。
![](/public/images/helloworld.png)

下一步是关联你的github账号和你本地的博客，首先更改本地的git账号和邮箱，执行`git config --global user.name "你的昵称"`，`git config --global user.email "你的邮箱"`，然后去生成你的ssh密钥，这个密钥跟你的账号邮箱是绑定的，如果你之前生成过还要再检查一下账号密码是否对应，命令是`ssh-keygen -t rsa -C "你的邮箱"`然后**一路回车**

打开你的github，设置里面新建一个ssh公钥，从你的.ssh文件中复制.pub文件拷贝进去，就完成了。
## 关于踩坑
这也不知道算不算坑，安装git后右键可以直接打开命令行，然后在这里面执行一些时间比较长的命令，会没有进度条，就让人怀疑人生，总觉得自己是不是没装啊

之前公司的项目用过git，然后配置了git账号邮箱，也生成了ssh，这里的ssh是不能用到github里的，因为账号密码公司的和github的不一样，因此需要生成两个ssh，费了一番功夫。
主要是执行`ssh-keygen -t rsa -C "你的邮箱"`执行后会让你输入文件路径文件名，输入后就有了两个ssh，然后新建一个config文件，注意尽量**每行尽量不要有空格**
```
# 起个别名1
Host gitlab.com
HostName gitlab.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/你的公司密钥
User 账号名
# 别名2
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/你的git密钥
User 账号名
```
其实好像还是不怎么行，公司那个偶尔出现问题，暂时的pull和push没问题，应该是主机名配置的原因
## 关于好看
可以使用主题，我用了[next](https://hexo.io/zh-cn/)主题。

博客里的小人用的是[live2d](https://github.com/EYHN/hexo-helper-live2d/blob/HEAD/README.zh-CN.md)模型，里面有二三十个模型，可以在`_config.yml`文件中修改`model.use`换换，这个模型可以交互，还有声音（一般人我不告诉他）。
![](/public/images/myblog.png)

写博客用的是markdown，这个需要再写一篇记下来，很容易忘记，可以再简书上面写，里面可以一边预览一遍写，十分方便。
写完之后的命令:
```
hexo clean          //清缓存
hexo generate       //生成静态文件
hexo server         //在4000端口上可以预览到
hexo deploy         //发布到github上
```
先写这么多把