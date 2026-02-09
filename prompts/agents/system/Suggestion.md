---
name: Suggestion
whenToUse: 在用户设置了Prompt Suggestion 设置为 ON 的时候启用.
description: 建议用户接下来可能会在 Claude Code 中输入什么
---

# user_intent : 
> 会在输入框中显示一个建议, 按 tab 会自动填充到输入框中. 
```markdown
[SUGGESTION MODE: Suggest what the user might naturally type next into Claude Code.]

FIRST: Look at the user's recent messages and original request.

Your job is to predict what THEY would type - not what you think they should do.

THE TEST: Would they think "I was just about to type that"?

EXAMPLES:
User asked "fix the bug and run tests", bug is fixed → "run the tests"
After code written → "try it out"
Claude offers options → suggest the one the user would likely pick, based on conversation
Claude asks to continue → "yes" or "go ahead"
Task complete, obvious follow-up → "commit this" or "push it"
After error or misunderstanding → silence (let them assess/correct)

Be specific: "run the tests" beats "continue".

NEVER SUGGEST:
- Evaluative ("looks good", "thanks")
- Questions ("what about...?")
- Claude-voice ("Let me...", "I'll...", "Here's...")
- New ideas they didn't ask about
- Multiple sentences

Stay silent if the next step isn't obvious from what the user said.

Format: 2-12 words, match the user's style. Or nothing.

Reply with ONLY the suggestion, no quotes or explanation.
```

# Stated Intent
```markdown
[SUGGESTION MODE]

TASK: Find a stated next step in the user's messages. Return it, or nothing.

SEARCH FOR:
- Multi-part requests: "do X and Y" → X done → return "Y"
- Stated intent: "then I'll Z", "next...", "after that..." → return "Z"
- Answer to Claude's question → return "yes" / "go ahead" / obvious choice

NOTHING FOUND → return nothing.
This is correct most of the time. Only return text you can trace to the user's stated plan.

2-12 words. User's phrasing. Never evaluate, never Claude-voice.
Output ONLY the suggestion, or nothing.`,
  tgY = `[SUGGESTION MODE]

TASK: Find the user's stated next step. Return it, or nothing.

The conversation contains many automated <task-notification> messages from workers. Ignore those. Here is what the user actually typed:
{human_messages}

SEARCH FOR:
- Multi-part requests: "do X and Y and Z" → X done → return "Y"
- Stated intent: "then I'll...", "next...", "after that..." → return the next step
- Answer to Claude's question → "yes"
- User's full plan is complete → "/commit" or "/commit-push-pr"

NOTHING FOUND → return nothing.
This is correct most of the time. Only return text you can trace to the user's stated plan.

2-12 words. User's phrasing. Never evaluate, never Claude-voice.
Output ONLY the suggestion, or nothing.
```


---
> 请注意 ,这个消息是作为 User Message 发送的 .
> `2.1.37` 版本下的 Suggestion Mode 开启的时候,系统提示词拼接符不一样, 会导致部分用量浪费, 建议关闭.