# Anti-AI-Writing

一个用于中文论文段落润色的 Codex Skill，也适用于研究报告、项目说明和一般文章。当研究内容已经写清了，但段落读起来像提纲，每句话都带着“首先”“此外”，真正想说的发现反而藏在最后时，可以用它来改。

把原文交给它，默认会得到一段可直接替换的修改稿。它会检查标点、列表、对比句、抽象词、段首铺垫、段末拔高、事实依据和句间衔接。有助于读者理解的术语、直接引语、文献标注和限定条件应当保留。

## 在 Codex 桌面版中使用

1. **取得文件。** 用有仓库访问权限的 GitHub 账号打开本页，选择 **Code → Download ZIP** 并解压。在解压的文件夹中找到 `skills/anti-ai-writing`，其中应有一份 `SKILL.md`。
2. **安装到个人目录。** 将整个 `anti-ai-writing` 文件夹复制到个人技能目录。在 Windows 文件资源管理器的地址栏输入 `%USERPROFILE%\.agents\skills`。在 macOS 的 Finder 中按 `⌘⇧G`，输入 `~/.agents/skills`。如果目录尚不存在，先创建相应文件夹。安装后的最后三级应为 `skills/anti-ai-writing/SKILL.md`。
3. **开始润色。** 在 Codex 桌面版打开论文所在的项目，新建对话。把下面这句和要修改的段落一起发出。如果手稿已在项目里，也可以写明文件名和小节，只请它在对话中给出修改稿。

> 请用 anti-ai-writing 润色下面这段论文讨论。读者是本领域同行。保留引文、效应方向和不确定性，只返回修改后的段落。

如果需要审阅修改，可以追加一句“请在正文后指出哪两处改动可能影响读者对结果的理解”。如果 Codex 没有找到 Skill，先确认解压后没有多套一层同名文件夹，再重启应用并开一个新对话。

## 在 Codex CLI 中使用

需要本机已安装 Git 和 Codex CLI，并已登录 Codex。克隆私有仓库还需要相应的 GitHub 读取权限。桌面版和 CLI 如果在同一系统用户下使用，个人目录中的 Skill 安装一次即可。如果 CLI 运行在 WSL 或远程机器上，请在那个环境中安装。

**首次安装。** 在准备存放仓库的目录中执行对应命令。已有本地克隆时，跳过 `git clone`，从克隆所在的父目录执行复制命令。

Windows PowerShell

```powershell
git clone https://github.com/AphroditeYu/Anti-AI-Writing.git
$skillDir = Join-Path $env:USERPROFILE '.agents/skills/anti-ai-writing'
New-Item -ItemType Directory -Force -Path $skillDir | Out-Null
Copy-Item -LiteralPath './Anti-AI-Writing/skills/anti-ai-writing/SKILL.md' -Destination $skillDir
Test-Path -LiteralPath (Join-Path $skillDir 'SKILL.md')
```

macOS、Linux 或 WSL

```bash
git clone https://github.com/AphroditeYu/Anti-AI-Writing.git
mkdir -p "$HOME/.agents/skills/anti-ai-writing"
cp ./Anti-AI-Writing/skills/anti-ai-writing/SKILL.md "$HOME/.agents/skills/anti-ai-writing/"
ls "$HOME/.agents/skills/anti-ai-writing/SKILL.md"
```

**交互式润色。** 进入论文所在目录，运行 `codex`。在 Codex 会话中输入 `/skills` 查看可用技能，再输入以下请求。将其中的文件名和小节换成自己的，也可以直接粘贴原文。

```text
用 $anti-ai-writing 润色 discussion.md 中“研究局限”的第一段。
保留样本范围、引文和作者语气。在对话里给我修改稿，保留磁盘上的原稿。
```

**将结果保存为单独文件。** 如果当前目录下已有 `discussion.md`，以下命令可以在普通论文文件夹中直接运行。先选一个尚未使用的输出文件名。命令里的单引号会把 `$anti-ai-writing` 原样传给 Codex。

```bash
codex exec --skip-git-repo-check --sandbox read-only -o discussion-edited.md '用 $anti-ai-writing 润色当前目录的 discussion.md。保留小标题、引文、数字和限定条件。最终答复只给完整修改稿。'
```

## 一组论文段落对照

**原稿**

> 在学习平台使用与成绩表现的关系方面，本文进行了多维度分析。首先，在本课程样本中，平台登录次数与期末成绩呈正相关。此外，值得注意的是，本文未记录学生的课外学习时间。因此，对于上述相关关系的解释需要保持谨慎。

**润色稿**

> 在本课程样本中，平台登录次数与期末成绩呈正相关。研究未记录学生的课外学习时间，因此难以判断这部分学习投入能否解释观察到的关联。

这是为说明编辑方法编写的例句。改稿把样本和结果放在首句，随后交代缺少哪项观测，以及它如何限制解释。“本课程样本”和“正相关”都保留下来。读者据此能区分作者观察到的关系与尚待检验的解释。

## 怎样给出更具体的要求

| 你在改什么 | 可以补充的要求 |
| --- | --- |
| 方法段 | 保留变量的操作性定义、单位和步骤顺序，让同行看完能复述做法。 |
| 结果段 | 保留数值、分母、效应方向和表号。把观察结果放在前面。 |
| 讨论段 | 保留竞争解释和局限。原文用了“可能”的地方，维持同等程度的不确定性。 |
| 项目说明 | 读者第一次接触这个项目。保留必要术语，用原文已有的例子解释做法。 |

用于投稿时，可把目标期刊的体例要求和一小段自己的定稿随原文附上，分别用来约束格式和语气。交稿前逐一核对改稿中的数值、引文归属和研究范围。
