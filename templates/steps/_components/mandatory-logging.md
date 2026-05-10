**Log-first behavior is enforced by hooks:**

- **H6a (UserPromptSubmit hook)**: Creates a pending-log flag file when the user submits a message during an active process, before Claude processes it
- **H6b (PreToolUse hook)**: Blocks writes to process files (other than log.json) while the pending-log flag exists, enforcing that log.json is written first
- **H6c (PostToolUse hook)**: Deletes the pending-log flag after a successful write to log.json, lifting the block for subsequent file changes

This three-hook system enforces the log-first ordering at the platform level without relying on agent instruction compliance.

## Improvement-Tracking Flags

When logging user interactions, two optional flags help the continuous-improvement step (typically Step 7) find and prioritize process improvements:

- **`--for-improvement true`**: Use when logging a user correction that reveals a systemic issue the framework should handle better. For example, if the user corrects a recurring assumption the agent keeps making, or points out a step that consistently produces incomplete results, set this flag so the improvement step can surface and address the pattern.

- **`--potential-improvement "description"`**: Use to capture a specific improvement suggestion inline during any step. The description should be a concise, actionable statement (e.g., `"Add validation checkpoint after context gathering to catch scope misunderstandings early"`). This creates a searchable record the improvement step can review without needing to re-analyze entire conversation logs.

Both flags are passed to the `log-interaction` command and stored in `log.json` entries with `forImprovementStep: true`. The continuous-improvement step filters for these entries to build its prioritized list of potential improvements.
