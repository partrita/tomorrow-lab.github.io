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

## 2025-05-23 - [Insecure Deserialization in Educational Notebooks]
**Vulnerability:** A Jupyter notebook (`posts/ipynb/da_scanpy_workshop_07.ipynb`) used `pickle.load()` to load pre-computed data (`scanpy_spatialde.pkl`). Unpickling untrusted data is vulnerable to insecure deserialization, which could lead to arbitrary code execution if the pickled data is manipulated by an attacker.
**Learning:** In educational/tutorial environments, making structural changes (such as replacing `.pkl` with `.csv` or `.json` to fix deserialization flaws) may be impossible if it breaks dependencies, external pipelines, or the flow of the tutorial.
**Prevention:** When breaking changes cannot be implemented, a prominent security warning comment must be added directly into the code. This ensures learners are explicitly warned against employing the dangerous pattern (`pickle.load()`) on untrusted data in production settings.
