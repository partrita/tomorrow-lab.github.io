## 2024-04-06 - [Command Injection via eval() in Sample Code]
**Vulnerability:** A Python sample code block demonstrating how to solve a coding puzzle in `posts/qmd/learn_coding_basic.qmd` utilized the built-in `eval()` function to parse mathematical strings.
**Learning:** Even within static documentation and sample code, insecure patterns like `eval()` can act as attack vectors if readers copy-paste the snippet into production systems that handle untrusted input.
**Prevention:** Always replace `eval()`, `exec()`, and similar dynamic execution mechanisms with safe, deterministic string parsing or abstract syntax tree (AST) evaluation, particularly in reference materials designed to teach beginners.
