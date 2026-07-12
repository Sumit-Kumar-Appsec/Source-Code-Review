Code Review Strategies -

1. Linear / Sequential Code Review - Linear Code Review Strategy means reviewing the code in a fixed top-to-bottom sequence instead of following every function call immediately. You start with Function 1 and continue to Function 4, Function 5, and Function 8, following the green arrows. Even if Function 1 calls Function 2, you do not switch to it immediately. You stay on the main review path and review the called functions later when their turn comes in the sequence. The black arrows only show function calls or dependencies, while the green arrows define the order of the code review.

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

4. Pattern / Keyword Matching (Grep-based) Code Review - Using `grep`/`ripgrep` or IDE search to scan the entire codebase for dangerous keywords/patterns (e.g., `eval(`, `exec(`, `password`, `md5(`) — jumping straight to pattern matches without following function sequence or dependencies.
    
  A. Advantages:    
  - Very fast — scans thousands of lines in seconds
  - Known vulnerability patterns are found quickly
  - Scales well even on large codebases
  - Reusable checklist — applicable across multiple projects
  - Can start without understanding app logic first

  B. Disadvantages:
  - Shallow — no context (unclear if it's actually reachable/exploitable)
  - Only catches known patterns; misses new/logic-based bugs
  - False positives are common (matches safe code too)
  - False negatives (obfuscated/dynamic code may be missed)
  - Not standalone — manual verification still required afterward

  C. When to use:
  - Quick initial scan of a large, unfamiliar codebase
  - Hunting against a known checklist (OWASP/CWE)
  - Time-constrained engagements
  - Grep to find sinks, then confirm reachability via bottom-up tracing
  - Secrets/credential scanning

---

5. Feature-Based / Functionality-Driven - Instead of picking a random function or following code order, you pick a real-world feature/functionality(e.g., "Password reset") first, then trace which functions actually implement that feature (Function 5 → Password Reset → Function 3, 7). Review is organized around business functionality, not code structure.

  Advantages:
  - Maps directly to real user features — business-relevant findings
  - Easy to explain to non-technical stakeholders
  - Good for feature-by-feature test plans (login, signup, checkout)
  - Enables cross-app comparison of same feature
  - Naturally prioritizes sensitive features (auth, payments)
  
  B. Disadvantages:
  - Needs prior app familiarity to map functions to features
  - Unmapped functions can get skipped
  - Shared functions may get reviewed repeatedly
  - Time-consuming to build mapping in large/undocumented apps
  
  C. When to use this approach:
  - Structured pentests scoped by feature
  - Client specifically names a feature to test
  - Comparing same feature across multiple apps
  - Reporting to non-technical stakeholders
     
