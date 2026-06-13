# trellis-lightweight-tdd

一个给 Codex / Claude Code / 兼容 Agent 用的工作流 skill：把 Trellis 的项目记忆、spec、task、verify、finish 思想，和 Superpowers 的 TDD、red-green-refactor、证据优先纪律合并成可调档的开发体验。

核心:

> 小任务轻量直修，大项目走 Trellis 原生，关键实现可切 Superpowers 严格 TDD。

## 适合谁

- 已经在用 Trellis，但觉得小修小补不想每次都完整建任务的佬友。
- 喜欢 Superpowers 的测试纪律，但不想所有任务都强制 worktree / subagent / 大计划的佬友。
- 不想让小任务消耗大量token的佬友.
- 希望 AI 写代码前读项目规范、写完以后给验证证据的佬友。

## 三种模式

快捷触发词：

| 触发词 | 模式 | 适合场景 |
|---|---|---|
| `light模式` / `ight模式` | Lightweight Hybrid | 小 bug、小范围修改、文档、配置、小测试补充。`ight模式` 是容错别名。 |
| `tn模式` / `TN模式` | Trellis Native | 大项目、跨模块、需要 PRD / spec / verify / finish 的工作。 |
| `sn模式` / `SN模式` | Superpowers Native | 严格 TDD、red-green-refactor、证据先于结论。 |

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

适合明确要求严格 TDD、red-green-refactor、证据先于结论的时候。

流程：

1. 先写失败测试。
2. 确认它因为预期原因失败。
3. 写最小实现。
4. 确认测试变绿。
5. 再重构。
6. 完成前跑新鲜验证命令。

## 实际编程逻辑

不管使用哪种模式，实际写代码时都按这条主线走：

1. 先读最近的项目说明、相关代码、已有测试和入口文件。
2. 判断任务类型：bug / feature / refactor / docs-config / test-only。
3. 收紧范围：明确最小行为变化、可能影响的文件、需要验证的地方。
4. 先证明再改：
   - bug：优先复现或写失败回归测试；
   - feature：先确认第一个可观察行为；
   - refactor：先确认现有测试或补 characterization check；
   - docs/config：做语法、格式、构建或人工检查。
5. 窄范围修改，优先沿用项目已有模式，不顺手重构无关内容。
6. 先跑最近、最有意义的验证；改动影响面大时再扩大测试范围。
7. 最后汇报实际跑过的命令、结果和没能验证的缺口。

如果项目没有测试框架，不会为了流程硬搭一套大的测试体系；优先用 smoke test、脚本、type-check/build 或手动验证。

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
用 trellis-lightweight-tdd light模式修这个 bug，能写回归测试就先写失败测试，不要建 worktree。
```

容错触发：

```text
用 trellis-lightweight-tdd ight模式改这个小配置，只跑必要验证。
```

大项目：

```text
用 trellis-lightweight-tdd tn模式做这个功能，保留 PRD、spec、verify、finish 流程。
```

也可以写：

```text
用 trellis-lightweight-tdd TN模式做这个跨模块功能。
```

严格 TDD：

```text
用 trellis-lightweight-tdd SN模式，严格 red-green-refactor。
```

也可以写：

```text
用 trellis-lightweight-tdd sn模式修这个高风险回归。
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

但当主动要求 Trellis Native 或 Superpowers Native，它会尽量恢复原生体验。


## License

**学 AI，上 L 站**

**[Linux.do](https://linux.do)**

**欢迎各位佬友使用并给出修改意见**
