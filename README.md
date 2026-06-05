[README.md](https://github.com/user-attachments/files/28623532/README.md)
# paper-zotero-survey

`paper-zotero-survey` 是一个 Codex Skill，用于自动完成研究领域论文检索、Zotero 归档和结构化论文解读。

它会根据用户输入的研究领域，检索 5 篇较新的高价值论文，要求论文有 PDF、有开源代码，并尽量具备可复现条件；随后将论文整理到 Zotero 当天日期命名的新集合中，并为每篇论文生成 Markdown 解读文件。

## 功能概述

- 如果用户没有明确研究领域，先询问研究领域、关键词或任务方向。
- 检索 5 篇最新、相关、优先来自顶刊顶会的论文。
- 每篇入选论文必须尽量满足：
  - 有可下载 PDF
  - 有开源代码链接
  - 代码仓库真实可访问
  - 具备 README、环境配置、训练或评估脚本等复现线索
- 在 Zotero 中创建一个以当天日期命名的新集合。
- Zotero 集合中只保留论文 PDF 和对应 Markdown 解读文件。
- 为每篇论文生成结构化解读文件。
- 最后生成 `00_论文优先级总览.md`，按照阅读价值和复现价值排序。

## Skill 位置

```text
skills/paper-zotero-survey/SKILL.md
```

## 触发示例

可以这样向 Codex 提问：

```text
帮我找 5 篇组合图像检索方向最新可复现论文，并整理到 Zotero
```

```text
帮我调研细粒度跨模态检索的顶会开源论文
```

```text
帮我自动下载论文 PDF，生成解读，并放进 Zotero
```

## 输出内容

每篇论文都会生成一个 Markdown 解读文件。文件开头必须包含：

- 论文标题
- 作者
- 发表年份
- 会议或期刊
- 论文 PDF 链接
- 开源代码链接
- 代码可复现性判断
- 推荐阅读优先级
- 推荐复现优先级

每篇解读文件还会包含：

- Backbone、Framework、Module 的严格区分
- 模型结构拆解
- 实验基线与公平性分析
- 调参和刷分风险判断
- 主实验、消融实验和性能提升分析
- 主要图表的大白话讲解
- 论文局限性与可借鉴价值
- 投稿风险评估
- 最终可用性评级

批量总览文件为：

```text
00_论文优先级总览.md
```

## Zotero 归档规则

Skill 会创建一个以当天日期命名的 Zotero 新集合：

```text
YYYY-MM-DD
```

例如：

```text
2026-06-05
```

如果当天已经存在同名集合，则追加领域关键词或序号：

```text
2026-06-05_CIR
2026-06-05_CIR_02
```

集合中每篇论文只保留 PDF 和对应解读文件，命名格式为：

```text
01_Author_Year_Short_Title.pdf
01_Author_Year_Short_Title_解读.md
```

论文 `02` 到 `05` 使用相同格式。

## 筛选标准

默认优先选择：

- 近两年论文
- CVPR、ICCV、ECCV、NeurIPS、ICLR、ICML、ACM MM、AAAI、IJCAI 等顶会论文
- TPAMI、IJCV、TIP、TMM、TCSVT、PR 等高质量期刊论文
- 有官方 GitHub 或 Papers with Code 链接的论文
- 有明确复现说明、训练脚本、评估脚本或预训练权重的论文

如果找不到 5 篇完全满足条件的论文，Skill 会按规则逐步放宽条件，但不会为了凑数加入弱相关论文。

## 安全说明

- 不要把论文 PDF 上传到 GitHub。
- 不要上传 Zotero 数据库或 Zotero 本地存储目录。
- 不要上传 GitHub token、密码或任何认证信息。
- 不要编造论文贡献、实验结果或图表解释。
- 如果 PDF 下载、代码链接验证或 Zotero 写入失败，必须明确说明失败原因。
- 命令行 `git push` 不能使用 GitHub 登录密码，需要使用 Personal Access Token 或 SSH key。

## 仓库内容

本仓库只应跟踪 Skill 文件和轻量项目说明：

```text
.gitignore
README.md
skills/paper-zotero-survey/SKILL.md
```
