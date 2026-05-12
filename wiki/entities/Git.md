---
title: "Git"
type: entity
tags: [版本控制, 工具, 命令行]
sources: [原始资料已处理]
last_updated: 2026-05-11
---

# Git

Git 是一个分布式版本控制系统，用于跟踪文件的变更历史，支持多人协作开发。由 Linus Torvalds 创建。

---

## 导航地图

点击即可跳转到对应区块：

- 🚀 [[#快速开始：安装与工作流|快速开始]] — 首次配置、从零推送到 GitHub
- ⚙️ [[#核心机制]] — 理解 Git 三区域原理
- 🌐 [[#GitHub 平台操作]] — 创建仓库、Token、SSH 故障
- 🗑️ [[#删除操作]] — 删文件、删仓库
- 📋 [[#日常高频命令速查]] — 每天用的命令
- 🔧 [[#常见问题排查]] — 报错时对照

---

## 快速开始：安装与工作流

### 安装与身份配置

首次使用 Git 前需要配置身份信息，这些信息会出现在每次提交记录中：

```bash
# 设置全局用户名（提交代码时会显示这个名字）
git config --global user.name "你的名字"

# 设置全局邮箱（提交代码时会显示这个邮箱）
git config --global user.email "你的邮箱"

# 设置凭据管理器（让 Git 记住你的 GitHub 登录状态，避免每次都要输密码）
git config --global credential.helper manager
```

> [!warning] 关于凭据与 Token
> 没有 `credential.helper` 的话，每次 `git push` 都会卡在身份认证环节。Windows 上 `manager` 会调用系统的凭据存储，弹出登录窗口让你授权 GitHub。
>
> **GitHub 自 2021 年起不支持密码登录 Git 操作**。如果弹出登录窗口，密码栏请填写 [Personal Access Token](https://github.com/settings/tokens)（创建时勾选 `repo` 权限）。详细步骤见下方 [Token 认证](#token-认证详细步骤)。

---

### 完整工作流程：从零到推送

以下是将本地项目上传到 GitHub 的完整流程，每一步都有说明：

```bash
# 1. 进入项目目录（以 E:/AI 为例）
cd E:/AI

# 2. 初始化 Git 仓库
#    在当前目录下创建一个隐藏的 .git 文件夹，用来跟踪所有文件的变更历史
git init

# 3. 创建 .gitignore 文件
#    告诉 Git 哪些文件/文件夹不需要纳入版本管理
#    node_modules：依赖包，太大了，npm install 就能重新下载
#    .env：环境变量文件，通常含密码等敏感信息
#    *.log：日志文件，无保存价值
echo "node_modules/
.env
*.log" > .gitignore

# 4. 将所有文件加入暂存区
#    . 代表当前目录下的所有文件和子目录
git add .

# 5. 提交到本地仓库
#    -m 后面的内容是提交说明，描述这次提交做了什么
git commit -m "首次提交：初始化项目"

# 6. 关联远程仓库
#    origin 是远程仓库的别名（约定俗成的名字，可以改成别的但没必要）
#    后面跟着的是你在 GitHub 上创建的仓库地址
git remote add origin https://github.com/你的用户名/仓库名.git

# 7. 强制重命名当前分支为 main
#    -M 是大写强制模式，确保本地分支名为 main
#    GitHub 默认主分支叫 main，老版本 Git 叫 master，这条命令统一两者
git branch -M main

# 8. 推送到 GitHub 并绑定上下游关系
#    -u (--set-upstream)：绑定本地 main 和远程 origin/main 的上下游关系
#        绑定后以后只需要 git push / git pull，不用再指定 origin main
git push -u origin main
```

> [!tip] 绑定过上下游后
> 执行过 `git push -u origin main` 后，后续只需 `git push` / `git pull`，不用再写 `origin main`。

### 从远程克隆仓库

把 GitHub 上的仓库下载到本地（换电脑或拉取团队项目时使用）：

```bash
# clone = 克隆，把远程仓库的完整内容（代码 + 全部提交历史）下载到本地
# 会在当前目录下自动创建一个以仓库名命名的文件夹
git clone https://github.com/你的用户名/仓库名.git
```

---

## 核心机制

Git 的工作流分为三个区域：

```
工作目录（Working Directory）  →  暂存区（Staging Area）  →  本地仓库（Local Repository）  →  远程仓库（Remote）
       git add                       git commit                        git push
```

- **工作目录**：你实际编辑文件的地方
- **暂存区**：`git add` 将修改加入暂存区，标记"这些是我准备提交的东西"，还没真正记录到历史里
- **本地仓库**：`git commit` 将暂存内容正式写入 Git 历史，每次提交都有唯一 ID
- **远程仓库**：`git push` 将本地提交上传到 GitHub 等远程服务器

---

## GitHub 平台操作

以下是与 GitHub 平台交互的专项操作，补充本地 Git 命令之外的平台侧流程。

### 创建仓库（网页端）

1. 打开 [github.com](https://github.com) 并登录
2. 点击右上角 **+** → **New repository**
3. 填写仓库名（Repository name）
4. 选择 Public（公开）或 Private（私有）
5. **不要勾选** "Add a README file"（如果本地已有代码要上传）
6. 点击 **Create repository**
7. 记下仓库地址，格式为：`https://github.com/你的用户名/仓库名.git`

> [!info] 仓库地址用途
> 该地址用于 `git remote add` 关联和 `git clone` 克隆。

### Token 认证详细步骤

**GitHub 自 2021 年起不支持密码登录 Git 操作**，必须使用 Personal Access Token 代替密码：

1. 访问 [github.com/settings/tokens](https://github.com/settings/tokens)
2. 点击 **Generate new token**
3. 勾选 `repo` 权限（完整的仓库读写权限）
4. 生成后将 Token 复制保存（只显示一次）
5. 在 Git 弹出认证窗口时，密码栏粘贴 Token

如果配置了 `credential.helper manager`，认证成功后会记住凭据，后续无需重复输入。

### SSH 连接故障排查

使用 SSH 协议 (`git@github.com:...`) 推送时可能遇到以下问题：

> [!warning] Host key verification failed
> ```
> Host key verification failed.
> fatal: Could not read from remote repository.
> ```
> **原因**：本地 `~/.ssh/known_hosts` 中缺少 GitHub 的主机密钥。  
> **解决**：
> ```bash
> ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
> ```

> [!danger] Permission denied (publickey)
> ```
> git@github.com: Permission denied (publickey).
> ```
> **原因**：本地尚未生成 SSH 密钥对，或公钥未添加到 GitHub 账户。  
> **解决**：
> ```bash
> # 1. 生成密钥对
> ssh-keygen -t rsa -b 4096 -C "你的邮箱"
>
> # 2. 复制公钥内容
> cat ~/.ssh/id_rsa.pub
>
> # 3. 进入 GitHub Settings → SSH and GPG keys → New SSH key → 粘贴公钥
> # 4. 重新执行推送命令
> ```

---

## 删除操作

### 删除仓库中的文件

**方式一：在 GitHub 网页上操作**

1. 进入仓库，点进要删除的文件
2. 点击右上角 `...` → **Delete file**
3. 填写 commit message → 点击 **Commit changes**
4. 删除文件夹需要进去逐一删文件，文件删光后文件夹自动消失

**方式二：在本地操作（推荐，更高效）**

```bash
# rm：删除文件
# -r：递归删除，用于删除文件夹（recursive）
# 这个命令同时从磁盘和 Git 暂存区中删除文件
git rm -r 要删除的文件夹名

# 提交这次删除操作
git commit -m "删除xxx"

# 推送到 GitHub
git push
```

### 删除整个仓库

进入仓库 → Settings → 拉到页面最底部 → **Delete repository** → 输入仓库名确认。

> [!danger] 不可逆操作
> 删除仓库是不可逆的，确认前确保已备份重要内容。

---

## 日常高频命令速查

| 场景 | 命令 |
|------|------|
| 查看哪些文件改动了 | `git status` |
| 查看具体改了什么 | `git diff` |
| 添加所有改动到暂存区 | `git add .` |
| 提交 | `git commit -m "本次改动的说明"` |
| 推送上传 | `git push` |
| 拉取最新代码 | `git pull` |
| 查看提交历史 | `git log --oneline` |

---

## 常见问题排查

| 症状 | 原因 | 解决办法 |
|------|------|----------|
| `git push` 卡住不动 | 没有配置凭据管理器 | `git config --global credential.helper manager` |
| `fatal: couldn't find remote ref main` | 远程仓库是空的，没有分支可以拉 | 不执行 `pull`，直接 `push` 即可 |
| `fatal: not a git repository` | 当前目录还没初始化 Git | 先执行 `git init` |
| GitHub 要求输密码 | GitHub 不支持密码登录了 | 使用 Personal Access Token 代替密码 |

---

## 关联连接

- [[GitHub]] — GitHub 平台薄入口（详细操作见本页）
- [[VersionControl]] — 版本控制概念
- [[摘要-git-github-guide]] — 原始资料摘要
