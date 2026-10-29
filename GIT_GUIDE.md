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

参考: [Git 官方文档](https://git-scm.com/doc) · [Pro Git(中文版)](https://git-scm.com/book/zh/v2)
</content>
