# SimpleLang Interpreter

A tree-walking interpreter for **SimpleLang**, a small functional-style language, written in Java with [ANTLR4](https://www.antlr.org/). Built for the Compilers and Computer Architecture module at the University of Sussex, starting from a provided skeleton project.

## The language

A SimpleLang program is a list of function declarations, and execution starts at `main`. Functions take at most one typed parameter in the grammar and return the value of their last expression.

```
int main(int n) {
  print n;
  space;
  if (n <= 1) then { 1 } else { (n * double(n)) }
}
```

Supported constructs (see `SimpleLang.g4`):

| Feature | Syntax |
|---|---|
| Types | `int`, `bool`, `unit` |
| Integer literals | `0`, `42`, `-7` |
| Arithmetic and comparison | `(a + b)`, `(a - b)`, `(a * b)`, `(a == b)`, `(a < b)`, `(a <= b)` |
| Assignment | `x := expr` |
| Function calls | `f(a, b)` |
| Conditionals | `if cond then { ... } else { ... }` |
| Blocks | `{ e1; e2; e3 }`, which evaluate to the last expression |
| Output | `print expr`, `space` |

Binary operations are fully parenthesised, which keeps the grammar unambiguous.

## How it works

1. `SimpleLang.g4` defines the lexer and parser rules. ANTLR generates the lexer, parser and visitor interfaces from it.
2. `SimpleLangInterpreter.java` implements the generated visitor. Each function call pushes a new variable frame onto a stack, and `main`'s arguments come from the command line. Conditionals and arithmetic use plain integers, with booleans as 0 and 1.
3. `Task1.java` reads a program from standard input, parses it, runs it and prints the result after `NORMAL_TERMINATION`.
4. `Task1Tester.java` runs every `.simp` program in a folder, using the matching `.args` file for inputs and `.answers` file for expected output.

## Test programs

The `NNNNN.simp` files are sample programs. Each has a `.simp.args` file (one line of arguments per run) and a `.simp.answers` file (expected output for each run).

## Build and run

You need Java and the ANTLR 4.13.1 complete jar (not included).

```bash
# generate the lexer, parser and visitor from the grammar
java -jar antlr-4.13.1-complete.jar -visitor SimpleLang.g4

# compile
javac -cp antlr-4.13.1-complete.jar *.java

# run a program with arguments
java -cp .:antlr-4.13.1-complete.jar Task1 2 4 < 00001.simp
```

A Gradle build (`build.gradle`) is also included, with `run` and `runtests` tasks.

## Tech

Java · ANTLR4 · Gradle
