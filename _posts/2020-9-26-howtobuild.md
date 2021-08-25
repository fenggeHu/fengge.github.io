[仓库](https://github.com/csjue/csjue.github.io)

我的需求是，在pc端写markdown，需要能同步，偶尔要在手机上查看。尽量能保存markdown源码。

我想到了之前折腾的Github Pages，我之前是用Gridea来搭建的。但是如果要上传markdown，我需要复值粘贴到Gridea自带的编辑器，而且我发现他对markdown的支持不行。

所以就是用了标准模板的Github Pages，他完美的符合了我的要求，只要上传markdown，他就会自动帮我生成html。一方面我可以使用网页来查看，另一方面我可以到仓库里看 markdown源码。而且同步非常方便

以下教程 需要的前置知识：基础的git 和 GitHub，会使用markdown

如果哪一步不会，可以参考我的[仓库](https://github.com/csjue/csjue.github.io)

先创建一个名字为 用户名.github.io 的仓库

在仓库的setting/optional 可以找到GitHub Pages

![img](https://pic3.zhimg.com/v2-0bac8094e6efbdcbd77a866236bc7e6a_b.png)

开启GitHub Pages

然后去选个主题，我选的就是第一个 Cayman 

然后把这个仓库clone到本地。

![img](https://pic2.zhimg.com/v2-dd5ad5402b283807714f1361d2c80e41_b.png)

然后此时应该已经有了_config.yml [index.md](http://index.md/)

此时创建名为_posts的文件夹，以后markdown文件都写在这个文件夹下，注意：markdown的文件名应为这种格式 year-month-day-name.md   见上图。

然后把_config.yml修改

```
plugins:
  - jekyll-relative-links
relative_links:
  enabled: true
  collections: true
include:
  - CONTRIBUTING.md
  - README.md
  - LICENSE.md
  - COPYING.md
  - CODE_OF_CONDUCT.md
  - CONTRIBUTING.md
  - ISSUE_TEMPLATE.md
  - PULL_REQUEST_TEMPLATE.md
title: csjue's notebook
theme: jekyll-theme-cayman
```

注意：teme不要修改，title改为你自己想要的titile

此时index.md 是你的主页，如何从主页跳转到你的文章呢？

利用markdown语法

```
这个网站是用来存放我的markdown笔记，利用GitHub Pages搭建，直接使用官方主题，只要上传markdown就可以自动转换成html，相比在GitHub仓库，这个网站速度要快一点，观感也要好。

如果也想尝试，先建立一个pages仓库，参照我的[仓库](https://github.com/csjue/csjue.github.io)把_config.yml index.md修改。

[C++](_posts/2020-9-24-cpp.md)

[JavaScript](_posts/2020-9-25-JavaScript.md)

[算法](_posts/2020-9-25-algorithms.md)

[SQL](_posts/2020-9-25-SQL.md)

[csjue的力扣](_posts/2020-9-25-leetcode.md)

联系方式 csjue_sz@foxmail.com
```

这是我的index.md

然后上传到仓库。

就搞完了。

关于图床的问题。我是使用[Typora](https://typora.io/) markdown编辑器，他支持PicGo，所以我就用了[PicGo](https://picgo.github.io/PicGo-Doc/zh/)，使用了GitHub图床。

![img](https://pic1.zhimg.com/v2-08e0e6885a517d425f6e646569aaa85c_b.png)

我是直接在网站上设的图床仓库，你也可以再创建一个。

这里的token，是在[这个网站](https://github.com/settings/tokens)上，创建一个repro的token就可以了。

自定义域名写这个：

[https://raw.githubusercontent.com/用户名/仓库名/master](https://raw.githubusercontent.com/csjue/csjue.github.io/master)

建议PicGo要设置这两个

![img](https://pic1.zhimg.com/v2-dbc496795805d84af4092b513cebe22c_b.png)

上传前重命名和时间戳命名，因为GitHub图床要求名字应该不一样

应该就设置好了。