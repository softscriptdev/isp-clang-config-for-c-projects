# C Formatter & Linter Config (TU Graz ISP Style Guide)

A lightweight configuration for C projects to automatically format and statically analyze source code according to the **ISP Style Guide (TU Graz)**.

## Usage

### Automatically Format Code

To format a C file directly according to the style guide, run the following command in your terminal:

```bash
clang-format -i <filename.c>
```

*(The `-i` flag stands for "in-place" and directly overwrites the file with the formatted version.)*

### Analyze Code (Linting)

To statically analyze your code for complexity and style rules:

```bash
clang-tidy <filename.c>
```

## Currently Supported Rules

### Formatter (`clang-format`)

Based on the LLVM style, the formatter enforces the following rules:

* **Curly Braces (Allman Style):** Opening braces are placed on their own line after functions, structs, enums, unions, and control statements (`if`, `for`, `while`, etc.). `else` and `while` (in `do-while` loops) are also placed on a new line after a closing brace.
* **Line Length:** Lines exceeding **120 characters** are automatically wrapped.
* **Functions:** Single-line function definitions are disallowed (`AllowShortFunctionsOnASingleLine: None`) to ensure consistent brace placement.
* **Alignment:** Trailing inline comments and consecutive macro definitions (`#define`) across empty lines are vertically aligned.

### Linter (`clang-tidy`)

The linter checks function complexity (`readability-function-size`) and warns if a function exceeds the following thresholds:

* **Maximum Nesting Depth:** 3 levels (`NestingThreshold: 3`)
* **Maximum Statements:** 70 statements per function (`StatementThreshold: 70`)
* **Maximum Parameters:** 6 parameters per function (`ParameterThreshold: 6`)

## Known Limitations

> [!WARNING]
> **Formatting of `typedef struct`**
> The formatter does not automatically place the `typedef` keyword on a new line for struct definitions.
>
> **Current Behavior:**
>
> ```c
> struct _Command_
> {
>   char *command_name_;
>   char *args_[MAX_ARGS];
>   int num_args_;
> } typedef Command;
> ```
>
> **Workaround:**
> Place `typedef` on a new line manually. To prevent `clang-format` from reverting this adjustment on the next run, wrap the block with special comments:
>
> ```c
> // clang-format off
> struct _Command_
> {
>   char *command_name_;
>   char *args_[MAX_ARGS];
>   int num_args_;
> }
> typedef Command;
> // clang-format on
> ```