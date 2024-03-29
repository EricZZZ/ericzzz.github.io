# 再见了 WordPress，全面拥抱 Hugo


## 再见了 WordPress，全面拥抱 Hugo

2020 年 11 月的某天心血来潮，想开个 Blog 吧，说干就干，买服务器，买域名，远程安装 phpstudy，登录 phpstudy 后台，使用 WordPress 搭建个人 Blog 网站，最终完成了 Blog 搭建。花费不小，服务器和域名都是白花花的银子，不过是在促销活动中买的省了不少钱（ps：暂时安慰自己😭）。

WordPress 虽然号称全球有 40%的网站是用它构建的，但是毕竟时代在进步，以前 PHP 横扫互联网的日子一去不复返了，毕竟它与服务器是动态交互的，数据是存储在数据库中的，黑客们会不断请求服务器寻找软件漏洞，而且经常有一些不友好的评论，虽然有评论拦截器，但还是深受其扰，经常登录后台看到有几千条垃圾评论。

直到 2022 年的某一天，静态 Blog 搭建进入我的视线。操作优雅，而且关键省去了服务器的钱。从此之后 Blog 迁移提上日程（ps: 其实也没有多少文章😅）。

### 安装 Hugo

[Hugo](https://gohugo.io) 是一个用 Go 语言编写的静态网站生成器。

使用 Hugo 创建网站之前

1. 需要 [安装 Hugo](https://gohugo.io/installation/)
2. 需要 [安装 Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

完成上述步骤之后，让我们本地创建新的 Hugo 网站吧。

#### 1. 创建项目

```bash
hugo new site my_website
cd my_website
```

#### 2. 安装主题

Hugo 提供了 [丰富的主题](https://themes.gohugo.io/)，可在官网中预览主题，挑选自己喜欢的主题。

我选择的是 [LoveIt](https://hugoloveit.com/zh-cn/) 主题。

初始化你的项目目录为 git 仓库， 并且把主题仓库作为你的网站目录的子模块：

```bash
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

#### 3.Hugo 配置

以 LoveIt 主题为例

```toml
baseURL = "http://example.org/"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

# 网站标题
title = "我的全新 Hugo 网站"

# 网站语言，仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
# 是否包括中日韩文字
hasCJKLanguage = true

# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""

# 菜单配置
[menu]
  [[menu.main]]
    weight = 1
    identifier = "posts"
    # 你可以在名称 （允许 HTML 格式） 之前添加其他信息，例如图标
    pre = ""
    # 你可以在名称 （允许 HTML 格式） 之后添加其他信息，例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时，将显示的标题
    title = ""
  [[menu.main]]
    weight = 2
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
  [[menu.main]]
    weight = 3
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false

```

#### 4. 启动服务

```bash
hugo serve
```

执行后本地访问 http://localhost:1313 就能看到 Hugo 生成的网站了

### 配置 GitHub Pages

网站生成了，怎么把他放到互联网上呢🤔，这里用到了 [GitHub Pages](https://pages.github.com/)。

GitHub Pages 是 GitHub 提供的一个网页代管服务，于 2008 年推出。可以用于存放静态网页，包括 Blog，项目文档甚至整本书。

首先创建个公有仓库，用于存放 Hugo 构建后生成的静态文件，仓库名必须是** user.github.io**，**user **是你的 GitHub 用户名且全小写。

创建完成后点击仓库的 Settings，选择 Pages 打开，打开之后的效果。

![gitpages](https://image.ericzzz.com/2022/11/23/2022.11.23/gitpages.webp)

#### 自动发布

精髓，优雅的地方来了~🤩

有了 Git 公有仓库，虽然每次写完文章，本地 Hugo 生成静态文件后，再推到远程仓库上，但是操作繁琐。不应该是写好文章自动部署到公有仓库上嘛？

你想到的 Github 它想不到嘛？🤣

隆重推出 Github 家又一款明星产品 [GitHub Action](https://github.com/features/actions)，GItHub Actions 是一个持续集成和持续交付的平台，能够让你自动化你的编译、测试和部署流程。

简单说的就是，Action 会监听仓库某些事件（如提交），触发一些操作（编译，计算，打包等），这里就是所说的自动化。

配置 Action 工作流，需要 git 仓库根目录下创建**.github/workflows **目录，配置文件**.yml **结尾，以下是我的配置文件内容。

```yaml
name: deploy

on:
    push:
    workflow_dispatch:
    schedule:
        # Runs everyday at 8:00 AM
        - cron: "0 0 * * *"

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: EricZZZ/ericzzz.github.io
                  PUBLISH_BRANCH: master
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
```

`PERSONAL_TOKEN` ：因为是从私有仓库推到 GitHub Pages 的公有仓库上需要权限，这里需要创建个 Token，权限开启 `repo` 与 `workflow`。

`PUBLISH_BRANCH`：监听哪个分支。

`PUBLISH_DIR`：编译后生成的静态文件目录。

`EXTERNAL_REPOSITORY`：推送到的远程仓库地址。

![gittoken](https://image.ericzzz.com/2022/11/23/2022.11.23/gittoken.webp)

再到私有仓库 setting 里的 Actions secrets，把刚才的 Token 添加进去。

![actiontoken](https://image.ericzzz.com/2022/11/23/2022.11.23/actiontoken.webp)

好了万事俱备，只欠东风。在私有仓库里创建一篇文章并提交到远程。查看 Action 是否触发构建。

如果 Action 运行正常后，通过访问 http://ericzzz.github.io 查看刚才自己提交的文章有没有部署成功。

![actiondep](https://image.ericzzz.com/2022/11/23/2022.11.23/actiondeploy.webp)

### 配置 Netlify

到现在为止网站已经部署成功了，但是用的 GitHub 的流量，在国内访问可就太痛苦了，网站经常会打不开。怎么办呢？使用 Netlify 来部署咱们的静态网站。

Netlify 跟 GitHub 搭配也很好用，不用配置 Action 了。

创建 Netlify 账号并与 GitHub 关联。

新建网站，选择你的私有仓库，选择分支，使用 Hugo 构建，静态 public 目录。

![netlify](https://image.ericzzz.com/2022/11/23/2022.11.23/netlify.webp)

部署完成后通过 netlify 分配的域名访问 https://rad-seahorse-8dd9c0.netlify.app

### 配置域名

到目前为止，用的都是第三方平台的二级域名，这样不利于建立个人品牌。

配置属于自己域名的 Blog。

#### 购买域名

购买域名平台很多，国内的阿里云，腾讯云，华为云等，国外有亚马逊，GoDaddy 等，不过在国内域名都要备案，稍微有一丢丢麻烦。

#### Netlify 域名配置

购买域名后，我们需要用自己的域名访问到 Netlify 给分配的网站 https://rad-seahorse-8dd9c0.netlify.app。

我是在 GoDaddy 平台上购买的域名，用 GoDaddy 来举例。

##### 修改 DNS

先去 Netlify 新增一个域名，Netlify 会告诉你要去域名提供商添加一条 CNAME 记录。再到 GoDaddy 后台 DNS 管理那里新增一条记录。（ps: 这里 A 标签也改成了 Netlify 的地址）

![dns](https://image.ericzzz.com/2022/11/23/2022.11.23/dns.webp)

新增完成后过上一会儿，Netlify 自动检查成功就能通过自己域名访问了，记得在 Netlify 中把 HTTPS 配置上 https://miasanmia.cc/

### 大功告成🎉

以上就完成了静态 Blog 网站的搭建，以后每写一篇文章提交到远程仓库，网站就会自动部署发布新文章了🙂。

### 其他

#### 配置 Google Analytics，Google Search Console

修改 Hugo 配置文件** config.toml**，我用** LoveIt **主题刚好有配置的地方，把信息填上即可。

[Google Analytics](https://analytics.google.com/) 登录后台，搜索** Measurement**，就能找到谷歌分析代码。如下图：

![Google Analytics](https://image.ericzzz.com/2022/11/23/2022.11.23/analytics-1.webp)

```toml
  #  网站分析配置
  [params.analytics]
    enable = true
    # Google Analytics
    [params.analytics.google]
      id = "" #这里填 Google 分析代码
      # 是否匿名化用户 IP
      anonymizeIP = true
```

设置完成后，别忘了到 [Google Analytics](https://analytics.google.com/) 管理后台与 Google Search Console 关联，[Google Search Console](https://search.google.com/search-console/about) 帮助人们管理 Google SEO。

![Google Search Console 关联](https://image.ericzzz.com/2022/11/23/2022.11.23/search-1.webp)

给 Google 提交 sitemap 方便快速建立索引，增加你的网站曝光率。

使用 Hugo 建站会自动生成 sitemap，地址是在根目录下，也就是 https://你的域名/sitemap.xml。例如本站的 sitemap 地址就是 https://miasanmia.cc/sitemap.xml

#### 配置评论

网站建好了，该有的评论也整上，很简单不需要额外的花费。

这里用到的评论系统插件是 [giscus](https://giscus.app/zh-CN)，下面是使用 giscus 的必要条件，尤其是 2，3 一定要去 github 上开启。

1. **此仓库是 [公开的](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/setting-repository-visibility#making-a-repository-public)**，否则访客将无法查看 discussion。
2. **[giscus](https://github.com/apps/giscus) app 已安装**否则访客将无法评论和回应。
3. **Discussions **功能已 [在你的仓库中启用](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository)。

检测仓库是否满足条件。

![giscus](https://image.ericzzz.com/2022/11/23/2022.11.23/giscus-1.webp)

开启时之后，在 [giscus](https://giscus.app/zh-CN) 页面上填上你的公开仓库地址，自己选择配置，[giscus](https://giscus.app/zh-CN) 会自动生成配置，我们需要把这些配置填到 Hugo 的** config.toml **文件里。

![giscus](https://image.ericzzz.com/2022/11/23/2022.11.23/giscus-2.webp)

```toml
   # giscus comment 评论系统设置 (https://giscus.app/zh-CN)
      [params.page.comment.giscus]
        # 你可以参考官方文档来使用下列配置
        enable = true
        repo = "xxxxx"
        repoId = "xxxxx"
        category = "Announcements"
        categoryId = "xxxxxxx"
        # 为空时自动适配当前主题 i18n 配置
        lang = ""
        mapping = "pathname"
        reactionsEnabled = "1"
        emitMetadata = "0"
        inputPosition = "bottom"
        lazyLoading = true
        lightTheme = "light"
        darkTheme = "dark"
```

部署之后就能在文章下面看到评论区了，看到这里的朋友们还不到评论区里留下你的痕迹🤗。

#### 修改字体，大小，行高
[LoveIt](https://hugoloveit.com/zh-cn/) 主题默认的字体不美观，网上非常好看的中文开源字体 [霞鹜文楷](https://github.com/lxgw/LxgwWenKai)。

LoveIt 设置自定义样式，在根目录建立文件 `assets/css/_override.scss` , 并添加以下内容，这个文件可以覆盖 `themes/LoveIt/assets/css/_variables.scss` 中的变量以自定义样式。

```scss
@import url('https://npm.elemecdn.com/lxgw-wenkai-screen-webfont/style.css');// 引入字体 CDN
$global-font-family: "LXGW WenKai Screen", sans-serif;// 修改字体
$global-font-size: 18px;// 修改字体大小
$global-line-height: 1.8rem;// 修改行高
```

### 参考链接

[Hugo + GitHub Action，搭建你的博客自动发布系统](https://www.pseudoyu.com/zh/2022/05/29/deploy_your_Blog_using_hugo_and_github_action/)

