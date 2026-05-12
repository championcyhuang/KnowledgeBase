---
title: "操作日志"
type: log
last_updated: 2026-05-11
---

# 操作日志

## [2026-05-11] sync | 全局同步：ingest skill 新增 Obsidian 排版规范，全部 wiki 页面统一优化
- **变更**: 更新 ingest.md（新增 Obsidian 排版规范章节：导航地图、Callout、分隔线、Entity 合并/Source 归并规则）; 优化 PPT-Master（导航地图、Callout、分隔线）; 优化 VersionControl（导航地图、Callout、概念表格）; 优化 AI-PPT-Pipeline（导航地图、Callout、分隔线）; 优化 摘要-git-github-guide（导航地图、分隔线）; 优化 摘要-ppt-master-guide（导航地图、分隔线、指向 PPT-Master 的 info Callout）
- **冲突**: 无

## [2026-05-11] lint | 优化 Git.md 排版：新增 Obsidian 导航地图与 Callout
- **变更**: Git.md 新增顶部导航地图（含锚点链接）; 关键提示改为 [!warning]/[!tip]/[!info]/[!danger] Callout; 用 --- 分隔大区块; 删除操作独立成章
- **冲突**: 无

## [2026-05-11] lint | Source 归并：摘要-github-upload-record 并入 GitHub 实体与 git-github-guide
- **变更**: 将 摘要-github-upload-record 的 SSH 故障排查填充至 GitHub 实体页面; 将上传案例并入 摘要-git-github-guide; 删除 摘要-github-upload-record; 更新 index.md
- **冲突**: 无

## [2026-05-11] ingest | 摄入 imglab 项目 GitHub 上传记录
- **变更**: 新增 摘要-github-upload-record; 更新 index.md
- **冲突**: 无

## [2026-05-09] ingest | 摄入 Git & GitHub 实操指南
- **变更**: 新增 摘要-git-github-guide, Git, GitHub, VersionControl; 更新 index.md
- **冲突**: 无

## [2026-05-10] sync | 修改 ingest skill：强化归档为强制执行步骤，新增完成前自检清单
- **变更**: 更新 .claude/skills/ingest.md；归档 PPT-MASTER 至 raw/09-archive/

## [2026-05-10] ingest | 摄入 PPT-Master 使用指南
- **变更**: 新增 摘要-ppt-master-guide, PPT-Master, AI-PPT-Pipeline; 更新 index.md
- **冲突**: 无

## [2026-05-10] lint | 扩充实体页面至 raw 级别详细程度
- **变更**: 大幅扩充 Git（新增配置步骤、完整工作流、删除文件操作、故障排查表）、GitHub（新增仓库创建流程、Token认证说明、删除操作三种方式）、PPT-Master（新增前置条件、脚本速查、画布格式、内置模板全列表、动画导出、故障排查入口）
- **冲突**: 无

## [2026-05-10] sync | 更新 ingest skill 和 CLAUDE.md：强制实体页面详细程度标准
- **变更**: 更新 .claude/skills/ingest.md（新增详细程度标准章节、扩充实体/概念页面模板、新增正反例对比、完成前自检新增详细度检查项）；更新 CLAUDE.md（新增第5条实体页面详细程度标准、第4条新增 raw 直达链接要求）
- **冲突**: 无

## [2026-05-10] sync | 架构变更：raw 从不可变层改为消耗型收件箱，ingest 后删除源文件
- **变更**: 更新 CLAUDE.md（raw/ 从"绝对只读不可变层"改为"消耗型收件箱，处理后删除"）；更新 .claude/skills/ingest.md（步骤 6 从"归档到 09-archive"改为"删除源文件"，移除所有 archive 相关逻辑）；删除 raw/09-archive/ 下 2 个冗余归档文件；清理 7 个 wiki 页面中的死链
- **冲突**: 无

## [2026-05-10] sync | ingest 新增合并/填充决策步骤
- **变更**: 更新 .claude/skills/ingest.md；在步骤 2 和步骤 3 之间新增步骤 2.5"合并决策"（使用 AskUserQuestion 询问用户合并到已有摘要或新建）；步骤 3 拆分为模式 A（新建）和模式 B（合并填充）；步骤 4 新增合并模式下重新整理关联实体/概念的要求；自检清单新增合并决策和级联刷新检查项
- **冲突**: 无

## [2026-05-10] sync | 图谱优化：log.md 移出 wiki/，清除 wikilink 死链
- **变更**: log.md 从 wiki/ 移至项目根目录；log.md 内 wikilink 全部转为纯文本（避免在 Obsidian 图谱中连接无关集群）；修复 PPT-Master.md 中 ClaudeCode 死链；更新 CLAUDE.md、ingest.md、query.md、lint.md 中 log.md 路径引用
- **冲突**: 无

## [2026-05-09] init | 初始化 LLM Wiki 知识库
- **变更**: 创建目录结构、CLAUDE.md、index.md、log.md、ingest/query/lint 三个 skill
- **冲突**: 无
