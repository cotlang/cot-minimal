# cot-minimal

A minimal self-hosted Cot compiler - Track B of the parallel bootstrap race.

## Mission

Build a self-hosted compiler using only "Minimal Cot" - a simplified subset of the language.

## Rules

1. **Only use Minimal Cot features** (see spec below)
2. Can reference `~/cotlang/cot/src/` for algorithms
3. Must write fresh implementation
4. Goal: compile itself

## Minimal Cot Specification

### INCLUDED

```
Types:        i64, u64, u8, u16, u32, bool, string, void
Structs:      struct { field: Type }  (NO methods)
Enums:        enum { A, B, C }        (simple, no data)
Arrays:       [N]T, []T
Pointers:     *T, &value, ptr.*
Functions:    fn name(arg: Type) ReturnType { }
Control:      if/else, while, for, return, break, continue
Variables:    var, const
Operators:    + - * / % == != < > <= >= and or not
```

### EXCLUDED

```
- Generics           (fn foo<T>)
- Traits             (trait X { })
- Closures           (|| { })
- String interp      ("${x}")
- Comptime           (comptime { })
- Optionals          (?T)
- Try/catch/throw
- Methods
- Switch expressions
- Defer
```

## Directory Structure

```
cot-minimal/
├── src/
│   ├── main.cot       # Entry point
│   ├── lexer.cot      # Tokenization
│   ├── token.cot      # Token types
│   ├── parser.cot     # Parsing
│   ├── ast.cot        # AST nodes
│   ├── ir.cot         # IR types
│   ├── lower.cot      # AST → IR
│   ├── emit.cot       # IR → Bytecode
│   └── util.cot       # Lists, helpers
└── test/
    └── hello.cot      # Test file
```

## Success Criteria

```bash
# Zig compiler compiles cot-minimal
cot compile src/main.cot -o cot-minimal.cbo

# cot-minimal compiles itself
cot run cot-minimal.cbo -- compile src/main.cot -o cot-minimal2.cbo

# Verify it works
cot run cot-minimal2.cbo -- compile test/hello.cot -o hello.cbo
cot run hello.cbo
```

## Progress

| Component | Status | Notes |
|-----------|--------|-------|
| token.cot | Not started | |
| lexer.cot | Not started | |
| ast.cot | Not started | |
| parser.cot | Not started | |
| ir.cot | Not started | |
| lower.cot | Not started | |
| emit.cot | Not started | |
| main.cot | Not started | |

## See Also

- `~/cotlang/comparison/11-parallel-bootstrap-race.md` - Race rules and context
- `~/cotlang/cot/src/` - Reference implementation (Zig)
- `~/cotlang/cot-compiler/` - Track A (do not touch!)
