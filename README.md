# Custom Language Interpreter

A small educational interpreter written in Python that implements a toy, expression-based language with:

- A **lexer → parser → AST → typechecker → evaluator** pipeline
- Immutable and mutable bindings (`let`, `letMut`, `assign`, `put`, `get`)
- Control flow (`if`, `while`, `for`)
- First-class functions (`func` / `funCall`)
- Basic **lists** and **string** operations

This project is meant for learning about interpreters, type systems, and language design.

---

## 🚀 Quick Start

### Requirements

- **Python 3.10+** (uses structural pattern matching `match/case`)
- No external dependencies (standard library only)

### Layout

```text
custom_interpreter/
├─ interpreter.py        # The interpreter implementation
└─ examples/
   └─ demo.toy           # Sample program(s) written in the toy language
```

### Run

From the `custom_interpreter/` directory:

```bash
python interpreter.py examples/demo.toy
```

The interpreter reads the file path from `sys.argv[1]`, parses each `{ ... }` block as a separate program fragment, evaluates them in order, and prints results where appropriate.

---

## 🧠 Language Overview

Each program fragment is inside `{ ... }`. A file can contain multiple fragments, evaluated independently.

Example (`examples/demo.toy`):

```text
{ printing "Hello from custom_interpreter!" end }

{ let x is 5 in x + x end }

{ func add(a, b) a + b , funCall add(15, 2) }

{ letMut L is lst [1, 2] in listappend 3 in L end }

{ slice "Hello, world!" start 0 stop 5 }
```

### Basic Expressions

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Comparisons: `<`, `>`, `=`
- Boolean: `and`, `or`, `not`
- Grouping: `(a + b) * c`

Examples:

```text
{ 1 + 2 * 3 }
{ (1 + 2) * 3 }
{ 10 < 20 }
{ not (1 = 2) }
```

### Bindings & Mutation

- Immutable: `let x is 5 in x + x end`
- Mutable: `letMut x is 2 in put x is x + 1 end`
- Low-level:
  - `assign v is e`
  - `get v`
  - `put v is e end`

Example:

```text
{ seq
    assign y is 10 ;
    put y is y + 5 end ;
    printing get y end
  end }
```

### Control Flow

**If / else**

```text
{ if 10 * 5 > 6 * 6
  then 10 * 5
  else 6 * 6
  end }
```

**While**

```text
{ letMut i is 0 in
    while i < 5 do
      put i is i + 1 end
    done
  end }
```

**For**

```text
{ letMut s is 0 in
    for i is 0 ;
        i < 5 ;
        put i is i + 1 end ;
        put s is s + i end
    end
  end }
```

### Functions

Functions are declared with `func` and called with `funCall`:

```text
{ func add(a, b) a + b , funCall add(15, 2) }
```

Recursive example (factorial sketch):

```text
{ func fact(n)
    if n = 1 then 1 else n * funCall fact(n - 1) end
  , funCall fact(5)
}
```

---

## 📚 Lists & Strings

### Lists

- Literal: `lst [expr1, expr2, ...]`
- Append: `listappend element in ListVar`
- Pop last: `popval(ListVar)`
- Length: `len ListVar`
- Indexing: `index ListVar [ indexExpr ]`
- Emptiness: `isEmpty ListVar`

Example:

```text
{ seq
    assign xs is lst [1, 2, 3] ;
    listappend 4 in xs ;
    printing index xs [2] end ;   # 3
    printing len xs end ;         # 4
    popval(xs) ;
    printing isEmpty xs end       # False
  end }
```

### Strings

- Length: `strlength("hello")`
- Reverse: `reversestr("abc")`
- Slice: `slice "Hello, world!" start 0 stop 5`
- Char at index: `stringidx("hello", 1)`
- Word count: `lenSen s`
- Vowel count: `vowelnumb(s)`

Example:

```text
{ seq
    printing strlength("abc") end ;
    printing reversestr("stressed") end ;
    printing slice "Hello, world!" start 0 stop 5 end ;
    printing stringidx("hello", 1) end ;
    printing vowelnumb("aeiouxyz") end
  end }
```

---

## 🧪 Tests

`interpreter.py` includes a number of `test_*` functions (`test_eval`, `test_let_eval`, `test_Letfun*`, `test_typecheck*`, etc.) that you can call manually from a Python REPL, or wire up to a test framework like `pytest` if desired.

Example in a REPL:

```python
from interpreter import test_eval, test_let_eval
test_eval()
test_let_eval()
```

---

## 🛠 Implementation Sketch

- `Stream` → character input
- `Lexer` → tokens (`Num`, `Bool`, `Keyword`, `Identifier`, `Operator`, `String`)
- `Parser` → AST (`NumLiteral`, `BinOp`, `Let`, `while_loop`, `LetFun`, etc.)
- `typecheck` → assigns simple types (`NumType`, `BoolType`, `StringType`, …)
- `eval` → evaluates AST using an `Environment` with scoped dictionaries

This makes it straightforward to extend the language or experiment with different semantics and type rules.
