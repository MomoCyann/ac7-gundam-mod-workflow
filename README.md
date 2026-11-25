# AC7 高达机体替换 · 柯西 1.0 制作记录与 Agent Skill

这份记录来自把《装甲核心 VI》的柯西高达外观移植到《皇牌空战 7》X-02S 的实际项目。它覆盖 Blender 分件装配、骨骼重绑、UE 4.18 Cook、速度变形、SP1 武器切换、灯光、武器蓝图和实机排错。仓库提供文档和可安装的 Skill。跟做时请准备自己有权使用的素材和本地游戏环境。

| 从哪里开始 | 有什么内容 |
| --- | --- |
| [工具和准备](skills/ac7-gundam-mod/references/setup.md) | 解包工具、Blender 插件、UE 版本、验证工具，以及第一次做时应先验证什么 |
| [制作与排错](skills/ac7-gundam-mod/references/workflow.md) | 从静态替换到实机验收，含机体隐身、撕裂、姿态跳变、灯不亮、机库闪退的排查顺序 |
| [柯西 1.0 项目记录](skills/ac7-gundam-mod/references/xi-case-study.md) | 两轮开发实际做了什么，AC6 柯西和 AC7 强袭自由各用了什么，项目发起者亲手做了什么 |
| [Agent Skill](skills/ac7-gundam-mod/SKILL.md) | 可迁移到另一个替换项目，带本地输入检查、参考资料和验证步骤 |

## 给 Agent 安装

```sh
git clone https://github.com/MomoCyann/ac7-gundam-mod-workflow.git
```

克隆或下载本仓库后，把 `skills/ac7-gundam-mod/` **整个文件夹**放到你的 Agent 的技能目录，保持 `SKILL.md` 和 `references/` 的相对位置。Codex 可放到目标项目的 `.agents/skills/ac7-gundam-mod/`，其他支持 Agent Skills 的工具沿用各自的发现目录。[Codex 技能文档](https://developers.openai.com/codex/skills) · [Agent Skills 规范](https://agentskills.io/specification)

可以把下面这段任务交给 Agent。

> 使用 `$ac7-gundam-mod`。我想把手里的可授权机体替换成 AC7 的 X-02S。请先盘点本地工具和输入，确认一个可进机库、可进单人任务的静态基线，再给我一个可在 Blender 里调整的姿态文件。每个阶段都保留可回退的 PAK。

公开发布你自己制作的可玩包之前，请分别取得实际复用素材所需的授权。柯西案例中的原作者与复用范围见[项目记录](skills/ac7-gundam-mod/references/xi-case-study.md)。
