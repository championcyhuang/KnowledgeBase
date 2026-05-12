---
title: "PPT-Master"
type: entity
tags: [AI, PPT, 自动化工具, Claude-Code]
sources: [原始资料已处理]
last_updated: 2026-05-11
---

# PPT-Master

一款 AI 驱动的 PPT 自动生成系统，运行在 Claude Code 环境中，通过多角色协作流水线将源文档（PDF、DOCX、网页等）转换为原生可编辑的 PPTX 文件。

---

## 导航地图

点击即可跳转到对应区块：

- 🚀 [[#前置条件]] — 环境安装与依赖
- ⚙️ [[#核心架构]] — 流水线阶段与角色职责
- 📖 [[#使用方式]] — 如何用自然语言启动
- 🎨 [[#画布格式与内置模板]] — 尺寸与 18 套模板
- 📋 [[#常用脚本速查]] — 转换、管理、后处理命令
- 🔧 [[#严格约束]] — 不可违反的规则
- 🐛 [[#故障排查]] — 问题检查优先级

---

## 前置条件

```bash
pip install -r requirements.txt
```

- Python 3.10+，依赖包括 `python-pptx`、`PyMuPDF`、`Pillow`、`flask`、`openai` 等 60+ 个包
- **Windows 用户**：若遇到 `python: command not found`，需先安装 Python（`winget install Python.Python.3.12`），然后关闭「Windows 设置 → 应用 → 高级应用设置 → 应用执行别名」中的 Python 占位存根

---

## 核心架构

PPT-Master 采用 **分阶段流水线架构**，由多个 AI 角色按固定顺序协作：

| 阶段 | 角色 | 职责 |
| --- | --- | --- |
| 1 | 转换器 | 将非 Markdown 源文件转为 Markdown |
| 2 | 项目管理器 | 初始化项目目录结构 |
| 3 | 模板加载器 | 如有指定模板则加载 |
| 4 | 策略师 | 提出 8 项设计建议，等待用户确认 ⛔ |
| 5 | 图片获取器 | 条件性执行 AI 图片生成或网络搜图 |
| 6 | 执行器 | 逐页生成 SVG 和演讲备注 |
| 7 | 后处理器 | 三个脚本依次执行，导出 PPTX |

```
[源文档] → 格式转换(MD) → 项目初始化 → [模板加载] → 策略师设计 ⛔ → [图片获取] → 执行器生成SVG → 后处理三步骤 → [PPTX输出]
```

- **⛔** 标记唯一的人机确认点

---

## 使用方式

在 Claude Code 对话中直接用自然语言描述需求即可，例如：

- "帮我把这个 PDF 做成 16:9 的 PPT"
- "根据这个网页内容生成演示文稿"
- "我要做一个关于 AI 发展的 10 页 PPT"

AI 会自动按流水线逐阶段执行，无需手动调用脚本。

---

## 支持的源材料格式

**无需手动放置文件** — 把源文件/链接直接在对话中发给 Claude Code 即可，AI 会自动转换并归入项目目录。

| 格式 | 说明 |
|------|------|
| PDF | 自动提取文本、图片、表格 |
| DOCX / Word | 原生 Python 转换 |
| PPTX | 提取已有 PPT 内容 |
| Excel (.xlsx/.xlsm) | 保留工作表结构 |
| 网页 URL | 支持微信公众号等高风险网站 |
| EPUB / HTML | 原生 Python 转换 |
| Markdown / 纯文本 | 直接使用 |
| 只有话题没有材料 | 自动触发网络调研工作流 `topic-research` |

---

## 策略师阶段：8 项设计建议（唯一确认点）

这是整个流水线中**唯一需要用户确认的步骤**。AI 会提出 8 项设计建议：

1. 画布格式（16:9 / 4:3 / 小红书 / 故事等）
2. 页数范围
3. 目标受众
4. 风格目标
5. 配色方案
6. 图标使用策略
7. 字体方案
8. 图片使用策略

确认后所有后续步骤自动完成，无需再干预。

---

## 画布格式与内置模板

### 画布格式

| 格式 | 尺寸 | 说明 |
|------|------|------|
| `ppt169` | 1920×1080 | 16:9 宽屏（默认） |
| `ppt43` | 1920×1440 | 4:3 传统 |
| `xhs` | 1080×1440 | 小红书竖版 |
| `story` | 1080×1920 | 故事/手机全屏 |

完整格式列表见 `skills/ppt-master/references/canvas-formats.md`。

### 内置模板（18 套）

模板位于 `skills/ppt-master/templates/layouts/`，使用时需给出完整路径：

| 分类 | 模板 |
|------|------|
| 学术 | `academic_defense`、`重庆大学`、`medical_university` |
| 企业 | `招商银行`、`中汽研_商务`、`中汽研_常规`、`中汽研_现代`、`中国电建_常规`、`中国电建_现代` |
| 政府 | `government_blue`、`government_red` |
| 风格 | `google_style`、`anthropic`、`pixel_retro`、`psychology_attachment` |
| 科技 | `ai_ops`、`china_telecom_template` |

例如：`skills/ppt-master/templates/layouts/招商银行/`

> [!info] 风格描述 vs 模板路径
> 风格描述（如"麦肯锡风格"）不会触发模板加载，而是作为策略师的设计参考。只有明确给出模板目录路径时才会使用模板。

---

## 常用脚本速查

### 源文件转换

```bash
python skills/ppt-master/scripts/source_to_md/pdf_to_md.py <PDF文件>
python skills/ppt-master/scripts/source_to_md/doc_to_md.py <Word/EPUB/HTML文件>
python skills/ppt-master/scripts/source_to_md/excel_to_md.py <Excel文件>
python skills/ppt-master/scripts/source_to_md/ppt_to_md.py <PPTX文件>
python skills/ppt-master/scripts/source_to_md/web_to_md.py <URL>
```

### 项目管理

```bash
python skills/ppt-master/scripts/project_manager.py init <项目名> --format ppt169
python skills/ppt-master/scripts/project_manager.py import-sources <项目路径> <源文件> --move
python skills/ppt-master/scripts/project_manager.py validate <项目路径>
```

### AI 图片生成

```bash
python skills/ppt-master/scripts/image_gen.py "提示词" --aspect_ratio 16:9 --image_size 1K -o <项目路径>/images
```

### 后处理三步骤（依次执行，不可合并，不可跳过）

```bash
python skills/ppt-master/scripts/total_md_split.py <项目路径>
python skills/ppt-master/scripts/finalize_svg.py <项目路径>
python skills/ppt-master/scripts/svg_to_pptx.py <项目路径>
```

> [!danger] 三步不可合并
> `total_md_split.py` → `finalize_svg.py` → `svg_to_pptx.py` 必须依次单独运行，因为每步的输出是下一步的输入，合并执行会导致中间态丢失。

### 质量检查与预览

```bash
# SVG 质量检查
python skills/ppt-master/scripts/svg_quality_checker.py <项目路径>

# 本地预览（启动后在浏览器中查看）
python -m http.server -d <项目路径>/svg_final 8000
```

### 导出动画选项

```bash
python skills/ppt-master/scripts/svg_to_pptx.py <项目路径> \
  -t fade \                          # 页面切换效果
  -a mixed \                         # 元素入场动画
  --animation-trigger after-previous \  # 动画触发方式
  --auto-advance 10                  # 自动播放间隔（秒）
```

最终输出：
- `exports/<项目名>_<时间戳>.pptx` — 原生可编辑 PPTX（含动画）
- `backup/` — SVG 预览版及源文件备份

---

## 严格约束

1. **串行执行** — 流水线步骤必须按序执行，不可跳跃
2. **Step 4 是唯一阻断点** — 策略师确认后，剩余步骤自动完成，中间不再打断用户
3. **禁止跨阶段打包** — 不能在策略师阶段就开始写 SVG
4. **执行器不可委托子代理** — SVG 生成必须在主代理中逐页完成
5. **每页前必须重读 spec_lock.md** — 确保颜色/字体/图标不偏离设计规格，防止设计漂移
6. **后处理三个命令不可合并执行** — `total_md_split.py` → `finalize_svg.py` → `svg_to_pptx.py` 必须依次单独运行

---

## 故障排查

遇到问题时按以下优先级检查：

1. `docs/faq.md` — 常见问题与解决方案
2. `skills/ppt-master/scripts/docs/troubleshooting.md` — 脚本级故障排查
3. SVG 质量检查：`python skills/ppt-master/scripts/svg_quality_checker.py <项目路径>`

---

## 关联连接

- [[摘要-ppt-master-guide]] — 原始使用指南摘要
- [[AI-PPT-Pipeline]] — 多阶段 AI 流水线架构概念
