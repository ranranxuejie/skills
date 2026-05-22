# Skills 合集

面向学术研究工作流的 Claude Code 技能包。

## 安装方式

将本目录下的技能通过符号链接安装到 `~/.cc-switch/skills/`：

```bash
# 批量安装
cd /path/to/skills
for skill in openalex-paper-search paper_guidance_review; do
  ln -s "$(pwd)/$skill" ~/.cc-switch/skills/$skill
done
```

或单独安装：

```bash
ln -s /path/to/skills/openalex-paper-search ~/.cc-switch/skills/openalex-paper-search
ln -s /path/to/skills/paper_guidance_review ~/.cc-switch/skills/paper_guidance_review
```

安装后重启 Claude Code 即可生效。

## 技能说明

### openalex-paper-search

基于 OpenAlex API 的学术论文搜索，覆盖 2.4 亿+ 篇学术文献。免费使用，无需 API Key（可选提供以获取更高请求速率）。

**适用场景：** 搜索论文、文献综述、引用分析、查看作者发表记录、查找高影响力论文。

```bash
# 示例：查找高引用的 Transformer 论文
curl -s "https://api.openalex.org/works?search=transformer+architecture&sort=cited_by_count:desc&filter=cited_by_count:>100&per_page=10&mailto=agent@kortix.ai"
```

### paper_guidance_review

学术论文写作指导与审稿检查。支持所有研究类型（临床/公共卫生、实验科学、计算机/AI、社会科学、工程等）。

**两种模式：**
- **写作指导** — 按照顶刊标准，辅助撰写论文各章节（摘要、引言、方法、结果、讨论、结论）。
- **审稿检查** — 对照质量清单，审核现有稿件并给出改进建议。

**使用方式：** 直接告诉 Claude 你想写或审哪个章节，例如"帮我写引言"或"检查一下讨论部分"。
