---
name: pr-feedback
description: Fetch and assess review comments on the current PR. Evaluates each comment for validity, checks code context, and provides a summary. Use when the user wants to review PR feedback, assess Copilot comments, or check review comments on a pull request.
---

# PR Feedback Assessment

Fetch review comments for the current PR, assess each for validity, and summarize findings. **Do not fix anything without explicit user approval.**

## Workflow

### Step 1: Identify the PR

```bash
# Get PR number from current branch
gh pr view --json number --jq '.number'
```

If no PR exists, inform the user.

### Step 2: Fetch Review Comments

```bash
# Get all reviews with comments
gh api repos/{owner}/{repo}/pulls/{pr_number}/reviews \
  --jq '.[] | select(.body != "" or .state == "CHANGES_REQUESTED") | {id: .id, user: .user.login, state: .state, body: .body, submitted_at: .submitted_at}'

# Get detailed comments from each review
gh api repos/{owner}/{repo}/pulls/{pr_number}/reviews/{review_id}/comments \
  --jq '.[] | {id: .id, path: .path, line: .line, body: .body}'
```

### Step 3: Assess Each Comment

For each comment:

1. **Read the relevant code** at the file path and line number mentioned
2. **Understand the context** - what does the code do, what's the intent?
3. **Evaluate the comment's merit**:
   - Is the concern valid?
   - Is it already addressed elsewhere?
   - Is it a matter of preference vs correctness?
   - Does it identify a real bug/inconsistency?

### Step 4: Provide Summary

Present findings in this format:

```markdown
## PR Feedback Assessment

### Summary
- **Total comments**: X
- **Valid concerns**: Y  
- **Already addressed**: Z
- **Skipped/Not applicable**: W

### Detailed Findings

| # | File | Concern | Assessment | Action |
|---|------|---------|------------|--------|
| 1 | `path/file.tsx` | [Brief description] | ✅ Valid / ⏭️ Skip / ✓ Already fixed | [Recommended action or reason for skip] |

### Comments Requiring Action

#### Comment 1: [Brief title]
- **File**: `path/to/file.tsx:42`
- **Concern**: [What the reviewer flagged]
- **Assessment**: [Your analysis of validity]
- **Suggested fix**: [What would address this]

### Skipped Comments

#### Comment N: [Brief title]
- **Reason**: [Why this was skipped - e.g., "Already addressed in commit abc123", "Intentional design choice", "Outdated - code has changed"]
```

## Assessment Criteria

When evaluating comments, consider:

| Criterion | Valid Concern | Skip/Dismiss |
|-----------|--------------|--------------|
| **Bug/Error** | Code has actual defect | False positive, code is correct |
| **Inconsistency** | Real inconsistency exists | Intentional difference with reason |
| **Missing code** | Something is genuinely missing | Already exists elsewhere |
| **Style/Preference** | Violates project standards | Subjective preference |
| **Outdated** | Still applies to current code | Code has changed since comment |

## Important Rules

1. **Read before assessing** - Always read the actual code before evaluating a comment
2. **Check recent commits** - Comment may reference old code that's been fixed
3. **No automatic fixes** - Present findings only; wait for user to approve fixes
4. **Be objective** - Acknowledge valid concerns even if they require work
5. **Provide context** - Explain why something is valid or why it should be skipped

## Example Assessment

> **Copilot comment**: "The `height` prop has no default value, which could cause undefined to be passed..."
>
> **Assessment**: ✅ Valid - Checked `bar-chart.tsx` which has `height = 400` default, but `line-chart.tsx` has no default. This is an inconsistency.
>
> **Suggested fix**: Add `height = 400` default to LineChart for consistency.

## After Assessment

Ask the user:
- "Would you like me to fix the valid concerns?"
- "Should I reply to any skipped comments explaining why?"
