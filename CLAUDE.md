 # 语言设定与核心角色 (Global Rules)
- **语言指令**：无论输入何种语言，你必须始终使用**简体中文**进行思考、回复和知识库的编写。
- **角色定义**：你正在维护一个 **LLM Wiki**（根据 [Karpathy 的规范](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)），你的任务是将碎片化的信息编译成结构化、高度相互链接的 Obsidian 知识库。

# 核心目录与权限边界 (Architecture)
你必须严格遵守以下文件操作权限：

- `/raw/` (消耗型收件箱 - Consumable Inbox)：
  - 存放待处理的原始素材（网页剪藏、文章、文案等）。
  - **ingest 处理完成后立即删除源文件**，不保留副本。Wiki entities 已达到并超越 raw 的详细程度，raw 仅作为编译原料，无保留价值。
  - 子目录：`01-articles/`、`02-papers/`、`03-transcripts/`、`04-meeting_notes/`、`09-archive/`（archive 目录保留但不再使用，旧文件待清理）。
- `/assets/` (媒体资产层)：
  - 存放图片、PDF和媒体。引用时使用 Obsidian 标准语法 `![[文件名称.png]]`。
- `/wiki/` (编译输出层 - You Own This)：
  - 这是你的专属工作区。你需要在此处创建、更新、提炼知识并解决矛盾。

# Wiki 核心文件契约 (The Wiki Schema)
当你在 `/wiki/` 中工作时（尤其是执行写入操作后），必须维护以下基石：

1. **`wiki/index.md` (总目录)**：
   每次向 wiki 新增知识页后，必须同步更新此文件，将其按分类加入目录中。
   格式要求： `[[页面名称]]` — 一句话描述。
    - Entities/Concepts: 使用 TitleCase 命名。
    - Sources/Syntheses: 使用 kebab-case 命名。
    范例：
    ```markdown
    # Wiki Index

    ## Sources
    - [[摘要-source-slug]] — 该资料的核心主旨摘要。

    ## Entities
    - [[EntityName]] — 该实体的身份定义或核心功能。

    ## Concepts
    - [[ConceptName]] — 该概念或框架的核心定义。

    ## Syntheses
    - [[synthesis-slug]] — 该页面回答的复杂问题。
    ```

2. **`log.md` (操作日志 / 项目根目录)**：
    只能追加写入（Append-only）。每次操作后记录：`## [YYYY-MM-DD] <动作> | <操作简述>`。
    操作类型： ingest, query, lint, sync
    **日志内容使用纯文本（禁止 wikilink）**，避免在 Obsidian 知识图谱中制造无关连接。
    范例：
    ```markdown
    ## [2026-04-11] ingest | 引入项目 Claude Code 核心概念
    - **变更**: 新增 ClaudeCode, 摘要-claude-code-docs; 更新 index.md
    - **冲突**: 无

    ## [2026-04-11] query | 解析 Karpathy LLM-Wiki 理念
    - **输出**: 已保存至 分析-karpathy-wiki-philosophy

    ## [2026-04-11] lint | 周度健康检查
    - **结果**: 修复 2 处死链，发现 1 个孤儿页面 UnlinkedPage
    ```

3. **内容分类**：
   - `/wiki/concepts/`：存放概念、框架、方法论（如 `Agent_Skill.md`）。
   - `/wiki/entities/`：存放人物、公司、工具、产品（如 `Claude_Code.md`）。
   - `/wiki/sources/`：存放从 `raw/` 提炼出的原始素材摘要。
   - `/wiki/syntheses/`：存放针对复杂提问生成的深度研究报告。

4. **强制双向链接**：
   每一个 wiki 页面必须包含 `## 关联连接` 区域，使用 Obsidian 双链 `[[页面名称]]` 链接到其他相关概念。绝不能产生孤岛页面。

5. **实体页面详细程度标准**：
   实体页面不可写成简单摘要，必须以 raw 文件的细节密度为基准，达到"脱离 raw 也能独立操作"的程度：
   - **命令必须带注释**：每条命令上方或行尾用 `#` 注释说明 WHY，禁止只贴裸命令
   - **踩坑提示必须保留**：raw 里的警告块、注意事项、版本兼容说明必须完整保留
   - **故障排查表必须完整**：raw 里的"症状→原因→解法"对照表不可删减
   - **操作流程必须完整**：从安装配置到最终产出的全流程步骤不可跳步
   - **正例参考**：wiki/entities/Git.md、wiki/entities/PPT-Master.md

6. **矛盾处理原则**：
   如果新摄入的知识与旧知识冲突，不要静默覆盖。在页面中新建 `## 知识冲突` 区块，将两种说法都保留并做对比。

# 工作流指令说明 (Workflows / Skills)
当被要求执行以下操作时，请遵循核心逻辑（未来可能由专用 Agent Skills 接管）：

- `/ingest <路径>`：读取指定的 `raw/` 文件，将其编译为详细的 wiki 页面（entities 达到可独立操作的详细程度）。处理完成后删除源文件。必须更新 index 和 log。
- `/query <问题>`：通过读取 `wiki/index.md` 寻找相关文件，进行深度阅读后综合回答，并在回答中必须使用 `[[wikilink]]` 标注引用来源。
- `/lint`：全局扫描 `wiki/` 目录，找出孤岛页面（没有双链）、死链（链接不存在的页面）以及存在逻辑冲突的地方，并向我报告。

# 页面 Frontmatter (YAML) 规范
所有生成的 wiki 页面必须包含以下 YAML 头部：
```yaml
---
title: "页面标题"
type: concept | entity | source | synthesis
tags: [知识标签]
sources: [关联的raw文件相对路径]
last_updated: YYYY-MM-DD
---
```
