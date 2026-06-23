# Contribution [1]: [Bug] /crawl/job and /llm/job return HTTP 500 when jwt_enabled=true

**Contribution Number:** [1]  
**Student:** [Samuel Perez]  
**Issue:** [https://github.com/unclecode/crawl4ai/issues/2016]  
**Status:** [Phase I]

Crawl4ai currently has a bug where enabling JWT authentication (security.jwt_enabled: true in config.yml) breaks all four async job endpoints (POST /crawl/job, POST /llm/job, and their corresponding GET .../job/{task_id} status endpoints), causing them to return HTTP 500 Internal Server Error with AttributeError: 'Depends' object has no attribute 'credentials'. Meanwhile, all synchronous endpoints (/crawl, /md, /html, /screenshot, /pdf) work correctly with the same JWT. This matters because it makes the entire async task workflow unusable for any deployment that relies on JWT-based auth, forcing users to fall back to synchronous endpoints and lose the async/polling pattern.
---

## Why I Chose This Issue

I chose this issue because it has a clearly defined solution: replacing the Depends(lambda: _token_dep()) pattern with Depends(_token_dep) in deploy/docker/job.py (lines 59, 90, 100, 126), which lets FastAPI resolve the dependency normally instead of returning an unresolved Depends object. The root cause is already well-documented in the issue, with a working reference pattern from server.py to compare against, making it an ideal entry point into open source contribution.
---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]

