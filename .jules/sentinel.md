## 2024-11-22 - [Remove Dangerous eval() from Code Examples]
**Vulnerability:** The codebase contained an example solution in `posts/qmd/learn_coding_basic.qmd` using Python's `eval()` to parse math equations. This allowed arbitrary code execution and taught poor security practices to readers of the blog.
**Learning:** Even in educational/tutorial contexts, unsafe functions like `eval()` should never be used, as users copy-paste examples into production code without understanding the risks. Using language features intended for metaprogramming or dynamic evaluation for simple tasks like string parsing introduces severe vulnerabilities.
**Prevention:** Always use safe parsing mechanisms or built-in secure alternatives to evaluate mathematical expressions or parse strings. Avoid `eval()`, `exec()`, or similar functions when untrusted input might be processed.

## 2024-11-23 - [Remove Hardcoded Secrets in Tutorials]
**Vulnerability:** A hardcoded Hugging Face API token (`hf_cTKyTsXtqSHWyXPLAuxSIGECiIctuNsBona`) was exposed in `posts/qmd/learn_pythonCoding.qmd`. Although it was inside a tutorial Markdown file, any secret checked into version control is a critical risk because automated scrapers and malicious actors can harvest it.
**Learning:** Hardcoded secrets can easily slip into documentation, tutorials, and examples. Developers often copy-paste functional code into guides without sanitizing credentials. These pose the exact same risk as secrets hardcoded in source files.
**Prevention:** Regularly scan not just source code but also `.md`, `.qmd`, `.ipynb`, and other documentation formats for credential patterns (e.g., `hf_...`, `sk_...`). Always use explicit placeholders like `<YOUR_TOKEN_HERE>` or `hf_your_token_here_xxxxxxxxx` in tutorials instead of real or realistic-looking tokens.

## 2025-05-20 - [Remove Dangerous eval() from Pedigree Parsing]
**Vulnerability:** The code example in `posts/qmd/Rosalind_stronghold.qmd` previously parsed Newick-formatted pedigree strings by performing string replacements (turning parentheses into function calls) and passing the result to `eval()`. This executes arbitrary Python code and could allow remote code execution if evaluating untrusted inputs.
**Learning:** Even when the input strings look like purely structural text (e.g. Newick format `(((Aa,aa)...`), transforming them into executable expressions and using `eval()` is incredibly dangerous and teaches poor security practices.
**Prevention:** Always use safe serialization/deserialization methods like `ast.literal_eval`, `json.loads`, or a dedicated parsing library instead of `eval()` or `exec()`.

## 2026-04-18 - [Add Warning for Insecure Deserialization in Educational Notebooks]
**Vulnerability:** The educational notebook `posts/ipynb/da_scanpy_workshop_07.ipynb` used `pickle.load()` to deserialize an external `.pkl` file. This represents a critical insecure deserialization vulnerability that could lead to Remote Code Execution (RCE) if the `.pkl` file is compromised or replaced by a malicious actor.
**Learning:** In workshop or educational materials, it is sometimes impossible to replace dangerous file formats like `.pkl` with safer alternatives (e.g., `.csv` or `.json`) without breaking the tutorial's dependency on an external endpoint. However, teaching insecure patterns without context is dangerous as users often copy-paste code.
**Prevention:** When an unsafe API (like `pickle.load`) must be used due to upstream constraints in educational materials, prominently document the security risk with an inline warning directly in the code cell, advising users to only use it with trusted data sources.
