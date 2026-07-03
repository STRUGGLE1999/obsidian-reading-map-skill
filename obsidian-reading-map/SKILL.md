---
name: obsidian-reading-map
description: Convert Obsidian or Apple Books exported book-highlight Markdown into a study-review mind map for XMind. Use when the user provides reading notes, highlights, annotations, or an Obsidian markdown file and asks to generate an XMind-importable OPML mind map, optionally also creating an online/native XMind version through the XMind MCP server.
---

# Obsidian Reading Map

## Goal

Turn one book's Obsidian/Apple Books highlight Markdown into a learning-review mind map that can be imported into XMind.

The required final artifact is an XMind-importable `.opml` file. If the XMind MCP tools are available and the user wants or accepts it, also create an online/native XMind mind map from the same final structure.

Do not present or bundle a reusable generation script as the user-facing output. If code is needed, use a temporary or task-local Python standard-library helper, then report only the OPML path, optional XMind link, and verification result.

## Workflow

1. Read the full source Markdown and extract metadata, highlights, notes, repeated marks, and any explicit user comments.
2. Identify the book type before structuring:
   - Product/business/management/technical books: emphasize problem, concepts, frameworks, workflow, roles, tradeoffs, artifacts, and action transfer.
   - Communication/psychology/self-management books: emphasize core situation, principles, method sequence, signals, pitfalls, and personal practice.
   - If uncertain, infer from the book title, author, chapter/highlight themes, and user notes.
3. Deduplicate semantically similar highlights before outlining.
4. Group and rebuild the logic. Do not flatten by chapter order unless the book itself is primarily chronological.
5. Preserve user-highlighted and user-noted content first. Use the book's well-known original framework only to connect the user's highlighted ideas into a complete loop.
6. Exclude ideas from other books, outside articles, or general professional knowledge. If an application idea is useful but not an original book claim, place it only under an action/practice branch and label it as based on the book's method or the user's notes.
7. Generate OPML with Python standard library XML APIs. Avoid hand-written XML string concatenation.
8. Validate the OPML by parsing it as XML and checking node counts and top-level branch count.
9. Optionally create a XMind MCP version using the same final outline, after the OPML structure is stable.

## Mind Map Shape

Use a dynamic version of this default structure. Rename, merge, or remove branches to match the book:

1. `0. 一句话总纲｜...`
2. `1. 这本书试图解决什么问题`
3. `2. 核心理念与底层逻辑`
4. `3. 关键框架与方法`
5. `4. 工作流程 / 实践步骤`
6. `5. 角色分工与协作关系`
7. `6. 常见误区与边界条件`
8. `7. 可迁移到实际工作的启发`
9. `8. 我的行动清单｜基于本书批注练习`

For product/management books, common branch substitutions include:

- `需求采集与用户研究`
- `需求分析与取舍`
- `文档体系与表达工具`
- `商业、产品、技术协作关系`

Keep each top-level branch to about 5-8 second-level nodes at most. Keep the practical default depth to: central topic -> top-level branch -> category -> short leaf. Avoid going deeper unless the user's source notes demand it.

## Node Writing Rules

- Write in Chinese unless the user asks otherwise.
- Keep each node short enough for XMind; one line is ideal.
- Prefer verbs and usable phrases over abstract labels.
- Preserve important user notes and repeated highlights.
- Do not quote long passages from the book; compress into concise nodes.
- Avoid "目录复述" and avoid merely summarizing chapter by chapter.
- Use `补充理解` sparingly, only when the user explicitly permits it or when a necessary bridge is not a book claim. Prefer omitting uncertain material.
- Never mix in frameworks from books that are merely mentioned in the source annotations.

## OPML Requirements

Follow `references/opml-format.md` for the exact OPML shape and validation checklist.

The OPML root topic must be:

`书名｜读后学习复盘思维导图`

The generated file name should be filesystem-safe and include the book title, for example:

`人人都是产品经理_version_1.1_读后学习复盘思维导图.opml`

## Final Response

Report succinctly:

- OPML file path as a clickable local file link.
- Optional XMind MCP URL if created.
- Verification result: source highlight count, outline node count, top-level branch count, and XML validity.
- Any intentionally excluded material, especially external book references or uncertain ideas.

Do not include the generation script in the final response unless the user explicitly asks for it.
