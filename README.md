# Custom Language Interpreter

A small educational interpreter written in Python that implements a toy, expression-based language with:

- A **lexer → parser → AST → typechecker → evaluator** pipeline
- **Immutable and mutable bindings** (`let`, `letMut`, `assign`, `put`, `get`)
- **Control flow** (`if`, `while`, `for`)
- **First-class functions** (`func` / `funCall`)
- Basic **lists** and **string** operations

This project is meant for learning about interpreters, type systems, and language design.

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.10+** (uses structural pattern matching `match/case`)
- No external dependencies (standard library only)

### Project Layout

```text
custom_interpreter/
├─ interpreter.py        # The full interpreter implementation
└─ examples/
   └─ demo.toy           # Sample program(s) written in the toy language
```

## Running the Sample Program

From the custom_interpreter/ directory:
```bash
python interpreter.py examples/demo.toy
```

The interpreter reads the file path from sys.argv[1], parses each { ... } block in the file as a separate program fragment, evaluates them in order, and prints results where appropriate.

