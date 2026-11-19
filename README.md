# GPT-6.2 Prompt Rewriter

A Codex skill that rewrites existing prompts for GPT-6.2 Astra and Sol using [OpenAI’s prompting guidance](https://developers.openai.com/api/docs/guides/latest-model#prompting-best-practices). It keeps your goal, exact requirements, and approval boundaries in the rewritten prompt.

Install in your project, then invoke `$gpt-6-2-prompt-rewriter` in Codex and paste a prompt. You can name Astra or Sol as the target.

```sh
npx skills add https://api.github.com/repos/Y1fe1Zh0u/gpt-6-2-prompt-rewriter/tarball/main --agent codex --yes
```

一个面向 **GPT‑6.2 Astra 和 GPT‑6.2 Sol** 的 Codex Skill。给它一条已有 prompt，它会返回一版可直接复制的改写，并在需要时解释哪些修改会改变模型行为。默认做最小必要修改，保留原任务、输出格式和明确的权限边界。

可指定 `gpt-6.2-astra` 或 `gpt-6.2-sol`。省略型号时，Skill 会应用官方指南中两个模型共用的提示建议。 [官方提示词指南](https://developers.openai.com/api/docs/guides/latest-model#prompting-best-practices)

## 安装和使用

下载或克隆项目后，在项目根目录运行以下命令。

```sh
mkdir -p "$HOME/.agents/skills/gpt-6-2-prompt-rewriter"
cp -i SKILL.md LICENSE "$HOME/.agents/skills/gpt-6-2-prompt-rewriter/"
```

已有同名 Skill 时，在原安装目录更新文件。在 Codex 对话里输入 `$gpt-6-2-prompt-rewriter`，然后贴上原 prompt。可以同时指定目标型号、说明哪些约束必须保留，或要求只返回改写稿。Skill 会使用当前会话改写，目标型号用于指导输出。 [Codex Skill 安装位置](https://learn.chatgpt.com/docs/build-skills)

## 改写前后

以下示例展示预期改写方式，由项目手工编写。

### 写作

**原 prompt**

> 把这些发布笔记写成给老客户的邮件，专业、简洁，200 字以内。

**改写后**

> 根据发布笔记给老客户写一封专业、简洁的邮件，200 字以内。开头说清这次更新对他们有什么影响，再补充理解变化所需的背景。沿用发布笔记里的产品术语。

保留受众、语气和字数限制，把邮件开头应交代的信息写清楚。

### Agent 执行

**原 prompt**

> 根据录屏修复设置页保存失败，拿不准就问我，修完汇报。

**改写后**

> 根据录屏定位并修复设置页保存失败。可逆的实现选择可以按合理假设继续，交付时说明影响结果的假设。需要改变产品行为或超出授权范围时先确认。修复后验证录屏里的操作可以成功保存，并简短汇报修复内容和验证结果。

用录屏里的操作定义完成条件，明确什么时候可以继续、什么时候应等待用户决策。原文如果要求每次改文件前都确认，这条要求应保留。

### 代码

**原 prompt**

> 合并这两处重复的日期格式化逻辑，保持兼容，代码写干净。

**改写后**

> 合并两处重复的日期格式化逻辑，优先复用能满足要求的现有日期工具。保留两个调用方的输入处理和输出格式。用适用的现有用例检查这些行为，按项目要求或在现有用例会漏掉具体回归时补充覆盖。

把兼容性落实到调用方可观察的行为，复用和补测都按实际代码与项目约定决定。

### 前端

**原 prompt**

> 按参考图把设置页做得更清爽，字体用 Inter，保持功能。

**改写后**

> 将现有设置页的字体调整为 Inter，按参考图调整布局和视觉层级。保留现有操作、必要标签和状态反馈，删掉重复标题和说明。渲染页面后对照参考图检查。

把视觉偏好落实到参考图、字体和界面层级，并把现有操作和反馈带入验收。

## Responses API

当输入包含 Responses API 请求并要求优化缓存时，Skill 会建议把新会话的稳定指令放到 `input` 的第一条 `developer` 消息里，把当次请求的指导留在顶层 `instructions`。 [官方迁移指南](https://developers.openai.com/api/docs/guides/latest-model#migration-quickstart)

## 许可

[MIT](LICENSE)
