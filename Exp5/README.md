# Experiment 5: Case Conversion (Toggle Case) using LEX

## 1. Objective
To write and execute a LEX program that toggles the case of alphabetic characters in an input string:
- Converting every lowercase character (`a-z`) to uppercase (`A-Z`).
- Converting every uppercase character (`A-Z`) to lowercase (`a-z`).
- Retaining newlines and other non-alphabetic characters as-is.

---

## 2. Source Code
- **LEX Source File:** [`case_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp5/case_1276.l)

### Code Overview
```lex
%{
#include <stdio.h>
%}

%%
[a-z] {
    printf("%c", yytext[0] - 32);
}

[A-Z] {
    printf("%c", yytext[0] + 32);
}

\n {
    printf("\n");
}

. {
    printf("%c", yytext[0]);
}
%%

int main()
{
    printf("Enter a string: ");
    yylex();
    return 0;
}

int yywrap()
{
    return 1;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Run lex compiler
lex case_1276.l

# Step 2: Compile with cc/gcc linking lex library (-ll) and specifying output name
cc lex.yy.c -ll -o case_1276

# Step 3: Execute the compiled program
./case_1276
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim case_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex case_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ cc lex.yy.c -ll -o case_1276
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./case_1276
Enter a string: MY NAME IS AKSHAN
my name is akshan
i LIVE IN NAGPUR
I live in nagpur
```

---

## 5. Explanation
- **ASCII Conversion:**
  - For lowercase `[a-z]`, ASCII values are shifted by `- 32` to produce uppercase ASCII values (`'a'` (97) - 32 = `'A'` (65)).
  - For uppercase `[A-Z]`, ASCII values are shifted by `+ 32` to produce lowercase ASCII values (`'A'` (65) + 32 = `'a'` (97)).
- **Preservation of Whitespace & Special Characters:**
  - `\n` outputs a newline character.
  - `.` matches any other single character (such as spaces, punctuation, digits) and prints it unchanged via `printf("%c", yytext[0])`.
