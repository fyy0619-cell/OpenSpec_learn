# OpenSpec 实战练习手册：从 0 做完一个真实 Change

> 这份文档是 README 的配套练习。README 负责讲概念，这里负责带你一步一步动手。

建议练习目标：先用一个“给仓库增加术语表”的文档型需求，完整走一遍 OpenSpec 流程。这个练习不涉及复杂代码，但能覆盖 proposal、specs、design、tasks、validate 和 archive。

---

## 目录

- [0. 本练习你会学会什么](#0-本练习你会学会什么)
- [1. 练习前准备](#1-练习前准备)
- [2. 练习项目：增加 OpenSpec 术语表](#2-练习项目增加-openspec-术语表)
- [3. 第一步：创建 change](#3-第一步创建-change)
- [4. 第二步：写 proposal.md](#4-第二步写-proposalmd)
- [5. 第三步：写规格 specs](#5-第三步写规格-specs)
- [6. 第四步：写 design.md](#6-第四步写-designmd)
- [7. 第五步：写 tasks.md](#7-第五步写-tasksmd)
- [8. 第六步：检查 artifact 状态](#8-第六步检查-artifact-状态)
- [9. 第七步：严格校验](#9-第七步严格校验)
- [10. 第八步：实现文档](#10-第八步实现文档)
- [11. 第九步：最终校验和提交](#11-第九步最终校验和提交)
- [12. 第十步：什么时候 archive](#12-第十步什么时候-archive)
- [13. 如何举一反三](#13-如何举一反三)
- [14. 最终检查清单](#14-最终检查清单)

---

## 0. 本练习你会学会什么

完成本练习后，你应该能独立完成：

- 创建一个 OpenSpec change；
- 编写 `proposal.md`；
- 编写规格增量 `specs/<capability>/spec.md`；
- 编写 `design.md`；
- 编写 `tasks.md`；
- 使用 `status` 查看 artifact 完成度；
- 使用 `validate --all --strict` 校验；
- 根据 tasks 实现内容；
- 在合适时机归档 change。

---

## 1. 练习前准备

进入仓库：

```bash
cd D:\OpenSpec_learn
```

检查 OpenSpec 是否可用：

```bash
npx @fission-ai/openspec@latest --version
npx @fission-ai/openspec@latest --help
```

查看当前 active changes：

```bash
npx @fission-ai/openspec@latest list
```

查看当前正式 specs：

```bash
npx @fission-ai/openspec@latest list --specs
```

如果你是第一次接触，先运行：

```bash
npx @fission-ai/openspec@latest status --change add-learning-notes
```

你会看到一个已经写好的示例 change。

---

## 2. 练习项目：增加 OpenSpec 术语表

我们要做一个小功能：给仓库增加一份 `docs/openspec-glossary.md`，解释 OpenSpec 常见术语。

为什么选这个练习？

- 不涉及复杂代码，适合新手；
- 但流程完整，能练到 proposal、specs、design、tasks；
- 可以安全验证，不容易破坏项目。

---

## 3. 第一步：创建 change

命令：

```bash
npx @fission-ai/openspec@latest new change add-openspec-glossary --description "增加 OpenSpec 术语表"
```

生成目录：

```text
openspec/changes/add-openspec-glossary/
  .openspec.yaml
  README.md
```

现在查看状态：

```bash
npx @fission-ai/openspec@latest status --change add-openspec-glossary
```

你会看到 proposal、design、specs、tasks 还没有完成。

---

## 4. 第二步：写 proposal.md

创建文件：

```text
openspec/changes/add-openspec-glossary/proposal.md
```

参考内容：

```markdown
## Why

Beginners often see terms like change, spec, proposal, archive, and capability, but they may not understand the difference between them.

## What Changes

- Add a Chinese glossary document for common OpenSpec terms.
- Link the glossary from README.md.
- Keep the glossary focused on beginner learning.

## Capabilities

### New Capabilities
- `openspec-glossary`: Beginner glossary for common OpenSpec terms.

### Modified Capabilities
- None.

## Impact

- Adds `docs/openspec-glossary.md`.
- Updates `README.md` with a link.
- Does not change runtime behavior.
```

`proposal.md` 不是实现方案，它主要回答“为什么做”和“做什么”。不要在 proposal 里写太多代码细节。

---

## 5. 第三步：写规格 specs

创建目录：

```text
openspec/changes/add-openspec-glossary/specs/openspec-glossary/
```

创建文件：

```text
openspec/changes/add-openspec-glossary/specs/openspec-glossary/spec.md
```

参考内容：

```markdown
## ADDED Requirements

### Requirement: Glossary explains core OpenSpec terms
The repository SHALL provide a Chinese glossary that explains core OpenSpec terms for beginners.

#### Scenario: Beginner reads glossary
- **WHEN** a beginner opens the glossary
- **THEN** they can understand terms including change, spec, proposal, design, tasks, capability, validation, and archive

### Requirement: Glossary is linked from README
The repository SHALL link the glossary from the main README.

#### Scenario: Beginner starts from README
- **WHEN** a beginner reads README.md
- **THEN** they can navigate to the glossary document
```

规格不是普通说明文档，它要尽量可验证。记住：

- `### Requirement:` 是三级标题；
- `#### Scenario:` 是四级标题；
- 每个 requirement 至少一个 scenario；
- `SHALL` / `MUST` 表示必须满足。

---

## 6. 第四步：写 design.md

创建文件：

```text
openspec/changes/add-openspec-glossary/design.md
```

参考内容：

```markdown
## Context

The repository is a beginner learning repository for OpenSpec. README is already long, so a separate glossary keeps the entry document readable.

## Goals / Non-Goals

**Goals:**

- Add a glossary for common OpenSpec terms.
- Keep each explanation short and beginner-friendly.
- Link the glossary from README.

**Non-Goals:**

- Do not translate the full OpenSpec source documentation.
- Do not add application code.

## Decisions

- Put the glossary under `docs/` because it is supporting learning material.
- Use a Markdown table for quick scanning.
- Keep explanations in Chinese while retaining English term names.

## Risks / Trade-offs

- Glossary may become outdated -> Keep entries simple and update when OpenSpec concepts change.
```

---

## 7. 第五步：写 tasks.md

创建文件：

```text
openspec/changes/add-openspec-glossary/tasks.md
```

参考内容：

```markdown
## 1. Glossary Content

- [ ] 1.1 Create `docs/openspec-glossary.md`.
- [ ] 1.2 Explain core terms: change, spec, proposal, design, tasks, capability, validation, archive.
- [ ] 1.3 Add examples for confusing terms.

## 2. Navigation

- [ ] 2.1 Link the glossary from `README.md`.

## 3. Validation

- [ ] 3.1 Run `npx @fission-ai/openspec@latest validate --all --strict`.
- [ ] 3.2 Fix validation errors.
```

任务要小，最好每一项都能在一次会话里完成。

---

## 8. 第六步：检查 artifact 状态

```bash
npx @fission-ai/openspec@latest status --change add-openspec-glossary
```

如果 proposal、design、specs、tasks 都写好了，应该显示 4/4 完成。

---

## 9. 第七步：严格校验

```bash
npx @fission-ai/openspec@latest validate --all --strict
```

如果失败，常见原因有：

- `#### Scenario` 写成了 `### Scenario`；
- requirement 没有 scenario；
- proposal 里写了 capability，但 specs 目录名称不一致；
- Markdown 标题层级不符合 OpenSpec 要求。

---

## 10. 第八步：实现文档

创建：

```text
docs/openspec-glossary.md
```

可以写这些术语：

| Term | 中文理解 |
| --- | --- |
| OpenSpec | 规格驱动开发工具 |
| Spec | 正式规格 |
| Change | 一次正在进行的变更 |
| Proposal | 变更提案 |
| Design | 技术设计 |
| Tasks | 执行清单 |
| Capability | 系统能力 |
| Scenario | 验收场景 |
| Validation | 规格校验 |
| Archive | 归档变更 |

然后在 `README.md` 中加入链接：

```markdown
- [OpenSpec 术语表](docs/openspec-glossary.md)
```

完成任务后，把 `tasks.md` 改为：

```markdown
- [x] 1.1 Create `docs/openspec-glossary.md`.
```

---

## 11. 第九步：最终校验和提交

最终校验：

```bash
npx @fission-ai/openspec@latest validate --all --strict
```

查看 Git 改动：

```bash
git status --short
git diff --stat
```

提交：

```bash
git add README.md docs/openspec-glossary.md openspec/changes/add-openspec-glossary
git commit -m "Add OpenSpec glossary"
git push origin main
```

---

## 12. 第十步：什么时候 archive

当你确认这些都完成后，再归档：

- 文档已经完成；
- README 链接已经添加；
- OpenSpec 校验通过；
- GitHub 上内容显示正常。

归档命令：

```bash
npx @fission-ai/openspec@latest archive add-openspec-glossary -y
```

如果你暂时想保留 active change 供学习观察，可以先不归档。

---

## 13. 如何举一反三

掌握这个练习后，可以换成真实项目需求：

- `add-fall-alert-threshold-config`：增加摔倒阈值配置；
- `fix-sle-reconnect`：修复 SLE 断连重连；
- `add-wifi-alert-retry`：增加 Wi-Fi 报警重试；
- `add-mpu6050-sampling-log`：增加 MPU6050 采样日志；
- `refactor-led-status-state-machine`：重构 LED 状态机。

无论需求是什么，流程不变：

```text
new change -> proposal -> specs -> design -> tasks -> implement -> validate -> archive
```

---

## 14. 最终检查清单

每次做 OpenSpec change 前后，都可以检查：

- [ ] change 名字是否清晰？
- [ ] proposal 是否解释了 Why / What / Capabilities / Impact？
- [ ] specs 是否包含 SHALL/MUST？
- [ ] 每个 requirement 是否至少有一个 `#### Scenario`？
- [ ] design 是否解释了关键技术决策？
- [ ] tasks 是否使用 `- [ ]` checkbox？
- [ ] 是否运行了 `validate --all --strict`？
- [ ] 是否只实现了 specs 定义的范围？
- [ ] 完成后是否考虑 archive？
