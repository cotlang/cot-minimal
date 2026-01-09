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

| Component | Status | Lines | Notes |
|-----------|--------|-------|-------|
| token.cot | Done | 177 | Token types enum, keyword lookup |
| util.cot | Done | 298 | Result type, lists, string/byte helpers |
| lexer.cot | Done | 353 | Single-pass tokenizer |
| ast.cot | Done | 536 | Expression, statement, type nodes |
| parser.cot | Done | 837 | Recursive descent + Pratt expressions |
| ir.cot | Done | 592 | IR types, instructions, module |
| lower.cot | Done | 780 | AST to IR lowering with scopes |
| emit.cot | Done | 540 | IR to bytecode with register allocation |
| main.cot | Done | 150 | Entry point, compilation pipeline |

**Total: ~4,263 lines**

## Next Steps

1. Test compilation with the main Cot compiler
2. Fix any compilation errors
3. Test running cot-minimal on hello.cot
4. Iterate until self-hosting works

## See Also

- `~/cotlang/comparison/11-parallel-bootstrap-race.md` - Race rules and context
- `~/cotlang/cot/src/` - Reference implementation (Zig)
- `~/cotlang/cot-compiler/` - Track A (do not touch!)
