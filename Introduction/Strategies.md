<img width="1649" height="954" alt="63d77c79-bfa2-4df1-b8e7-896751a05088" src="https://github.com/user-attachments/assets/b7959c0a-e983-4f34-b909-8dc358e20439" />Code Review Strategies -

1. Linear / Sequential Code Review - Linear Code Review Strategy means reviewing the code in a fixed top-to-bottom sequence instead of following every function call immediately. You start with Function 1 and continue to Function 4, Function 5, and Function 8, following the green arrows. Even if Function 1 calls Function 2, you do not switch to it immediately. You stay on the main review path and review the called functions later when their turn comes in the sequence. The black arrows only show function calls or dependencies, while the green arrows define the order of the code review.
- ![Uploading 63d77c79-bfa2-4df1-b8e7-896751a05088.png…]()

A. Advantages:
- Simple and beginner-friendly
- Nothing gets missed
- You build overall familiarity with the codebase
- No prior knowledge required

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

