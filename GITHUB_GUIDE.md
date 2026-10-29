# GitHub 新手入门指南

> 面向第一次接触 GitHub 的同学。读完你会知道:它是什么、能做什么、怎么开始、怎么避免踩坑。

## 1. GitHub 是什么

GitHub 是全球最大的**代码托管与协作平台**,底层使用 Git 做版本管理,并在 Git 之上提供了网页界面、协作工具和自动化能力。

| | Git | GitHub |
| :--- | :--- | :--- |
| 本质 | 本地版本控制工具(软件) | 托管 Git 仓库的在线平台(网站) |
| 位置 | 装在你的电脑上 | 运行在云端 |
| 作用 | 记录代码的每一次变化 | 存储、展示、协作、自动化 |
| 能否离线 | 可以 | 不行 |

一句话:**Git 是"怎么记录",GitHub 是"记录放哪里、和谁一起看"**。

## 2. GitHub 能干什么

| 场景 | 说明 | 用到的功能 |
| :--- | :--- | :--- |
| 代码托管与备份 | 代码放云端,换电脑只需 clone | Repository |
| 作品集展示 | 招聘方会看你的 GitHub 主页和项目 | Profile README、Pinned repos |
| 多人协作 | 各自开分支开发,通过 PR 合并 | Branch、Pull Request、Review |
| 开源贡献 | 给别人的项目提 Issue / PR | Fork、Issue、PR |
| 自动构建与测试 | 每次推送自动跑测试、打包、发布 | GitHub Actions |
| 免费建站 | 静态网站/博客直接托管 | GitHub Pages |
| 学习与阅读源码 | 看别人怎么写项目、修 bug | Explore、Code Search |
| 写笔记与文档 | README、Wiki、Gist 都能当笔记 | Markdown、Wiki、Gist |
| 版本发布 | 打标签、附安装包、写更新日志 | Releases、Tags |
| 任务管理 | 用看板管理待办与进度 | Issues、Projects |
| 社区与交流 | 关注大佬、讨论问题、Star 收藏 | Watch、Star、Discussions |

对学生还有额外福利:**GitHub Student Developer Pack**(用学校邮箱认证后可免费领取 Copilot、云服务、域名等资源)。

## 3. 核心概念地图

| 概念 | 一句话解释 |
| :--- | :--- |
| Repository(仓库) | 一个项目,包含全部代码与提交历史 |
| README | 仓库首页自动展示的说明文档 |
| Commit(提交) | 一次代码快照,带哈希与说明 |
| Branch(分支) | 从主线分出的一条独立开发线 |
| Clone(克隆) | 把远程仓库完整下载到本地 |
| Fork(复刻) | 把别人的仓库复制一份到自己账号下 |
| Star(收藏) | 类似点赞 + 收藏,表示"这个项目不错" |
| Watch(关注) | 订阅仓库动态,有 Issue/PR 会通知你 |
| Issue(议题) | 记录 bug、需求、讨论的地方 |
| Pull Request(PR) | 请求把自己的分支合并进目标分支,可附带代码评审 |
| Review(评审) | 对 PR 逐行评论、提出修改意见 |
| Merge(合并) | 把分支改动并入目标分支 |
| Release(发布) | 给某个版本打标签并附说明/安装包 |
| GitHub Actions | 自动化流水线,可跑测试、部署、定时任务 |
| GitHub Pages | 直接从仓库托管静态网站 |
| Projects | 看板式任务管理,可关联 Issue 与 PR |
| Discussions | 仓库内的论坛式讨论区 |
| Wiki | 仓库内的文档区,适合写长文档 |
| Gist | 临时代码片段/笔记分享,不必建仓库 |
| Codespaces | 云端的 VS Code 开发环境 |
| Copilot | AI 编程助手(部分功能需付费/学生包) |

## 4. 注册与账号设置

1. 打开 [github.com](https://github.com) 注册,用户名会出现在所有仓库地址里,尽量用真名拼音或稳定 ID。
2. 验证邮箱,否则部分功能受限。
3. **开启两步验证(2FA)**:Settings → Password and authentication → Two-factor authentication。GitHub 对未开 2FA 的账号会限制部分操作。
4. 完善个人主页:头像、简介、所在地。
5. 创建 **Profile README**:新建一个与用户名同名的仓库(如 `Liinmingyi/Liinmingyi`),里面的 `README.md` 会显示在你的主页顶部。
6. 配置 SSH(可选,免密推送),见 [Git 使用指南](GIT_GUIDE.md) 第 12 节。
7. 学生可认证 GitHub Student Developer Pack。

## 5. 第一次上手:从零到第一个仓库

### 方式 A:网页直接建仓库(最简单)

1. 右上角 `+` → New repository
2. 填仓库名、描述,勾选 `Add a README file`
3. 按需选择 `.gitignore` 模板与开源许可证
4. Create repository

之后可直接在网页里点文件名 → 铅笔图标编辑 → 底部 Commit changes。

### 方式 B:本地项目推上 GitHub

```bash
# 1. GitHub 上先建一个空仓库(不要勾 README)
# 2. 本地初始化
git init
git add .
git commit -m "feat: 初始提交"

# 3. 关联远程并推送
git remote add origin https://github.com/<用户名>/<仓库名>.git
git branch -M main
git push -u origin main
```

### 第一次 Pull Request(全程网页即可)

1. 在仓库里新建分支(网页上 branch 下拉 → New branch),例如 `feature/update-readme`
2. 在该分支上编辑文件并提交
3. 页面提示 `Compare & pull request`,点进去填写标题与说明
4. 点 `Create pull request`
5. 自己检查没问题后点 `Merge pull request`,再删除分支

### 开启 GitHub Pages(免费建站)

Settings → Pages → Source 选 `Deploy from a branch` → 选择分支与目录(`/root` 或 `/docs`)→ Save。
稍等片刻,访问 `https://<用户名>.github.io/<仓库名>/`。

## 6. 日常协作流程

```text
提 Issue 描述问题
   ↓
从 main 拉分支 feature/xxx
   ↓
本地开发、commit、push
   ↓
在 GitHub 开 PR,写清"改了什么、为什么改"
   ↓
评审通过 → Squash merge → 删除分支
   ↓
本地 git switch main && git pull
```

PR 描述建议包含:

- **改动内容**:一句话概述
- **背景/原因**:为什么需要这次改动
- **验证方式**:跑了什么测试、怎么手动验证
- **关联 Issue**:`Closes #12`(合并后自动关闭该 Issue)

评审礼仪:对事不对人;提出问题时说明理由;作者及时回复或标记 Resolved。

## 7. 进阶功能

### GitHub Actions(自动化)

在仓库根目录建 `.github/workflows/ci.yml`:

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: pip install -r requirements.txt
      - run: pytest -q
```

推送到 GitHub 后自动运行,结果会显示在 PR 的 Checks 里。

### Releases(版本发布)

```bash
git tag -a v1.0.0 -m "首个版本"
git push origin v1.0.0
```

再到仓库 Releases 页面写更新说明、上传构建产物。

### 其他常用功能

- **Projects**:新建看板,把 Issue 拖进 Todo / Doing / Done
- **Wiki**:写系列文档,首页可当目录
- **Gist**:分享单文件代码或笔记,不必建仓库
- **Codespaces**:浏览器里直接打开完整开发环境
- **Dependabot**:自动提 PR 升级依赖、修复已知漏洞

## 8. 用 GitHub 学习与找资源

- **Explore**:https://github.com/explore ,看每日趋势项目
- **Trending**:按语言筛选热门仓库
- **搜索语法**:

| 语法 | 示例 | 作用 |
| :--- | :--- | :--- |
| `language:` | `language:python` | 限定语言 |
| `stars:` | `stars:>1000` | 限定 Star 数 |
| `topic:` | `topic:algorithm` | 按主题标签 |
| `filename:` | `filename:README.md` | 按文件名 |
| `path:` | `path:src/` | 按路径 |
| `org:` | `org:microsoft` | 限定组织 |
| `is:issue` | `is:issue is:open label:bug` | 找 Issue |

- 搜 `awesome <主题>` 能找到大量精选资源列表
- 用 `good first issue` 标签找适合新手的开源任务

## 9. 安全与规范

- **绝不提交密钥**:API Key、`.env`、私钥一律写进 `.gitignore`;万一提交了,立刻到平台吊销并重新生成,再清理历史。
- **选许可证**:MIT(最宽松)、Apache-2.0(含专利授权)、GPL(衍生作品必须开源)。
- **课程作业放私有仓库**:避免被同学"参考",也避免学术诚信问题。
- **不要刷 Star / 刷关注**:违反社区规范,可能被封号。
- **尊重开源协议**:使用他人代码时保留 LICENSE 与版权声明。
- **谨慎运行陌生代码**:clone 别人的项目先看脚本内容,别直接 `sudo` 执行。

## 10. 常见问题 FAQ

### Q1:Git 和 GitHub 必须一起用吗?

不必须。Git 可以完全离线使用,也可以推到 GitLab、Gitee 等平台;GitHub 只是最常用的托管平台。

### Q2:`push` 被拒绝,提示 non-fast-forward?

远端有你本地没有的提交。先 `git pull --rebase origin main`,解决冲突后再 `git push`。

### Q3:每次打开 GitHub 都是亮色主题,手动切成暗色后下次又变回来?

这是**浏览器没有记住主题**的典型情况,按顺序排查:

1. **确认已经登录**。登录后主题保存在账号里,跨设备、跨会话都会保留;未登录时主题只存在当前浏览器的 Cookie/本地存储中,一旦被清理就恢复默认。
2. 打开 <https://github.com/settings/appearance>,在 **Theme** 里选择 `Dark`(暗色)。
   - 想让 GitHub 跟随系统:选 `Sync with system preferences`,并确保操作系统本身是深色模式。
   - 只选了 `Light` 或 `Sync with system` 且系统是浅色,就会一直是亮色。
3. 若设置后仍反复变回亮色,检查浏览器:
   - 是否在**无痕/隐私窗口**里使用(关闭即丢数据),改用普通窗口;
   - 是否开启了"关闭浏览器时清除 Cookie 和站点数据",或用了清理类扩展(CCleaner、Cookie AutoDelete 等),把 `github.com` 加入白名单;
   - 是否同时用了多个浏览器/多个浏览器用户配置,每个都需要单独设置;
   - 手机 App 是独立设置:GitHub App → Settings → Appearance;
   - 公司/学校网络的安全软件若重写 Cookie,也会导致偏好丢失。
4. 注意 `github.com`、`gist.github.com`、`docs.github.com` 的主题设置是**各自独立**的,需要分别设置。

> 最快的解决路径:登录状态下访问 `github.com/settings/appearance` 选 Dark,并关闭浏览器的自动清理站点数据功能。

### Q4:忘记密码或 2FA 设备丢失?

用注册邮箱走密码找回;2FA 丢失可使用保存的恢复码,或在 Settings 中重新配置。因此**开启 2FA 时务必把恢复码存到安全位置**。

### Q5:免费账号有哪些限制?

公共仓库免费且不限数量;私有仓库数量与协作人数有上限;Actions 每月有免费额度(公共仓库基本不限),超出后按用量计费。

### Q6:别人 Fork 我的仓库等于抄走吗?

Fork 会在对方账号下生成副本,这是开源协作的正常机制。只要你选了合适的许可证,别人使用需遵守协议;不想被复制就把仓库设为 Private。

### Q7:网页上能直接改代码吗?

可以。单个文件编辑、新建文件、删除文件都能在网页完成并提交,适合改文档和小修正;多文件重构仍建议本地开发。

## 11. 术语速查表

| 英文 | 中文 | 说明 |
| :--- | :--- | :--- |
| Repository | 仓库 | 项目代码与历史的集合 |
| Fork | 复刻 | 复制他人仓库到自己账号 |
| Star | 收藏/点赞 | 表示喜欢该项目 |
| Watch | 关注 | 订阅仓库通知 |
| Issue | 议题 | 记录 bug、需求、讨论 |
| Pull Request | 拉取请求 | 请求合并分支改动 |
| Merge | 合并 | 把改动并入目标分支 |
| Review | 评审 | 对代码提出意见 |
| Release | 发布 | 版本发布页 |
| Actions | 自动化 | CI/CD 流水线 |
| Pages | 静态站点 | 免费托管网站 |
| Gist | 代码片段 | 轻量分享 |
| Codespaces | 云开发环境 | 在线 VS Code |
| Copilot | 智能助手 | AI 代码补全 |

## 12. 建议的入门路线

| 阶段 | 做什么 | 目标 |
| :--- | :--- | :--- |
| 第 1 天 | 注册、开 2FA、完善头像与简介 | 账号可用且安全 |
| 第 2 天 | 建 Profile README 仓库 | 主页有内容 |
| 第 3 天 | 本地建仓库并 push 第一个项目 | 跑通完整链路 |
| 第 4 天 | 用分支 + PR 改一次自己的仓库 | 理解协作流程 |
| 第 5 天 | 开 Issue、用 Projects 管待办 | 学会任务管理 |
| 第 6 天 | 配 Actions 自动跑测试、开 Pages | 体验自动化 |
| 第 7 天 | 给感兴趣的项目提 Issue/PR | 参与开源 |

---

参考: [GitHub 官方文档](https://docs.github.com/zh) · [GitHub Skills(交互式课程)](https://skills.github.com/) · [Git 使用指南](GIT_GUIDE.md)
</content>
