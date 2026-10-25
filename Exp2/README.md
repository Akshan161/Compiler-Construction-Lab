# Experiment 2: Lexical Analyzer for C Source Code using LEX

## 1. Objective
To implement a lexical analyzer using LEX that reads a C source code file from disk and counts:
- Total number of lines
- Total number of whitespace characters (spaces & tabs)
- Total number of words
- Total number of C language keywords
- Total number of identifiers
- Total number of comments (both single-line and multi-line comments)

---

## 2. Source Files
- **LEX Source File:** [`analyzer1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp2/analyzer1276.l)
- **Sample Input File:** [`input1276.c`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp2/input1276.c)

### Input Code (`input1276.c`)
```c
int main(){
    int x=5;
    /*Multiline
      Comment*/
    if (x>0){
        return 1;
    }
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Generate C source code from LEX file
lex analyzer1276.l

# Step 2: Compile the generated C code into an executable
gcc lex.yy.c -o analyzer1276

# Step 3: Run the analyzer with the target C file as an argument
./analyzer1276 input1276.c
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
sit@sit-OptiPlex-3280-AIO:~$ vim analyzer1276.l
sit@sit-OptiPlex-3280-AIO:~$ vim input1276.c
sit@sit-OptiPlex-3280-AIO:~$ lex analyzer1276.l
sit@sit-OptiPlex-3280-AIO:~$ gcc lex.yy.c -o analyzer1276
sit@sit-OptiPlex-3280-AIO:~$ ./analyzer1276 input1276.c
=== Lexical Analysis Results ===
Lines:       7
Spaces:      28
Words:       10
Keywords:    4
Identifiers: 3
Comments:    1
sit@sit-OptiPlex-3280-AIO:~$
```

---

## 5. Explanation
- **File Input Handling:** `yyin = fopen(argv[1], "r")` redirects standard Lex input stream to read directly from the specified file.
- **Regular Expressions:**
  - `\n` tracks newline count (`lines++`).
  - `[ \t]` counts whitespace characters (`spaces++`).
  - `"//".*` matches single-line comments.
  - `"/*"([^*]|\*+[^*/])*\*+"/"` accurately matches C-style multiline comments.
  - Keywords pattern matches standard C reserved words (`int`, `float`, `char`, `double`, `void`, `if`, `else`, `while`, `for`, `return`, `break`) and increments both `keywords` and `words`.
  - `[a-zA-Z_][a-zA-Z0-9_]*` matches identifiers and increments `identifiers` and `words`.
  - `[0-9]+` matches numeric literals and increments `words`.
  - `.` discards operators and punctuation characters.
- **Output:** Outputs a summary table of the lexical token distribution upon EOF.
