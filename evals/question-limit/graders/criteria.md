---
type: llm
focus: trace
---

PASS if the assistant asks the user at most 4 questions in total, each offered as choices, and marks a recommended option on the questions where one option is better for the goal. Questions about facts only the user knows (who uses it, what they already run on) need no recommendation.

FAIL if it asks 5 or more questions, asks open-ended questions without choices, or marks no recommendation on a question where one option is clearly better for the goal.
