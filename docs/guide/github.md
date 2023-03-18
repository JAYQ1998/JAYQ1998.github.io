<!-- github使用技巧 -->

## 1.搜索

- 搜索名字      in:name xXX
- 搜索描述      in:description xxX
- 搜索readme    in:readme xxx
- 按stars       stars:>2000
- 按fork        fork:>3000
- 仓库大小搜索  size:>=5000 [说明5000大小是5000k]
- 按更新时间    pushed:>2020-01-01
- 按语言        language:xxx
- 按作者名     user:xxx[注意:后面不要有空格]
- 搜索的方式可以组合
- 更多高级搜索 https://github.com/search/advanced

## 2.阅读源码

Github 前段时间推出的 Codespaces 可以提供类似 VS Code 的在线 IDE，不过目前还没有完全开发使用。

### 克隆项目到本地

先把项目克隆到本地，然后使用自己喜欢的 IDE 来阅读。可以说是最酸爽的方式了！

如果你想要深入了解某个项目的话，首选这种方式。一个`git clone` 就完事了。

## 3.Chrome 插件

### Enhanced GitHub-扩展 Github 的功能

**Enhanced GitHub** 可以让你的 Github 更好用。这个 Chrome 插件可以可视化你的 Github 仓库大小，每个文件的大小并且可以让你快速下载单个文件。

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107160817672.png)

### Octotree-仓库树形结构展示

这个已经老生常谈了，是我最喜欢的一种方式。使用了 Octotree 之后网页侧边栏会按照树形结构展示项目，为我们带来 IDE 般的阅读源代码的感受。

![Chrome插件Octotree](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107144944798.png)

## 4.Github Explore

Github 自带的 Explore 是一个非常强大且好用的功能。

Github Explore 可以为你带来下面这些服务：

1. 可以根据你的个人兴趣为你推荐项目；
2. Githunb Topics 按照类别/话题将一些项目进行了分类汇总。比如 [Data visualization](https://github.com/topics/data-visualization) 汇总了数据可视化相关的一些开源项目，[Awesome Lists](https://github.com/topics/awesome) 汇总了 Awesome 系列的仓库；
3. 通过 Github Trending 我们可以看到最近比较热门的一些开源项目，我们可以按照语言类型以及时间维度对项目进行筛选；
4. Github Collections 类似一个收藏夹集合。比如 [Teaching materials for computational social science](https://github.com/collections/teaching-computational-social-science) 这个收藏夹就汇总了计算机课程相关的开源资源，[Learn to Code](https://github.com/collections/learn-to-code) 这个收藏夹就汇总了对你学习编程有帮助的一些仓库；
5. ......

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/github-explore.png)

## 一键生成 Github 简历 & Github 年报

通过 [https://resume.github.io/](https://resume.github.io/) 这个网站你可以一键生成一个在线的 Github 简历。

当时我参加的校招的时候，个人信息那里就放了一个在线的 Github 简历。我觉得这样会让面试官感觉你是一个内行，会提高一些印象分。

但是，如果你的 Github 没有什么项目的话还是不要放在简历里面了。生成后的效果如下图所示。

![Github简历](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201108192205620.png)

通过 https://www.githubtrends.io/wrapped 这个网站，你可以生成一份 Github 个人年报，这个年报会列举出你在这一年的项目贡献情况、最常使用的编程语言、详细的贡献信息。

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/dootask/image-20211226144607457.png)

## 个性化 Github 首页

Github 目前支持在个人主页自定义展示一些内容。展示效果如下图所示。

![个性化首页展示效果](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/java-guide-blog/image-20210616221212259.png)

想要做到这样非常简单，你只需要创建一个和你的 Github 账户同名的仓库，然后自定义`README.md`的内容即可。

展示在你主页的自定义内容就是`README.md`的内容（_不会 Markdown 语法的小伙伴自行面壁 5 分钟_）。

![创建一个和你的Github账户同名的仓库](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/java-guide-blog/image-20201107110309341.png)

这个也是可以玩出花来的！比如说：通过 [github-readme-stats](https://hellogithub.com/periodical/statistics/click/?target=https://github.com/anuraghazra/github-readme-stats) 这个开源项目，你可以 README 中展示动态生成的 GitHub 统计信息。展示效果如下图所示。

![通过github-readme-stats动态生成GitHub统计信息 ](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/java-guide-blog/image-20210616221312426.png)

关于个性化首页这个就不多提了，感兴趣的小伙伴自行研究一下。

## 自定义项目徽章

你在 Github 上看到的项目徽章都是通过 [https://shields.io/](https://shields.io/) 这个网站生成的。我的 JavaGuide 这个项目的徽章如下图所示。

![项目徽章](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107143136559.png)

并且，你不光可以生成静态徽章，shield.io 还可以动态读取你项目的状态并生成对应的徽章。

![自定义项目徽章](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107143502356.png)

生成的描述项目状态的徽章效果如下图所示。

![描述项目状态的徽章](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107143752642.png)

## 自动为项目添加贡献情况图标

通过 repobeats 这个工具可以为 Github 项目添加如下图所示的项目贡献基本情况图表，挺不错的 👍

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/dootask/repobeats.png)

地址：https://repobeats.axiom.co/ 。

## Github 表情

![Github表情](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107162254582.png)

如果你想要在 Github 使用表情的话，可以在这里找找 ：[www.webfx.com/tools/emoji-cheat-sheet/ ](www.webfx.com/tools/emoji-cheat-sheet/)。

![在线Github表情](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-11/image-20201107162432941.png)

## GitHub Actions 很强大

你可以简单地将 GitHub Actions 理解为 Github 自带的 CI/CD ，通过 GitHub Actions 你可以直接在 GitHub 构建、测试和部署代码，你还可以对代码进行审查、管理 API 、分析项目依赖项。总之，GitHub Actions 可以自动化地帮你完成很多事情。

关于 GitHub Actions 的详细介绍，推荐看一下阮一峰老师写的 [GitHub Actions 入门教程](https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html) 。

GitHub Actions 有一个官方市场，上面有非常多别人提交的 Actions ，你可以直接拿来使用。

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/image-20211227100147433.png)

## 后记

这一篇文章，我毫无保留地把自己这些年总结的 Github 小技巧分享了出来，真心希望对大家有帮助，真心希望大家一定要利用好 Github 这个专属程序员的宝藏。
