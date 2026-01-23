---
name: AskUserQuestion
---

Use this tool when you need to ask the user questions during execution. This allows you to:
1. Gather user preferences or requirements
2. Clarify ambiguous instructions
3. Get decisions on implementation choices as you work
4. Offer choices to the user about what direction to take.

Usage notes:
- Users will always be able to select "Other" to provide custom text input
- Use multiSelect: true to allow multiple answers to be selected for a question
- If you recommend a specific option, make that the first option in the list and add "(Recommended)" at the end of the label

Plan mode note: In plan mode, use this tool to clarify requirements or choose between approaches BEFORE finalizing your plan. Do NOT use this tool to ask "Is my plan ready?" or "Should I proceed?" - use ExitPlanMode for plan approval.



--- 
# Tool Params

- questions: array<object>, Questions to ask the user (1-4 questions), required, minItems: 1, maxItems: 4
  - question: string, The complete question to ask the user. Should be clear, specific, and end with a question mark. Example: "Which library should we use for date formatting?" If multiSelect is true, phrase it accordingly, e.g. "Which features do you want to enable?", required
  - header: string, Very short label displayed as a chip/tag (max 12 chars). Examples: "Auth method", "Library", "Approach"., required
  - options: array<object>, The available choices for this question. Must have 2-4 options. Each option should be a distinct, mutually exclusive choice (unless multiSelect is enabled). There should be no 'Other' option, that will be provided automatically., required
    - label: string, The display text for this option that the user will see and select. Should be concise (1-5 words) and clearly describe the choice., required
    - description: string, Explanation of what this option means or what will happen if chosen. Useful for providing context about trade-offs or implications., required
  - multiSelect: boolean, Set to true to allow the user to select multiple options instead of just one. Use when choices are not mutually exclusive., required
- answers: object, User answers collected by the permission component, optional
- metadata: object, Optional metadata for tracking and analytics purposes. Not displayed to user., optional
  - source: string, Optional identifier for the source of this question (e.g., "remember" for /remember command). Used for analytics tracking., optional