# Research Workflow Skill Usage Guide

本指南教你安装、调用、切换模式并定制 `research-mentor`。中文在前，English follows.

官方参考：[Anthropic Agent Skills docs](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) 说明 Skill 需要带有 `name` 和 `description` 的 YAML frontmatter；[Claude Help Center: Use Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude) 说明自定义 Skills 可通过 zip 上传并在 `Customize > Skills` 启用。

## 中文指南

### 1. 安装到 Claude Code

如果你的 Claude Code 使用本地 skills 目录，推荐把本仓库作为一个独立 skill 文件夹复制进去：

```powershell
$src = "C:\Users\JInhAoYang\Desktop\科研工作流skill"
$dst = "$HOME\.claude\skills\research-mentor"
New-Item -ItemType Directory -Force -Path (Split-Path $dst) | Out-Null
Copy-Item -Recurse -Force $src $dst
```

确认目标目录中有：

```text
research-mentor/
├── SKILL.md
├── references/
└── examples/
```

重启 Claude Code，或开启一个新会话，然后用自然语言触发：

```text
使用 research-mentor。导师模式：我想做一个关于城市能源系统负荷预测的研究，请帮我收敛选题。
```

### 2. 上传到 Claude.ai

1. 确保仓库根目录包含 `SKILL.md`。
2. 将文件夹压缩成 zip。zip 内部应包含名为 `research-mentor/` 的 skill 文件夹，而不是把所有文件直接散放在 zip 根目录。

```powershell
$repo = "C:\Users\JInhAoYang\Desktop\科研工作流skill"
$pkgRoot = Join-Path $env:TEMP "research-mentor-skill-package"
$skillDir = Join-Path $pkgRoot "research-mentor"
Remove-Item -Recurse -Force $pkgRoot -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null
Copy-Item -Recurse -Force "$repo\*" $skillDir -Exclude ".git"
Compress-Archive -Path $skillDir -DestinationPath "$HOME\Desktop\research-mentor.zip" -Force
```

3. 在 Claude.ai 打开 `Customize > Skills`。
4. 点击 `+` 或 `Create skill`，选择 `Upload a skill`。
5. 上传 `research-mentor.zip`，然后启用该 Skill。

### 3. First run / 第一次运行

建议第一次这样调用：

```text
使用 research-mentor，mentor 模式。
我的研究领域是 <你的领域>，我目前处于 <选题/方法/实验/写作/投稿> 阶段。
我已有 <数据/代码/文献/草稿>，目标是 <会议/期刊/毕业论文/项目申请>。
请先判断我处于哪个科研阶段，再给我下一步计划。
```

如果你已经有草稿，可以这样调用：

```text
使用 research-mentor，reviewer 模式。
请严格审查下面的方法论段落，指出会导致拒稿的 major concerns，并给出修改方案：
<粘贴你的方法论>
```

### 4. 阶段调用速查

| 阶段 | 你可以这样说 |
| --- | --- |
| 1. 选题 / 文献调研 | `我应该读哪些文献？帮我做 literature review plan。` |
| 2. 问题与假设 | `帮我把这个方向改成 research question 和 hypothesis。` |
| 3. 方法与实验设计 | `帮我设计实验；帮我审一下这段方法论。` |
| 4. 数据 / 复现性 | `检查我的数据划分有没有 leakage；帮我写 reproducibility plan。` |
| 5. 结果 / 消融 | `这些结果能支持我的 claim 吗？还需要哪些 ablation？` |
| 6. 论文写作 | `帮我改 abstract/introduction/related work/methods/results/discussion。` |
| 7. 自审 / 模拟同行评议 | `请模拟 reviewer 给出 major concerns 和评分。` |
| 8. 修改与 rebuttal | `帮我回应 reviewer comment，并制定 revision plan。` |
| 9. 投稿策略 | `帮我判断这篇论文适合投哪个 venue。` |

### 5. 模式切换速查

| 模式 | 适合何时使用 | 触发说法 |
| --- | --- | --- |
| `mentor` | 你需要规划、收敛、学习路径、下一步建议 | `导师模式`、`mentor mode`、`帮我规划`、`下一步怎么做` |
| `reviewer` | 你需要挑错、审稿、找拒稿风险、回应审稿人 | `reviewer mode`、`reviewer 模式`、`审查模式`、`严格审查`、`挑刺`、`major concerns`、`rebuttal` |
| `mixed` | 你既想得到导师建议，也想看到审稿人风险清单 | `mixed mode`、`混合模式`、`mentor + reviewer`、`导师加审稿人`、`both modes`、`两种模式` |

### 6. 定制到你的子领域

你可以直接修改 `references/` 中对应阶段的文件：

- 做强化学习：在 `03-methodology-experimental-design.md` 增加 environment、reward、policy、seed、offline/online evaluation 的 checklist。
- 做能源系统：在 `04-data-reproducibility.md` 增加时间序列划分、极端天气、节假日、拓扑、负荷泄漏检查。
- 做社会科学：在 `02-research-question.md` 和 `04-data-reproducibility.md` 增加识别策略、混杂变量、稳健性检验。

修改后检查：

```powershell
Select-String -Path .\SKILL.md -Pattern "references/"
Get-ChildItem .\references
```

## English Guide

### 1. Install in Claude Code

If your Claude Code setup uses a local skills directory, copy this repository as one skill folder:

```powershell
$src = "C:\Users\JInhAoYang\Desktop\科研工作流skill"
$dst = "$HOME\.claude\skills\research-mentor"
New-Item -ItemType Directory -Force -Path (Split-Path $dst) | Out-Null
Copy-Item -Recurse -Force $src $dst
```

Open a new Claude Code session and invoke it naturally:

```text
Use research-mentor in mentor mode. I need to scope a publishable research topic in ML for energy systems.
```

### 2. Upload to Claude.ai

1. Make sure the repository root contains `SKILL.md`.
2. Zip the skill folder. The zip should contain a top-level `research-mentor/` folder:

```powershell
$repo = "C:\Users\JInhAoYang\Desktop\科研工作流skill"
$pkgRoot = Join-Path $env:TEMP "research-mentor-skill-package"
$skillDir = Join-Path $pkgRoot "research-mentor"
Remove-Item -Recurse -Force $pkgRoot -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null
Copy-Item -Recurse -Force "$repo\*" $skillDir -Exclude ".git"
Compress-Archive -Path $skillDir -DestinationPath "$HOME\Desktop\research-mentor.zip" -Force
```

3. In Claude.ai, open `Customize > Skills`.
4. Choose `Create skill` or the `+` button, then `Upload a skill`.
5. Upload the zip and enable the Skill.

### 3. First Run

Start with:

```text
Use research-mentor in mentor mode.
My field is <field>. I am currently at the <topic/method/experiment/writing/submission> stage.
I have <data/code/papers/draft>. My target is <venue or outcome>.
Please identify my current research phase and give me a next-step plan.
```

For critique:

```text
Use research-mentor in reviewer mode.
Critique the following methodology section. Lead with major concerns and rejection risks:
<paste your method section>
```

### 4. Phase Invocation Cheat Sheet

| Phase | Example prompt |
| --- | --- |
| 1. Topic and literature review | `What papers should I read, and how should I map this literature?` |
| 2. Research question and hypothesis | `Turn this topic into research questions and hypotheses.` |
| 3. Method and experiment design | `Help me design the experiment and critique my methodology.` |
| 4. Data and reproducibility | `Check my data split for leakage and write a reproducibility plan.` |
| 5. Results and ablations | `Do these results support my claim? What ablations are missing?` |
| 6. Paper drafting | `Improve my abstract/introduction/related work/methods/results/discussion.` |
| 7. Simulated peer review | `Simulate peer review and give major concerns.` |
| 8. Revision and rebuttal | `Help me answer reviewer comments and plan revisions.` |
| 9. Submission strategy | `Which venues fit this paper and why?` |

### 5. Mode Switching Cheat Sheet

| Mode | Use when | Trigger phrases |
| --- | --- | --- |
| `mentor` | You need guidance, planning, tradeoffs, or next steps. | `mentor mode`, `guide me`, `help me plan`, `what should I do next` |
| `reviewer` | You need critique, rejection risks, or rebuttal help. | `reviewer mode`, `审查模式`, `critique`, `peer review`, `major concerns`, `rebuttal` |
| `mixed` | You need both mentor guidance and reviewer stress testing in one response. | `mixed mode`, `混合模式`, `mentor + reviewer`, `导师加审稿人`, `both modes`, `两种模式` |

### 6. Customize for Your Subfield

Edit the relevant file in `references/`:

- Reinforcement learning: add environment, reward, policy, seed, and offline/online evaluation checks to `03-methodology-experimental-design.md`.
- Energy systems: add time-series splits, topology, weather, holiday, and leakage checks to `04-data-reproducibility.md`.
- Social science: add identification strategy, confounders, and robustness checks to `02-research-question.md` and `04-data-reproducibility.md`.

Then verify:

```powershell
Select-String -Path .\SKILL.md -Pattern "references/"
Get-ChildItem .\references
```
