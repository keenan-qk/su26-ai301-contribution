# Contribution [1]: [Setup CI to use Flight Recorder]

**Contribution Number:** [1]  
**Student:** [Keenan Kornegay]  
**Issue:** [Setup CI to use Flight Recorder]  
**Status:** [Phase I] [Complete]

---

## Why I Chose This Issue

This issue interests me, because I've been interested in brushing up on my C and C++ to contribute to real world issues. Unit testing and continuous integration testing is also something that I wanted to learn more about. By now, I have enough experience in Github, C, and C++ to tackle an issue such as this. I'm also eager to learn more about the Github ecosystem, how to contribute to open source projects, and to learn more about CI testing. In addition, as someone who has always an interest in aviation and flight, I like the 'flight recorder' analogy.

---

## Understanding the Issue

### Problem Description

Currently, for pull requests there does not exist a simple "vanilla" use test. I.e., a compile test that checks to see if Pony simply compliles successfully. In addition, we want flight recorder, a runtime test that logs events, to be used for all stress tests. Flight recorder should also be output in stdout or stderr. 

### Expected Behavior

When we run a program in Pony, there should be a message in stdout or stderr that our program ran successfully.

### Current Behavior

Currently, there are use tests to check for various things but nothing for a vanilla, "It runs." 

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
