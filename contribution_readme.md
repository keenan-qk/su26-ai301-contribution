# Contribution [1]: [Setup CI to use Flight Recorder]

**Contribution Number:** [1]  
**Student:** [Keenan Kornegay]  
**Issue:** [Setup CI to use Flight Recorder]  
**Status:** [Phase III] [In Progress]

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

Since I was working from a fresh Linux Mint install, I had to first install Git, and then log into Github from terminal. 
Then I had to install some dependencies, Clang, including Cmake. I also had to clone the Ponyc repository to my machine. 

### Steps to Reproduce

1. I had to locate the existing runtime tracing uset test. 
2. Then I had to verify that the workflow matrix contained "directives: runtime_tracing".
3. I verified that the workflow invokes the build: 
    make configure arch=x86-64 config=debug use=${{ matrix.directives }}
    make build config=debug
    make test-ci-core config=debug usedebugger=lldb test_full_program_timeout=120

4. Confirmed that runtime_tracing is a valid Makefile use option
5. Built Pony with runtime tracing enabled:
    make libs
    make configure arch=x86-64 config=debug use=runtime_tracing
    make build config=debug
6. Ran the same test on a release version:
    make configure arch=x86-64 config=release use=runtime_tracing
    make build config=release
    make test-ci-core config=release usedebugger=lldb
7. The test ran successfully and I found that the flight recorder ran in:
    src/libponyrt/tracing/tracing.c
    src/libponyrt/tracing/tracing.h
    src/libponyrt/sched/start.c

    And the flight recorder is directed to:
      stdout  (--ponytracingoutput -)
      stderr  (--ponytracingoutput ~)
### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- <img width="819" height="190" alt="image" src="https://github.com/user-attachments/assets/4c099102-9f63-4dd1-a00e-65463c7bcbc5" />
<img width="496" height="189" alt="image" src="https://github.com/user-attachments/assets/ecc6ec82-90c4-49ff-93e5-c9e80925dba0" />

- **My findings:** I found that flight recorder is not a separate use= directive, and is in fact, a runtime tracing mode.

---

## Solution Approach

### Analysis

CI is organized around a runtime_tracing use test, while flight recorder functionality has not been integrated into the primary testing workflows. This means flight recorder diagnostics are being prevented from being collected during normal tests. 

### Proposed Solution

Flight recorder needs to be integrated into primary CI test paths, runtime_tracing use test needs to be replaced with a vanilla use test, and surface tracing diagnostics need to be added through CI logs. 

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** The current CI configuration validates runtime tracing through a dedicated runtime_tracing use test. Flight recorder functionality has been added to runtime tracing, and the goal is to exercise flight recorder in the primary PR and stress-test workflows while preserving compilation coverage through a new vanilla use test.

**Match:** Investigated .github/workflows/ponyc-weekly-checks.yml and found the existing runtime_tracing use test.

Investigated Makefile and confirmed runtime_tracing is enabled through use=runtime_tracing.

Investigated src/libponyrt/sched/start.c and src/libponyrt/tracing/tracing.c and found that flight recorder is enabled through:

**Plan:** [Step-by-step implementation plan]
1. Identify the primary PR test workflows and stress-test workflows that should run with runtime tracing enabled.
2. Configure those workflows to run with flight recorder mode enabled and direct output to CI logs.
3. Remove the dedicated runtime_tracing use test from ponyc-weekly-checks.yml.
4. Add a vanilla use test that performs compilation without any use= directives.
5. Verify that debugger-based test execution remains unchanged. 
6. Run relevant CI tests and validate flight recorder output appears in logs. 

**Implement:** [[Link to your branch/commits as you work](https://github.com/ponylang/ponyc/compare/main...keenan-qk:ponyc-su26-ai301:issue-4702-flight-recorder)]
https://github.com/keenan-qk/ponyc-su26-ai301/tree/issue-4702-flight-recorder

Flight recorder enabled in intended CI workflows.
 Runtime tracing functionality still exercised.
 Dedicated runtime_tracing use test removed.
 Vanilla use test added.
 Debugger-based tests unchanged.
 CI configuration follows project conventions.

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?] 

**Evaluate:** [How will you verify it works?] Run the existing runtime tracing build locally. Verify that the new vanilla test compiles successfully without any use= directives. 

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

### Week [3] Progress

## Reproduction 

- Successfully reproduced the issue locally using: make test-ci-core config=debug usedebugger=lldb test_full_program_timeout=120.

## Investigation 

- Traced how usedebugger=lldb is passed from the Makefile into the full-program test runner
- Located LLDB-specific exit code handling in test/full-program-runner/_tester.pony. 
- Examined LLDB output:
    - stop reason = breakpoint 1.1
    - Process ... exited with status = 0
 
## Candidate Fix

- Updated LLDB fallback logic to look for stop reason = signal instead of any occurance of stop reason =
- Rationale: breakpoint stops should not necessarily be treated as test failures, while signal-based stops may indicate an actual error condition. 

## Current Status 

- Fix committed and pushed to the issue branch
- Need additional validation to confirm the change fully resolves the issue and does not introduce regressions.


### Week [4] Progress

## Course Correction 

I had made an error during my initial investigation of Issue #4702, as I had focused on reproducing the LLDB behavior and documenting the failure in the CI test environment. I had not realized until further analysis that I had spent the previous week validating the issue rather than implementing a fix. 

Thus, I shifted from a process of reproduction to resolution. I analyzed the LLDB stop-reason handling logic in the flight recorder CI tests and identified that signal-related stop events were treated too broadly. I implemented a code change to handle LLDB signal stop reasons more precisely, allowing the test runner to distinguish between expected child-process signals and actual interruptions. 

After making this change, I committed the update to my feature branch and began validating the fix using the project's CI test suite. This work moved the contribution from issue reproduction toward an actual code-level solution for the flight recorder testing issue. 

## Candidate Fix 

- I revisted the LLDB stop-reason handling logic in  test/full-program-running/_tester.pony
- Updated the fallback logic to more precisely distinguish signal-based stosp from other LLDB stop reasons
- Committed the implemenation to the issue-4702-flight-recorder branch
- Continued validiation using the projects test suite

## Current Status

- Fix has been implemented and pushed to the issue branch.
- Running additional tests to verify that the change resolves the Flight Recorder CI issue without introducing regressions.
- Preparing evidence and documentation for final submission once validation is complete. 

### Code Changes

- **Files modified:** [[List]](https://github.com/keenan-qk/ponyc-su26-ai301/commit/709466af2c8911c8194c9daba08e229035837a09)
- **Key commits:** [[Links to important commits]](https://github.com/keenan-qk/ponyc-su26-ai301/actions/runs/27888653777)
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

I learned about continuous integration tests and how they are implemented. I also learned a bit more about the open-source economy and how to contribute to an open-source issue. 

### Challenges Overcome

One challenge was identifying exactly what the issue was asking for. I had to re-read the issue and parse through it to really understand the steps I had to take to solve this issue. I had also made an error in confusing the scope of the issue. While focusing on the LLDB did help me in the end, it did not mean the issue was solved. 

### What I'd Do Differently Next Time

I would further self-verify what the issue was asking for. 

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
