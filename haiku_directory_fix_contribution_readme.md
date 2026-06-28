# Contribution [2]: [miniupnpc reports wrong path for $includeDir]

**Contribution Number:** [2]  
**Student:** [Keenan Kornegay]  
**Issue:** [[GitHub issue link](https://github.com/haikuports/haikuports/issues/13367)]  
**Status:** [Phase I]

---

## Why I Chose This Issue

I chose this issue because I'm interested in operating systems! Lately I've been getting more into the Linux operating system and am fascinated by its open source environment. Its community-driven efforts I find to be inspiring. While this is firmly not within the Linux ecosystem of operating systems, Haiku's philosophy appears to be similar. Plus I am curious to learn a bit more about how operating systems work underneath the system. 

---

## Understanding the Issue

### Problem Description

The MiniUPnPc library generated CMake package file exports the wrong path for INTERFACE_INCLUDE_DIRECTORIES on the Haiku operating system. Haiku expects a different path, so users are not able to import the MiniUPnPc library using CMake. 

### Expected Behavior

Users should be able to import the MiniUPnPc library and should report the correct path for $includeDir.

### Current Behavior

The MiniUPnPc library is not imported and the correct path is reported. 

### Affected Components

### Affected Components

- `libminiupnpc-shared.cmake` – exports the incorrect `INTERFACE_INCLUDE_DIRECTORIES` value.
- The CMake installation/export configuration for MiniUPnPc.
- Haiku package integration, which expects headers to be available under the development headers directory.
---

## Reproduction Process

### Environment Setup

No issues setting up my local environment for this issue.

### Steps to Reproduce

1. Clone the Miniupnp git, build and install MiniUPnPc with CMake 
2. Inspect the generated 'libminiupnpc-shared.cmake' file.
3. Confirm that 'INTERFACE_INCLUDE_DIRECTORIES' contains '/boot/system/include/miniupnpc`.
4. Create a small CMake project that imports MiniUPnPc using `find_package(miniupnpc REQUIRED)`.
5. Run CMake and confirm that the import fails or reports the incorrect include directory.
5. The generated CMake package exports `INTERFACE_INCLUDE_DIRECTORIES` as `${_IMPORT_PREFIX}/include/miniupnpc;${_IMPORT_PREFIX}/include`. On Haiku, this resolves to an incorrect include path, causing `find_package(miniupnpc)` consumers to fail to locate the library headers.

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** []
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
