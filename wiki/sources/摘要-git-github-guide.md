---
title: "摘要-git-github-guide"
type: source
tags: [Git, GitHub, 版本控制, 实操指南]
sources: [原始资料已处理]
last_updated: 2026-05-11
---

## 核心摘要

这是一篇面向零基础用户的 Git & GitHub 全流程实操指南，从安装配置 Git 身份和凭据管理器开始，涵盖在 GitHub 创建仓库、将本地项目初始化并推送到远程、从远程克隆仓库、删除仓库中文件等完整生命周期操作。同时附带了日常高频命令速查表和常见问题排查对照表，适合作为首次接触 Git 的新手速查手册。

---

## 关键要点

- Git 身份配置（`user.name`、`user.email`）是提交代码前的必要步骤
- GitHub 自 2021 年起不支持密码登录，需使用 Personal Access Token 认证
- `credential.helper manager` 可避免每次 push 都输入凭据
- `git push -u origin main` 绑定上下游后，后续只需 `git push` / `git pull`
- `.gitignore` 用于排除 `node_modules`、`.env`、`*.log` 等不应纳入版本管理的文件

---

## 实操案例：imglab 项目上传

以下是将本地 `imglab` 图像处理项目上传至 GitHub 的完整记录，可作为从零推送的参考。

**背景**：`imglab` 原先是父仓库下的子目录，没有独立的 `.git` 目录，因此需要重新初始化并关联远程。

**操作步骤**：

```bash
# 1. 进入项目目录并初始化 Git
cd F:\AI\imglab
git init

# 2. 添加 .gitignore（排除依赖、缓存、输出目录）
# 内容：.venv/ __pycache__/ *.pyc .ipynb_checkpoints/ outputs/

# 3. 关联远程仓库
git remote add origin git@github.com:championcyhuang/ImageProcessingSimulation.git

# 4. 提交并推送
git add .
git commit -m "Initial commit"
git push -u origin master
```

**提交文件清单**：配置（`.gitignore`、`requirements.txt`）、数据、笔记、源码（12 个 `.py` 文件）、脚本（7 个 `run_*.py`）、Web 演示目录，共 25 个文件。

**结果**：推送成功，分支 `master` 已关联远程 `origin/master`。

---

## 关联连接

- [[Git]] — Git 与 GitHub 完整操作指南（含导航地图、速查表、故障排查）
- [[GitHub]] — GitHub 平台薄入口
- [[VersionControl]] — 版本控制概念
