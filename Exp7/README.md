# Experiment 7: String Pattern Matching for Suffix 'com' using LEX

## 1. Objective
To write a LEX program that evaluates each input line and identifies whether the string/domain ends with the suffix `com` or not.

---

## 2. Source Code
- **LEX Source File:** [`exp7_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp7/exp7_1276.l)

### Code Overview
```lex
%{
#include <stdio.h>
%}

%%
^.*com$ { printf("ENDS WITH com: %s\n", yytext); }
^.*$    { printf("DOES NOT END WITH com: %s\n", yytext); }
\n      { /* Ignore newline */ }
%%

int yywrap()
{
    return 1;
}

int main()
{
    printf("Enter lines (Ctrl+D to stop):\n");
    yylex();
    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Process specification with lex
lex exp7_1276.l

# Step 2: Compile with cc linking the lex library (-ll)
cc lex.yy.c -ll

# Step 3: Run the executable
./a.out
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim exp7_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex exp7_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ cc lex.yy.c -ll
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./a.out
Enter lines (Ctrl+D to stop):
google.com
ENDS WITH com: google.com
amazon.in
DOES NOT END WITH com: amazon.in
irctc.co.in
DOES NOT END WITH com: irctc.co.in
```

---

## 5. Explanation
- **Anchors (`^` and `$`)**:
  - `^` anchors the regular expression match to the beginning of the line.
  - `$` anchors the match to the end of the line.
- **Rule Precedence:**
  - `^.*com$`: Matches an entire line that ends specifically with the substring `com`.
  - `^.*$`: Matches any other entire line that does not satisfy the more specific preceding rule. In LEX, when two patterns match strings of equal length, the rule listed earlier in the file takes precedence.
- **Newline Handling:** `\n` is discarded silently using `{ /* Ignore newline */ }`.
