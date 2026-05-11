# OpenSpec_learn：OpenSpec 从零到项目落地学习笔记

> 本仓库用于持续学习 OpenSpec。本文按“小白也能跟着做”的方式，讲清楚 OpenSpec 是什么、为什么用、底层思路、常用命令、工程目录、完整项目流程和常见坑。

更新时间：2026-05-11  
当前核对版本：`@fission-ai/openspec@1.3.1`  
官方仓库：https://github.com/Fission-AI/OpenSpec  
NPM 包：https://www.npmjs.com/package/@fission-ai/openspec

---

## 1. 一句话理解 OpenSpec

OpenSpec 是一个 **AI-native 的规格驱动开发工具**。

你可以把它理解成“给人和 AI 同时看的施工图纸”：

- 人先把需求、边界、验收标准写清楚；
- AI 再根据这些规格去写代码；
- 每个功能都有提案、规格、设计、任务和归档记录；
- 项目越做越大时，不会只剩一堆没人敢改的代码。

传统开发常见流程是：

```text
有个想法 -> 直接写代码 -> 临时调试 -> 事后补文档或不补文档
```

OpenSpec 推荐的流程是：

```text
想法 -> proposal -> specs -> design -> tasks -> implementation -> validation -> archive
```

核心口诀：

> 先提案，再规格；先验收，再实现；小步变更，完成归档。

---

## 2. 为什么需要 OpenSpec

如果你直接对 AI 说：

```text
帮我做登录功能
帮我优化摔倒检测报警
帮我把项目整体整理一下
```

AI 可能马上开始写代码，但很容易出现这些问题：

- **需求不清**：什么算完成？哪些场景必须支持？
- **边界不清**：哪些不做？哪些旧功能不能破坏？
- **验收不清**：做完后怎么判断对不对？
- **过程不清**：今天改了什么，明天很难复盘。
- **文档脱节**：代码变了，需求文档没变。

OpenSpec 的价值是：

1. **把需求结构化**：把“想法”变成可以检查的文档。
2. **把 AI 约束住**：AI 不再自由发挥，而是按规格执行。
3. **把验收前置**：场景写在 spec 里，天然就是测试用例来源。
4. **把变更留痕**：每个 change 都有独立目录，便于 code review。
5. **把知识沉淀**：完成后归档进正式 specs，形成长期项目记忆。

---

## 3. OpenSpec 的几个核心概念

### 3.1 Project

一个接入 OpenSpec 的工程项目。初始化后会出现：

```text
openspec/
  config.yaml
  specs/
  changes/
```

### 3.2 Spec

`spec` 是正式规格，描述项目当前已经承诺支持的能力。

示例：

```text
openspec/specs/user-auth/spec.md
openspec/specs/fall-alert/spec.md
openspec/specs/wifi-gateway/spec.md
```

### 3.3 Change

`change` 是一次正在进行的变更。一个 change 通常对应一个功能、一个修复或一次重构。

示例：

```text
openspec/changes/add-user-login/
openspec/changes/fix-fall-alert-retry/
```

### 3.4 Proposal

`proposal.md` 回答：

- 为什么做？
- 做什么？
- 不做什么？
- 影响哪些能力？

### 3.5 Specs Delta

change 里的 `specs/<capability>/spec.md` 不是完整正式规格，而是“规格增量”。

它描述这次 change 要新增、修改、删除或重命名哪些需求。

### 3.6 Design

`design.md` 回答“怎么做”，包括技术方案、取舍、风险、迁移计划等。

### 3.7 Tasks

`tasks.md` 是可执行清单。AI 或开发者按任务逐项完成。

### 3.8 Archive

功能完成后执行归档：

- change 被移动到 `openspec/changes/archive/`；
- change 中的规格增量被合并到正式 `openspec/specs/`。

---

## 4. 本仓库已经初始化的内容

本仓库已经执行过：

```bash
npx @fission-ai/openspec@latest init . --tools codex --force
npx @fission-ai/openspec@latest new change add-learning-notes --description "示例：为学习仓库增加 OpenSpec 入门笔记"
```

当前主要结构：

```text
OpenSpec_learn/
  README.md
  openspec/
    config.yaml
    specs/
    changes/
      add-learning-notes/
        .openspec.yaml
        README.md
        proposal.md
        design.md
        tasks.md
        specs/
          learning-guide/
            spec.md
  .codex/
    skills/
      openspec-propose/
      openspec-apply-change/
      openspec-archive-change/
      openspec-explore/
```

其中 `add-learning-notes` 是一个可学习、可校验的示例 change。

---

## 5. 安装和环境准备

OpenSpec 通过 NPM 分发，所以需要 Node.js 和 npm。

检查环境：

```bash
node --version
npm --version
```

不想全局安装时，推荐直接使用：

```bash
npx @fission-ai/openspec@latest --help
```

如果你想全局安装：

```bash
npm install -g @fission-ai/openspec
openspec --version
```

后文命令如果写 `openspec xxx`，都可以替换成：

```bash
npx @fission-ai/openspec@latest xxx
```

---

## 6. 初始化一个真实项目

进入你的工程：

```bash
cd your-project
```

初始化 OpenSpec：

```bash
npx @fission-ai/openspec@latest init . --tools codex
```

参数解释：

- `init`：初始化 OpenSpec；
- `.`：在当前目录初始化；
- `--tools codex`：生成 Codex 相关技能文件；
- `--force`：自动清理旧版文件，适合你确认要覆盖兼容文件时使用。

查看支持哪些 AI 工具：

```bash
npx @fission-ai/openspec@latest init --help
```

官方 CLI 当前支持的工具包括 codex、cursor、claude、cline、github-copilot、gemini、qwen、windsurf 等。

---

## 7. 完整工程流程：从想法到归档

下面用“增加一个学习笔记功能”演示。

### 第 1 步：创建 change

```bash
npx @fission-ai/openspec@latest new change add-learning-notes --description "示例：为学习仓库增加 OpenSpec 入门笔记"
```

建议命名规则：

- 英文小写；
- 单词之间用 `-`；
- 动词开头，例如 `add-xxx`、`fix-xxx`、`refactor-xxx`。

### 第 2 步：写 proposal.md

模板：

```markdown
## Why

说明为什么需要这个变更。

## What Changes

- 增加什么
- 修改什么
- 删除什么

## Capabilities

### New Capabilities
- `learning-guide`: 学习指南能力

### Modified Capabilities
- None.

## Impact

- 影响 README
- 不影响运行时代码
```

### 第 3 步：写 specs

路径一般是：

```text
openspec/changes/<change-name>/specs/<capability>/spec.md
```

示例：

```markdown
## ADDED Requirements

### Requirement: Beginner guide explains the OpenSpec workflow
The repository SHALL provide a beginner-friendly guide that explains the OpenSpec workflow from initialization to archive.

#### Scenario: Reader follows the guide
- **WHEN** a beginner reads the guide
- **THEN** they can understand proposal, specs, design, tasks, validation, and archive
```

注意：

- requirement 用三级标题：`### Requirement: ...`
- scenario 用四级标题：`#### Scenario: ...`
- 每个 requirement 至少要有一个 scenario；
- 使用 `SHALL` / `MUST` 表示强约束；
- `WHEN` / `THEN` 可以直接转成验收测试思路。

### 第 4 步：写 design.md

模板：

```markdown
## Context

当前背景和约束。

## Goals / Non-Goals

**Goals:**
- 要达成的目标

**Non-Goals:**
- 明确不做的事情

## Decisions

- 技术决策和原因

## Risks / Trade-offs

- 风险 -> 缓解方法
```

### 第 5 步：写 tasks.md

任务必须使用 checkbox：

```markdown
## 1. Documentation

- [ ] 1.1 更新 README
- [ ] 1.2 增加示例 change

## 2. Validation

- [ ] 2.1 运行 openspec validate --all --strict
```

完成后改成：

```markdown
- [x] 1.1 更新 README
```

### 第 6 步：查看状态

```bash
npx @fission-ai/openspec@latest status --change add-learning-notes
```

如果四项都完成，会看到 proposal、design、specs、tasks 都是完成状态。

### 第 7 步：校验

```bash
npx @fission-ai/openspec@latest validate --all --strict
```

提交前建议总是运行这条命令。

### 第 8 步：实现

让 AI 或开发者按 `tasks.md` 一项一项实现。

推荐对 AI 这样说：

```text
请阅读 openspec/config.yaml 和 openspec/changes/add-learning-notes 下的 proposal、specs、design、tasks。
严格按 tasks.md 实现，不要做超出 specs 的改动。
```

### 第 9 步：归档

功能完成并验收后：

```bash
npx @fission-ai/openspec@latest archive add-learning-notes -y
```

学习阶段可以先不归档，保留 active change 方便观察结构。

---

## 8. 常用命令速查

### 8.1 帮助和版本

```bash
openspec --help
openspec --version
```

### 8.2 初始化和更新

```bash
openspec init . --tools codex
openspec update .
```

### 8.3 列表

```bash
openspec list
openspec list --specs
```

### 8.4 新建变更

```bash
openspec new change add-demo --description "增加演示功能"
```

### 8.5 查看内容

```bash
openspec show add-demo
openspec show add-demo --json
openspec show learning-guide --type spec
```

### 8.6 校验

```bash
openspec validate add-demo
openspec validate --changes
openspec validate --specs
openspec validate --all --strict
openspec validate --all --strict --json
```

### 8.7 管理规格

```bash
openspec spec list
openspec spec show learning-guide
openspec spec validate learning-guide
```

### 8.8 管理变更

```bash
openspec change show add-demo
openspec change validate add-demo
```

### 8.9 查看 artifact 状态

```bash
openspec status --change add-demo
openspec status --change add-demo --json
```

### 8.10 获取写作指导

```bash
openspec instructions proposal --change add-demo
openspec instructions specs --change add-demo
openspec instructions design --change add-demo
openspec instructions tasks --change add-demo
```

不知道某个文件怎么写时，优先运行 `instructions`。

### 8.11 归档

```bash
openspec archive add-demo -y
```

常用选项：

- `--skip-specs`：只归档，不更新正式 specs，适合纯工具或纯文档变更；
- `--no-validate`：跳过校验，不推荐。

---

## 9. Spec 写法详解

OpenSpec 的规格增量常见有 4 类。

### 9.1 ADDED Requirements

新增需求：

```markdown
## ADDED Requirements

### Requirement: User can log in
The system SHALL allow a registered user to log in with email and password.

#### Scenario: Successful login
- **WHEN** the user submits a valid email and password
- **THEN** the system SHALL create a logged-in session
```

### 9.2 MODIFIED Requirements

修改已有需求时，必须复制完整 requirement 块，而不是只写变化部分：

```markdown
## MODIFIED Requirements

### Requirement: User can log in
The system SHALL allow a registered user to log in with email and password, and SHALL reject locked accounts.

#### Scenario: Successful login
- **WHEN** the user submits a valid email and password
- **THEN** the system SHALL create a logged-in session

#### Scenario: Locked account
- **WHEN** a locked user submits correct credentials
- **THEN** the system SHALL reject the login
```

### 9.3 REMOVED Requirements

删除需求：

```markdown
## REMOVED Requirements

### Requirement: Password login without rate limit
**Reason**: It is insecure.
**Migration**: Use rate-limited password login.
```

### 9.4 RENAMED Requirements

重命名需求：

```markdown
## RENAMED Requirements

### Requirement: Login
FROM: User login
TO: User authentication
```

---

## 10. OpenSpec 和 AI 编程助手怎么配合

推荐流程：

1. 让 AI 先探索项目结构，不要改代码；
2. 创建 OpenSpec change；
3. 先写 proposal/specs/design/tasks；
4. 人审核这些文档；
5. AI 根据 tasks 实现；
6. 运行测试和 `openspec validate --all --strict`；
7. 完成后归档。

推荐提示词：

```text
请基于 OpenSpec 创建一个 change：优化摔倒检测报警的误报处理。
先只写 proposal、specs、design、tasks，不要改代码。
```

实现阶段提示词：

```text
请严格根据 openspec/changes/<change-name>/tasks.md 执行。
不得实现 specs 中没有定义的额外功能。
完成每个任务后更新 checkbox，并运行必要验证。
```

---

## 11. 嵌入式 / IoT 项目使用建议

如果你的项目是 WS63、LiteOS、C 语言、MPU6050、SLE、Wi-Fi、4G DTU、摔倒检测这类嵌入式项目，可以这样拆规格：

```text
openspec/specs/
  fall-detection/spec.md
  mpu6050-sampling/spec.md
  sle-communication/spec.md
  wifi-alert-gateway/spec.md
  led-status/spec.md
```

嵌入式规格里尤其要写清楚：

- 采样频率；
- 阈值单位；
- 状态机；
- 通信帧格式；
- 超时和重试；
- RAM / Flash / 栈大小限制；
- 断网、低电量、传感器异常等失败策略；
- 验收方法，例如串口日志、LED 状态、抓包、后端收到报警。

示例：

```markdown
## ADDED Requirements

### Requirement: Fall event triggers alert
The system SHALL send an alert when the fall detection algorithm confirms a fall event.

#### Scenario: Confirmed fall
- **WHEN** acceleration and posture features match the configured fall threshold
- **THEN** the system SHALL enqueue one alert message
```

---

## 12. 常见错误

### 12.1 只写 tasks，不写 specs

问题：AI 不知道验收标准。  
做法：先写 proposal 和 specs，再写 tasks。

### 12.2 Scenario 标题写错

错误：

```markdown
### Scenario: Successful login
```

正确：

```markdown
#### Scenario: Successful login
```

### 12.3 MODIFIED 只写变化点

修改需求时要复制完整 requirement，包括所有 scenario。

### 12.4 一个 change 太大

不要写：

```text
add-whole-system
```

建议拆成：

```text
add-wifi-alert-gateway
add-sle-device-pairing
add-fall-threshold-config
```

### 12.5 过早归档

只有代码、测试、文档都完成后才归档。否则正式 specs 会记录一个“写了但没实现”的能力。

---

## 13. 新手练习路线

建议按这个顺序练：

1. 阅读本文；
2. 运行 `openspec --help`；
3. 运行 `openspec status --change add-learning-notes`；
4. 打开 `openspec/changes/add-learning-notes/` 逐个理解文件；
5. 自己创建一个 `add-first-demo-spec`；
6. 只写 proposal 和 spec；
7. 运行 `openspec validate --all --strict`；
8. 再写 design 和 tasks；
9. 让 AI 根据 tasks 实现；
10. 功能完成后 archive。

---

## 14. 本仓库验证命令

```bash
npx @fission-ai/openspec@latest status --change add-learning-notes
npx @fission-ai/openspec@latest validate --all --strict
```

当前示例 change 已通过严格校验。

---

## 15. 最后再记一遍

OpenSpec 不是“替你写代码”的工具，而是“让你、团队和 AI 在写代码前达成共识”的工具。

只要坚持：

```text
proposal -> specs -> design -> tasks -> implementation -> validate -> archive
```

你的项目就会更容易维护，AI 也更不容易乱改。
