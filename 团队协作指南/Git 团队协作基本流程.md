我们将使用 Git 作为团队协作开发的工具。

为了让大家能够非常清晰直观地了解协作开发的流程，大家在看的时候可以打开 Learn Git Branching 的[沙箱运行环境](https://learngitbranching.js.org/?NODEMO)来实践（可以直接输入提供的代码）。在左边的终端中输入命令，就会在右边看到相应的动画。其中左边的实线圆圈代表本地仓库，右边的虚线圆圈代表远程仓库，`*` 号指向的是当前分支，`o/master` 就是远程分支（ `o` 就相当于 `origin` ）。

![image.png | left | 747x441](https://cdn.yuque.com/yuque/0/2018/png/125564/1531141582351-a11bb751-92e5-489d-a869-8236cb1462b1.png)

**由于 Learn Git Branching 为了演示和学习的方便对部分命令做了简化，我将指出在实际操作中应当输入的命令。**

接下来将重点讲述以下两个流程：

- 贡献代码
- 更新本地仓库

## 贡献代码

接下来的流程描述了在接到开发任务后，如何为中心仓库贡献代码。

### 将仓库 clone 到本地

```bash
$ git clone
```

> 实际命令应当提供 URI 参数，例如：
>
> ```bash
> $ git clone https://github.com/dhucst/cooperation.git
> ```

### 开启新分支

```bash
$ git checkout -b B1
```

为了便于演示，我们将新分支命名为 `B1`。在实际开发中，新分支的命名应当遵循以下原则：

- 使用 kebab-case，例如 `new-branch` ，而不是 `new_branch` 或 `newBranch`
- 尽量能概括这个分支所要完成的任务
- 如果是为了解决某个 Issue，在最后加上 Issue 的编号，例如 `fix-75`

### 编写代码并提交

```bash
$ git commit
```

> 实际命令应当要先执行 `git add` 来将修改的文件添加到暂存区，例如：
>
> ```bash
> $ git add .
> $ git commit
> ```

Commit Message (Log) 的书写是有比较严格的规范的，会在此知识库的[提交信息书写规范](https://yuque.com/dhucst/team-collaboration/commit-messages)中详细阐述。

### 推送分支

```bash
$ git push
```

> 实际命令在**第一次** push 任何分支时，应当指定 remote 和分支名称：
>
> ```bash
> $ git push origin B1
> ```

有时候我们的分支会在一夜之间“过时”。什么是过时的分支，我们该怎样处理？不要方，后面会讲到。

### 提交 Pull Request

> 这一步骤无需在 Learn Git Branching 中操作。

将分支提交到远程仓库后，打开仓库的 GitHub 页面，应该会看到下面这样黄色的提示框：

![image.png | left | 747x278]("")

然后点击 Compare & pull request 按钮，即可进入到提交 Pull Request 页面。

![QQ20180709-191751@2x.png | center | 747x485](https://cdn.yuque.com/yuque/0/2018/png/125564/1531139030880-79efc1ca-5666-4311-a019-7cbbda3eacb7.png)

填写 Pull Request 标题所遵循的原则与 Commit message 大致相似。在填写 Pull Request 的详细内容时，如果是为了解决某个或多个 Issue 时，可以使用 `Close(s)`, `Fix(es)` 或 `Resolve(s)` 关键词来关闭某个 Issue，例如 `Fix #75` 。

点击 Create pull request 按钮后，即可完成本次 PR。如果经讨论后发现需要修改，则在本地仓库修改后直接 `git push` 继续提交即可。如果代码通过了评审，则会由项目管理者将此分支并入 master 中，本次贡献代码流程结束。

## 更新本地仓库

接下来的流程介绍了当团队其他成员贡献代码后，如何将远程仓库的更新同步到本地。

如果你在使用 Learn Git Branching 边看边练，请输入以下命令：

```bash
$ reset
$ git clone
```

### 其他成员贡献代码

```bash
$ git fakeTeamwork 2
```

> 实际没有这条 Git 命令 :joy:，是 Learn Git Branching 提供用于练习协作的。

这时候你会发现远程的仓库有了本地没有的提交 `C2` 和 `C3` 。

### 拉取远程代码

我们先来看第一种比较简单的情况：

![image.png | left | 747x444](https://cdn.yuque.com/yuque/0/2018/png/125564/1531142706844-211e6509-cbea-46e0-be65-922b62d56869.png)

这时候一眼就可以看出，只需把远程的 `C2` 和 `C3` 直接拉取过来接在本地的 `C1` 后面就可以了：

```bash
$ git pull
```

接着我们来看另一种比较棘手的情况：

![image.png | left | 747x400](https://cdn.yuque.com/yuque/0/2018/png/125564/1531143485007-49478fc8-eb02-4220-8b34-95e17816e1ba.png)

需要输入的命令如下：

```bash
$ reset
$ git clone
$ git checkout -b B2
$ git commit
$ git fakeTeamwork 2
```

对着图看，我们在 `B2` 分支上在开发某个新功能，这时候远程仓库已经更新到了 `C4`，很显然我们本地的 master 分支和 `B2` 分支都不是最新的了。这种情况很常见：几个小伙伴从同一个起点（在这里就是 `C1` ）各自开发新功能时，其他人先于我们提交。

大多数情况下，请遵循这一条原则：**只更新 master 分支。**

> 这一原则对于并行开发并不适用，我们会在本知识库后续文档中讲解。

```bash
$ git checkout master
$ git pull
```

![image.png | left | 747x422](https://cdn.yuque.com/yuque/0/2018/png/125564/1531144915702-54d9183d-f47c-4c05-a35f-d5fd1a907310.png)

这时候，我们就会认为 `B2` 分支已经**过时**（outdated），因为它没有最新的 `C3` 和 `C4` 。但过时的分支并不意味着没有价值了，我们可以像前面所讲解的那样 push 到远程仓库：

```bash
$ git checkout B2
$ git push
```

然后一样可以发起 Pull Request。GitHub 会提示你这条分支已经过时，你可以点击 Update Branch 按钮来更新这一条分支（通常由项目管理者来执行这一操作）。

## 小结

团队协作开发的模型只涉及两个核心流程：贡献代码和更新本地仓库。

![image.png | left | 747x532](https://cdn.yuque.com/yuque/0/2018/png/125564/1531146573677-7d5eddfa-822c-45c2-b618-2c8b065939ac.png)

贡献代码的流程：

```bash
$ git clone <REPO_URI>
$ git checkout -b new-branch
$ git add .
$ git commit
$ git push origin new-branch
```

更新代码库的流程：

```bash
$ git checkout master
$ git pull
```
