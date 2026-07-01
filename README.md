# Contribution [1]: [Feature] Clock with seconds

**Contribution Number:** [1]  
**Student:** [Samuel Perez]  
**Issue:** [https://github.com/nightscout/cgm-remote-monitor/issues/8048]
**Status:** [Phase I]

Nightscout currently lacks the ability to display seconds in the clock widget. A feature request was opened to add a SHOW_SECONDS environment variable as a browser-level setting, which would let users opt into showing seconds without affecting anyone who doesn't want it. A previous contributor submitted PR #8392 attempting this feature, but it was not merged due to four specific issues identified by the maintainer.
---

## Why I Chose This Issue

I chose this issue because the maintainer already reviewed and approved the overall direction of the feature, leaving a precise and actionable list of what needs to be corrected before it can be merged. Rather than guessing what to build or how to structure it, the feedback from the maintainer acts as a clear checklist.
---

## Understanding the Issue

### Problem Description

I need to add a feature that add seconds in a clock that has minutes and hours

### Expected Behavior

Display seconds live 

### Current Behavior

No seconds so it is a non-existent feature

### Affected Components

server route -> clocks.js
main template -> clock.html
config UI -> clock-config.html
Shared styles -> clock-shared.css
Config Styles -> clock-config.css
Client logic -> clock-client.js
Bundle entry -> bundle.clocks.source.js

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

