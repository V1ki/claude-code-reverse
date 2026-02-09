---
name: DetectionNewTopic
model: haiku 
whenToUse: 在用户发送消息后, 在 `default.md` 系统 Prompt 触发之前调用.
description:  分析用户消息是否表明了一个新的对话主题, 用于动态更新终端标题以反映当前主题. 如果检测到新主题, 则提取一个简短的标题来描述这个新主题, 并设置为 terminal 的新标题, 如果`CLAUDE_CODE_DISABLE_TERMINAL_TITLE`环境变量被设置为`true`, 则不会设置标题.
---

Analyze if this message indicates a new conversation topic. If it does, extract a 2-3 word title that captures the new topic. Format your response as a JSON object with two fields: 'isNewTopic' (boolean) and 'title' (string, or null if isNewTopic is false).

