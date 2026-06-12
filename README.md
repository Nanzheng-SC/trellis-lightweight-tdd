# trellis-lightweight-tdd

一个给 Codex / Claude Code / 兼容 Agent 用的工作流 skill：把 Trellis 的项目记忆、spec、task、verify、finish 思想，和 Superpowers 的 TDD、red-green-refactor、证据优先纪律合并成可调档的开发体验。

它不是又一个重流程。它的核心是：

> 小任务轻量直修，大项目走 Trellis 原生，关键实现可切 Superpowers 严格 TDD。

## 适合谁

- 已经在用 Trellis，但觉得小修小补不想每次都完整建任务的人。
- 喜欢 Superpowers 的测试纪律，但不想所有任务都强制 worktree / subagent / 大计划的人。
- 在 Codex 桌面版、CLI、多账号 API 环境里，希望同一个 skill 可以跨环境复用的人。
- 希望 AI 写代码前读项目规范、写完以后给验证证据的人。

## 三种模式

### 1. Lightweight Hybrid

默认模式。适合明确 bug、小范围改动、文档、配置、小测试补充。

流程：

1. 读相关代码和项目说明。
2. 判断影响范围。
3. 能写失败测试就先写最小失败测试。
4. 直接实现。
5. 跑最小必要验证。
6. 汇报改动和证据。

### 2. Trellis Native

适合大项目、跨模块、需求不清、需要 PRD / research / spec / finish 的工作。

保留 Trellis 的原生体验：

- `.trellis/spec/`
- `.trellis/tasks/`
- `prd.md`
- `implement.jsonl`
- `check.jsonl`
- Plan -> Implement -> Verify -> Finish
- `trellis-before-dev`
- `trellis-check`
- `trellis-update-spec`
- `finish-work`

### 3. Superpowers Native

适合你明确要求严格 TDD、red-green-refactor、证据先于结论的时候。

流程：

1. 先写失败测试。
2. 确认它因为预期原因失败。
3. 写最小实现。
4. 确认测试变绿。
5. 再重构。
6. 完成前跑新鲜验证命令。

## 安装

把目录放到：

```text
~/.codex/skills/trellis-lightweight-tdd
```

目录结构：

```text
trellis-lightweight-tdd/
├── SKILL.md
├── README.md
└── references/
    └── merge-analysis.md
```

## 推荐调用方式

轻量任务：

```text
用 trellis-lightweight-tdd 轻量模式修这个 bug，能写回归测试就先写失败测试，不要建 worktree。
```

大项目：

```text
用 trellis-lightweight-tdd 的 Trellis Native 模式做这个功能，保留 PRD、spec、verify、finish 流程。
```

严格 TDD：

```text
用 trellis-lightweight-tdd 的 Superpowers Native 模式，严格 red-green-refactor。
```

Codex inline：

```text
按 trellis-lightweight-tdd 做，读取 Trellis spec，但这次在主线程 inline 实现，不派 subagent。
```

## 它和 Trellis / Superpowers 的关系

- Trellis 擅长项目级记忆、规范、任务上下文和收尾沉淀。
- Superpowers 擅长强流程、TDD、验证纪律和防止“没跑测试就说好了”。
- trellis-lightweight-tdd 把两者合并成一个可调档入口。

默认不强制：

- worktree
- 新分支
- subagent
- 大型 plan
- 默认 commit
- 默认 push

但当你主动要求 Trellis Native 或 Superpowers Native，它会尽量恢复原生体验。

## 适合推荐给朋友的说法

短版：

> 这是一个把 Trellis 项目记忆和 Superpowers TDD 纪律合在一起的 Codex skill。小修不走重流程，大活能走 Trellis 原生，关键代码还能切严格红绿重构。

更口语一点：

> 给 Codex 装个“项目管理 + 测试纪律”的变速箱。平时小 bug 直接修，大需求走 Trellis，想上强度就切 Superpowers TDD。

面向工程师：

> trellis-lightweight-tdd is a workflow-router skill for agentic coding. It routes between lightweight inline fixes, Trellis-native task/spec workflows, and Superpowers-style strict TDD, with verification evidence before completion claims.

## License

MIT

致谢：Linux.do，学 AI，上 L 站
