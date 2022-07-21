# GitHub
下面的一些操作将以本仓库为例
- [上手GitHub \| Bilibili](https://www.bilibili.com/video/BV1r3411F7kn) 12'37"
- \*[GitHub Docs](https://docs.github.com/cn)：部分汉化的官方文档
  * 建议通读[入门](https://docs.github.com/cn/get-started)一节

## 目录
- [GitHub](#github)
  - [目录](#目录)
  - [特殊文件](#特殊文件)
    - [README](#readme)
    - [LICENSE](#license)
  - [Issue](#issue)
  - [Fork](#fork)
  - [Pull Request（PR）](#pull-requestpr)
  - [...](#)



---
## 特殊文件
### README
“读我”文件 / 自述文件

在有些仓库中会存在名为`README`的文件，常为 Markdown`.md`格式

Github 会将这样的文件内容渲染在文件列表的下方，因此我们可以直接在目录中阅读它的内容而无需打开它

![README](https://s2.loli.net/2022/07/21/OGWoYXLsI7jKunQ.png)

- [关于自述文件 \| Github Docs](https://docs.github.com/cn/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)

### LICENSE
开源许可证

在有些仓库中会存在名为`LICENSE`的文件，无后缀名

Github 会根据该文件内容检测仓库的开源许可证类型，并显示在仓库页面右侧（如下图中`CC-BY-SA-4.0 license`）

![LICENSE-about](https://s2.loli.net/2022/07/21/krGWHIx2RE3JzM6.png)

可以点击它来查看详情

![LICENSE-detail](https://s2.loli.net/2022/07/21/MEcWvdpx8sGDZuy.png)

- [许可仓库 \| GitHub Docs](https://docs.github.com/cn/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)



## Issue
议题

- [GitHub Issues 快速入门 \| GitHub Docs](https://docs.github.com/cn/issues/tracking-your-work-with-issues/quickstart)



## Fork
复刻，简单来说就是一个**属于你的**仓库副本

例如，在[本仓库](https://github.com/ngc7331/UCAS-CS-Guide)页面的右上角，可以看到一个`Fork`按钮，按下它，将进入[创建 Fork](https://github.com/ngc7331/UCAS-CS-Guide/fork) 页面

如果您的账户属于某个组织，可以在左侧选择 Fork 到个人账户或组织账户

可以在右侧输入仓库副本的名字，如果没有特殊需要，也建议保持与原仓库一致（默认）

可以在下方输入仓库副本的说明

随后点击 ![Create fork](https://s2.loli.net/2022/07/21/emWUlYkBuKNInaf.png) 按钮即可创建复刻



## Pull Request（PR）
拉取请求

上面已经提到，为开源做贡献的一个重要方式是发起 issue。如果维护者没有能力或精力解决 issue，而您认为自己有能力也愿意自行解决，更好的办法是发起 PR

一个简单且常见的流程如下：

![pr-1.drawio.png](https://s2.loli.net/2022/07/21/LVvYgTw9osxal6W.png)

也就是说，首先创建一个原仓库的复刻，由于复刻是属于您的，您可以直接向该复刻仓库提交 commit（就像向其它任何一个属于您的仓库提交一样）

当提交了一个或数个 commit，并且经过了测试判断功能正常，则可以在 Github 上向原仓库发起 PR

- [从复刻创建拉取请求 \| GitHub Docs](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)

原仓库的维护者会对 PR 进行代码审查（code review），若存在可能的 bug 或代码风格等问题，维护者可能会要求您进行修改（也有可能维护者自行修改）

- [请求 PR 审查 \| GitHub Docs](https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review)

您可以随时将新的 commit 附加到一个未关闭的 PR，只需要继续向复刻仓库提交 commit

若一切准备就绪，维护者将接受拉取请求，将更改 merge 到主分支上，此时您可以删除 fork 仓库。如有需要，再次 fork 即可

有些比较大型的项目，为了管理方便，维护者可能会对 PR 的方式、格式、代码风格等提出具体要求，这些要求通常会写在仓库根目录下，名为`CONTRIBUTING`、`CODE_OF_CONDUCT`的文件中，有些人也会写在`README`中，请务必在做出贡献前仔细阅读

有时会将 PR 和 Issue 结合使用，即：首先提出一个 Issue，说明需要修复的 bug 或新增的 feature 等，用这种方式与仓库的维护者、贡献者和关注者进行讨论；随后按上述流程提交修改、发起 PR；在 PR 的说明中使用`fix <issue id>`、`close <issue id>`等关键词（见下方参考资料）即可将 PR 连接到 Issue，并在 PR 合并时自动关闭 Issue

- [将 PR 链接到议题 \| Github Docs](https://docs.github.com/cn/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)



## ...
TODO
