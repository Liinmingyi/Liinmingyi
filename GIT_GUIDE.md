# Git 使用方法与说明

> 面向日常开发与学习的 Git 速查手册。全文使用 HTTPS 方式,命令以 `main` 分支为例。

## 1. 基础概念

| 概念 | 说明 |
| :--- | :--- |
| 工作区 (Working Directory) | 你在磁盘上看到的、正在编辑的文件 |
| 暂存区 (Staging Area) | 执行 `git add` 后,准备被提交的改动 |
| 本地仓库 (Repository) | `.git` 目录,保存所有提交历史 |
| 远程仓库 (Remote) | GitHub 上的仓库,如 `origin` |
| 提交 (Commit) | 一次不可变的快照,带哈希值和说明文字 |
| 分支 (Branch) | 指向某个提交的可移动指针,`main` 是默认主线 |

一个文件的典型流转: **工作区 → `git add` → 暂存区 → `git commit` → 本地仓库 → `git push` → 远程仓库**

## 2. 首次配置

```bash
# 提交身份(只对当前仓库生效;去掉 --local 则全局生效)
git config --local user.name  "Liinmingyi"
git config --local user.email "323918170+Liinmingyi@users.noreply.github.com"

# 常用默认值
git config --global init.defaultBranch main   # 新建仓库默认分支名为 main
git config --global core.quotepath false      # 正常显示中文文件名
git config --global core.autocrlf input       # Windows/macOS 混用时避免换行符噪音

# 查看所有配置
git config --list --show-origin
```

> 建议使用 GitHub 提供的 noreply 邮箱,避免把真实邮箱暴露在提交记录里。

## 3. 创建与克隆仓库

```bash
# 情况 A:本地已有文件夹,从零建库并关联远程
git init
git remote add origin https://github.com/<用户名>/<仓库名>.git

# 情况 B:远程仓库已有内容,直接克隆
git clone https://github.com/<用户名>/<仓库名>.git
```

## 4. 日常三件套:改、存、交

```bash
git status                 # 当前改动了什么(最常用,没事就敲一下)
git diff                   # 查看尚未暂存的具体改动
git diff --staged          # 查看已暂存、即将提交的改动

git add <文件路径>          # 暂存指定文件
git add .                  # 暂存当前目录下所有改动(先看 status 再用)

git commit -m "feat: 新增登录功能"          # 提交并写说明
git commit -am "fix: 修正拼写"              # 跳过 add(仅对已跟踪文件有效)
```

### 提交信息规范(推荐 Conventional Commits)

| 前缀 | 用途 | 示例 |
| :--- | :--- | :--- |
| `feat` | 新功能 | `feat: 支持按标签筛选` |
| `fix` | 修复缺陷 | `fix: 修正分页越界` |
| `docs` | 文档 | `docs: 补充 Git 使用说明` |
| `refactor` | 重构(不改行为) | `refactor: 拆分工具函数` |
| `test` | 测试 | `test: 补充边界用例` |
| `chore` | 构建/依赖/杂项 | `chore: 升级依赖` |

一条好的提交信息回答两个问题: **改了什么**(标题)和 **为什么改**(正文,可选)。

## 5. 查看历史

```bash
git log --oneline --graph --decorate --all   # 图形化一行式历史
git log -p <文件路径>                         # 某文件的逐次改动
git show <提交哈希>                           # 某次提交的完整内容
git blame <文件路径>                          # 每行代码是谁、何时改的
```

## 6. 与远程同步

```bash
git remote -v                     # 查看远程地址
git fetch origin                  # 只下载远程信息,不改工作区
git pull origin main              # 拉取并合并(等价于 fetch + merge)
git pull --rebase origin main     # 拉取并把本地提交"接"在最前面,历史更线性
git push -u origin main           # 首次推送并建立跟踪关系
git push                          # 之后直接推送当前分支
```

> 推送前先 `git pull`,能避免大部分非快进(non-fast-forward)错误。

## 7. 分支操作

```bash
git branch                       # 列出本地分支
git branch -a                    # 含远程分支
git switch -c feature/login      # 新建并切换到新分支
git switch main                  # 切回 main
git merge feature/login          # 把 feature 分支合并进当前分支
git branch -d feature/login      # 删除已合并的分支
```

### 推荐工作流(个人项目也适用)

```bash
git switch main
git pull                          # 1. 主线保持最新
git switch -c feature/xxx         # 2. 从主线拉新分支开发
# ... 修改、add、commit ...
git push -u origin feature/xxx    # 3. 推送分支
# 4. 在 GitHub 上开 Pull Request,评审后合并回 main
git switch main && git pull       # 5. 合并后同步主线
git branch -d feature/xxx         # 6. 清理本地分支
```

好处:主线始终可运行,改坏了只影响自己的分支。

## 8. 冲突处理

多人(或多分支)改了同一区域时,合并会停下并标记冲突:

```bash
git status                 # 查看 "both modified" 的文件
# 手动编辑冲突文件,保留正确内容并删除 <<<<<<< / ======= / >>>>>>> 标记
git add <已解决的文件>
git commit                 # 完成合并提交
```

放弃本次合并、回到干净状态:

```bash
git merge --abort
```

## 9. 撤销与后悔药

| 场景 | 命令 |
| :--- | :--- |
| 撤掉暂存(add 错了) | `git restore --staged <文件>` |
| 丢弃工作区未提交的修改 | `git restore <文件>` |
| 修改最后一次提交(信息或漏文件) | `git add <文件>` + `git commit --amend` |
| 撤回某次已推送到远端的提交 | `git revert <哈希>`(生成反向提交,最安全) |
| 本地提交未推送,想丢弃 | `git reset --soft HEAD~1`(保留改动) / `git reset --hard HEAD~1`(连改动一起丢) |

> `reset --hard` 会永久丢弃未提交内容,执行前确认没有想保留的改动。
> 已经推送的历史不要用 `reset` 重写,用 `revert`。

## 10. 暂存现场与临时保存

```bash
git stash              # 把未提交的改动临时收起来,工作区变干净
git stash list         # 查看已保存的现场
git stash pop          # 恢复最近一次 stash 并删除该记录
git stash apply        # 恢复但保留记录
```

适合"改到一半要切分支"的场景。

## 11. .gitignore

在仓库根目录新建 `.gitignore`,列出不该进版本库的内容:

```gitignore
# Python
__pycache__/
*.py[cod]
.venv/
.env

# C/C++
*.o
*.out
a.out

# 编辑器与系统
.vscode/
.idea/
.DS_Store

# 构建产物
build/
dist/
```

注意:`.gitignore` 只对**未被跟踪**的文件生效。已被跟踪的文件需先移除跟踪:

```bash
git rm --cached <文件>      # 停止跟踪但保留磁盘文件
```

## 12. SSH 方式(可选)

HTTPS 每次可能需要凭据;配置 SSH 后可免密推送:

```bash
ssh-keygen -t ed25519 -C "323918170+Liinmingyi@users.noreply.github.com"
cat ~/.ssh/id_ed25519.pub     # 复制输出,粘贴到 GitHub → Settings → SSH keys
ssh -T git@github.com         # 看到 "successfully authenticated" 即成功
git remote set-url origin git@github.com:<用户名>/<仓库名>.git  # 切换协议
```

## 13. 高频报错速查

| 报错 | 原因与处理 |
| :--- | :--- |
| `fatal: not a git repository` | 当前目录没有 `.git`,确认路径或用 `git init` |
| `rejected ... non-fast-forward` | 远端有新提交,先 `git pull --rebase` 再 `push` |
| `Please commit your changes or stash them` | 切换/拉取前有未提交改动,先 commit 或 `git stash` |
| `Author identity unknown` | 未配置身份,执行第 2 节的两条 `config` |
| `CONFLICT (content)` | 有冲突,按第 8 节手动解决 |
| `detached HEAD` | 直接检出了提交而非分支,`git switch main` 回到分支上工作 |

## 14. 安全习惯清单

- 动手前先 `git status`,推之前先 `git pull`。
- 一次提交只做一件事,信息写清楚,不要出现 `update`、`fix bug` 这类空话。
- 大改动走分支 + PR,别在 `main` 上直接试验。
- 密钥、密码、`.env` 一律不进仓库;万一提交了,立即换掉该凭据并清理历史。
- 定期 `git fetch` 了解远端变化,别攒一大批再合并。

---

## 15. 附录:Git 命令速查表

### 15.1 配置与帮助

| 命令 | 作用 |
| :--- | :--- |
| `git config --list --show-origin` | 查看全部配置及其来源文件 |
| `git config --global user.name "名字"` | 设置全局提交用户名 |
| `git config --local user.email "邮箱"` | 仅对当前仓库设置邮箱 |
| `git config --global --unset <键>` | 删除某项配置 |
| `git config --global init.defaultBranch main` | 新建仓库默认分支名 |
| `git help <命令>` / `git <命令> -h` | 查看命令帮助 |

常用别名(设置后可用 `git st`、`git lg` 等):

```bash
git config --global alias.st  status
git config --global alias.co  checkout
git config --global alias.br  branch
git config --global alias.ci  commit
git config --global alias.last "log -1 HEAD"
git config --global alias.lg  "log --oneline --graph --decorate --all"
```

### 15.2 仓库创建与克隆

| 命令 | 作用 |
| :--- | :--- |
| `git init` | 在当前目录初始化仓库 |
| `git init <目录>` | 在指定目录初始化仓库 |
| `git clone <url>` | 克隆远程仓库 |
| `git clone <url> <目录>` | 克隆到指定目录 |
| `git clone --depth 1 <url>` | 浅克隆,只取最近一次提交 |
| `git clone -b <分支> <url>` | 克隆并检出指定分支 |

### 15.3 状态与差异

| 命令 | 作用 |
| :--- | :--- |
| `git status` | 查看工作区状态 |
| `git status -sb` | 精简状态 + 当前分支 |
| `git diff` | 工作区 vs 暂存区 |
| `git diff --staged` | 暂存区 vs 最新提交 |
| `git diff <版本A>..<版本B>` | 两个提交/分支之间的差异 |
| `git diff --stat` | 只显示改动文件与行数统计 |

### 15.4 暂存、提交与文件管理

| 命令 | 作用 |
| :--- | :--- |
| `git add <文件>` | 暂存指定文件 |
| `git add -A` | 暂存全部改动(含新增与删除) |
| `git add -p` | 交互式按代码块暂存 |
| `git commit -m "说明"` | 提交并附带说明 |
| `git commit -am "说明"` | 暂存已跟踪文件并提交 |
| `git commit --amend` | 修改最后一次提交(信息或补文件) |
| `git commit --allow-empty -m "说明"` | 生成空提交(常用于触发 CI) |
| `git rm <文件>` | 删除文件并暂存删除动作 |
| `git rm --cached <文件>` | 停止跟踪但保留磁盘文件 |
| `git mv <旧路径> <新路径>` | 重命名或移动文件 |

### 15.5 查看历史

| 命令 | 作用 |
| :--- | :--- |
| `git log --oneline` | 一行式提交历史 |
| `git log --oneline --graph --all` | 图形化展示所有分支 |
| `git log -n 5` | 只看最近 5 条 |
| `git log --author="名字"` | 按作者筛选 |
| `git log --since="2 weeks ago"` | 按时间筛选 |
| `git log -p <文件>` | 某文件的逐次改动详情 |
| `git show <哈希>` | 查看某次提交的完整内容 |
| `git show <哈希>:<文件>` | 查看该提交下的文件内容 |
| `git blame <文件>` | 逐行追溯作者与提交 |
| `git reflog` | HEAD 移动记录,误 reset 后的救命命令 |
| `git shortlog -sn` | 按作者统计提交次数 |

### 15.6 分支与合并

| 命令 | 作用 |
| :--- | :--- |
| `git branch` | 列出本地分支 |
| `git branch -a` | 列出本地 + 远程分支 |
| `git branch -v` | 列出分支及各自最新提交 |
| `git branch <名称>` | 新建分支(不切换) |
| `git switch <名称>` / `git checkout <名称>` | 切换分支 |
| `git switch -c <名称>` | 新建并切换 |
| `git switch -c <新分支> <起点>` | 从指定提交/分支拉出新分支 |
| `git switch --detach <哈希>` | 临时检出某次提交(游离 HEAD) |
| `git branch -d <名称>` | 删除已合并的分支 |
| `git branch -D <名称>` | 强制删除未合并分支 |
| `git branch -m <旧名> <新名>` | 重命名分支 |
| `git merge <分支>` | 将目标分支合并进当前分支 |
| `git merge --no-ff <分支>` | 强制生成合并提交,保留分支痕迹 |
| `git merge --abort` | 放弃本次合并,回到合并前状态 |
| `git rebase <分支>` | 变基:把当前分支的提交接到目标分支之后 |
| `git rebase -i HEAD~3` | 交互式整理最近 3 次提交(合并/改信息/重排) |
| `git rebase --abort` | 放弃变基 |
| `git cherry-pick <哈希>` | 把某次提交复制到当前分支 |

### 15.7 远程协作

| 命令 | 作用 |
| :--- | :--- |
| `git remote -v` | 查看远程仓库地址 |
| `git remote add origin <url>` | 添加名为 origin 的远程 |
| `git remote set-url origin <url>` | 修改远程地址 |
| `git remote remove origin` | 移除远程 |
| `git remote prune origin` | 清理远程已删除分支的本地引用 |
| `git fetch origin` | 拉取远程更新(不合并) |
| `git fetch --prune` | 拉取并清理失效的远程分支引用 |
| `git pull` | 拉取并合并(fetch + merge) |
| `git pull --rebase` | 拉取并变基(历史更线性) |
| `git push` | 推送当前分支 |
| `git push -u origin <分支>` | 首次推送并建立跟踪关系 |
| `git push origin --delete <分支>` | 删除远程分支 |
| `git push --tags` | 推送所有本地标签 |
| `git push --force-with-lease` | 安全强推(仅本地历史确认无误时使用) |

### 15.8 撤销与回退

| 命令 | 作用 |
| :--- | :--- |
| `git restore <文件>` | 丢弃工作区中该文件的改动 |
| `git restore --staged <文件>` | 取消暂存,改动退回工作区 |
| `git restore --source=<哈希> <文件>` | 把文件恢复为某次提交的版本 |
| `git reset --soft HEAD~1` | 撤销最后一次提交,改动保留在暂存区 |
| `git reset HEAD~1` | 撤销提交与暂存,改动保留在工作区(默认 mixed) |
| `git reset --hard HEAD~1` | 撤销提交并丢弃改动(危险,不可恢复) |
| `git reset --hard origin/main` | 强制对齐远端 main(危险) |
| `git revert <哈希>` | 生成一个反向提交,安全撤销已推送内容 |
| `git revert <A>..<B>` | 撤销一个提交区间 |
| `git clean -n` | 预览将被删除的未跟踪文件 |
| `git clean -fd` | 删除未跟踪文件与目录(危险) |

### 15.9 标签(版本发布)

| 命令 | 作用 |
| :--- | :--- |
| `git tag` | 列出本地标签 |
| `git tag v1.0.0` | 创建轻量标签 |
| `git tag -a v1.0.0 -m "首个版本"` | 创建附注标签(带说明与作者信息) |
| `git show v1.0.0` | 查看标签详情 |
| `git tag -d v1.0.0` | 删除本地标签 |
| `git push origin v1.0.0` | 推送单个标签 |
| `git push origin --delete v1.0.0` | 删除远程标签 |

### 15.10 暂存现场(stash)

| 命令 | 作用 |
| :--- | :--- |
| `git stash` | 收起当前未提交的改动 |
| `git stash -u` | 连未跟踪文件一起收起 |
| `git stash list` | 查看已保存的现场列表 |
| `git stash pop` | 恢复最近一次现场并删除记录 |
| `git stash apply stash@{1}` | 恢复指定现场,保留记录 |
| `git stash drop stash@{0}` | 删除指定现场记录 |
| `git stash clear` | 清空全部现场记录 |

### 15.11 其他实用命令

| 命令 | 作用 |
| :--- | :--- |
| `git worktree add ../目录 <分支>` | 同一仓库多目录并行开发 |
| `git worktree list` | 查看所有工作树 |
| `git worktree remove <目录>` | 移除工作树 |
| `git bisect start` / `good` / `bad` | 二分查找引入缺陷的提交 |
| `git count-objects -vH` | 查看仓库占用体积 |
| `git gc` | 手动触发仓库垃圾回收与压缩 |
| `git fsck` | 检查仓库对象完整性 |
| `git archive --format=zip HEAD -o src.zip` | 导出当前提交的源码压缩包 |

### 15.12 一条命令搞定的高频组合

```bash
# 提交并推送当前分支
git add -A && git commit -m "说明" && git push

# 同步远端并保持线性历史
git pull --rebase && git push

# 查看当前分支相对 main 的差异
git diff main...HEAD --stat

# 找出某行代码是哪个提交引入的
git log -L 10,20:src/main.py

# 误操作后找回丢失的提交
git reflog                       # 找到哈希
git switch -c rescue <哈希>       # 用该哈希建分支找回
```

---

参考: [Git 官方文档](https://git-scm.com/doc) · [Pro Git(中文版)](https://git-scm.com/book/zh/v2)
</content>
