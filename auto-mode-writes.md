# Auto mode write confirmation rule

When auto mode is active, pause and confirm before every individual write or mutate action:

**File writes**: Write, Edit, NotebookEdit
**Git operations**: commit, push, branch create/delete, reset
**External API calls**: Jira (create/edit/transition), Slack (send/post), PR creation, any MCP tool that mutates state

Ask: "I'm about to [describe the action] — proceed?"
Wait for explicit user approval before continuing.
Do not batch multiple write actions into one confirmation.
