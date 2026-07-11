Code Review Strategies -

1. Linear / Sequential Code Review - Linear Code Review Strategy means reviewing the code in a fixed top-to-bottom sequence instead of following every function call immediately. You start with Function 1 and continue to Function 4, Function 5, and Function 8, following the green arrows. Even if Function 1 calls Function 2, you do not switch to it immediately. You stay on the main review path and review the called functions later when their turn comes in the sequence. The black arrows only show function calls or dependencies, while the green arrows define the order of the code review.
<img width="1649" height="954" alt="63d77c79-bfa2-4df1-b8e7-896751a05088" src="https://github.com/user-attachments/assets/1603ac34-285c-460c-b32d-29ef3c69ee3d" />

A. Advantages:
- Simple and beginner-friendly
- Nothing gets missed
- You build overall familiarity with the codebase
- No prior knowledge required
- Coverage - Pretty good

B. Disadvantages:
- Time-inefficient
- Lack of prioritization
- Impractical for large codebases
- Missing context
- Slower vulnerability discovery

C. When to use this approach:
- When the codebase is small (e.g., 10-20 functions, like in your diagram)
- When you're new to a codebase and haven't built a mental model of it yet
- For learning/practice purposes
- When there's no time constraint
- When the codebase is poorly documented and there isn't time to build a dependency graph first

---

2. Top to Bottom -

A. Advantages:
- Deep understanding of one complete flow
- Mirrors real attacker path (entry → impact)
- Great for testing one specific feature
- No context loss — you stay in one logical thread
- Fast results for short chains

B. Disadvantages:
- Narrow coverage — other functions stay untouched
- Tunnel vision — may miss critical bugs outside the chain
- Needs tracking if covering multiple chains
- Shared functions may get re-analyzed repeatedly

C. When to use this approach:
- Testing one specific feature (login, password reset)
- Targeted pentest / bug bounty, not full app
- Building an exploit chain (attacker mindset)
- Time-limited engagements needing depth over breadth
- When visually tracking coverage per flow

---

3. Bottom-to-Top **-** Start from a critical/sensitive function (a "sink" — e.g., a function that writes to DB, executes a command) and trace **backward** to find all functions that call into it, up to the original input source. Opposite of top-to-bottom.

A. Advantages:
- Directly finds what reaches a dangerous/sensitive point
- Efficient for vulnerability hunting — focuses on impact first, not noise
- Reveals all entry points feeding a risky function (useful for input validation checks)
- Good for confirming if user input can actually reach a sink (real exploitability)

B. Disadvantages:
- Needs a known "target" sink to start from — not useful if you don't know what's risky yet
- Misses functions unrelated to that sink
- Can be confusing in large codebases with many indirect callers
- Requires good call-graph/IDE tooling to trace backward efficiently

C. When to use:
- When you already found a **dangerous sink** (e.g., `eval()`, SQL query, file write) and want to check if it's reachable from user input
- Vulnerability ****confirmation — proving exploitability by tracing back to source
- Reverse engineering / audit of a specific risky function
- When doing taint ****analysis manually

---

