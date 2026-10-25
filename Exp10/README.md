# Experiment 10: Syntax Validation of 'for' Loop using LEX and YACC

## 1. Objective
To write and execute a parser using LEX and YACC that checks the syntactic validity of a standard C-style `for` loop construct (verifying correct initialization, condition, and increment expressions enclosed in parentheses).

---

## 2. Source Files
- **LEX Source File:** [`exp10_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp10/exp10_1276.l)
- **YACC Source File:** [`exp10_1276.y`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp10/exp10_1276.y)

### LEX File Overview (`exp10_1276.l`)
```lex
%{
#include "y.tab.h"
%}

%%

"for"       { return FOR; }
"++"        { return INC; }
"--"        { return DEC; }

[a-zA-Z][a-zA-Z0-9]*  { return ID; }
[0-9]+                { return NUM; }

"="         { return '='; }
"<"         { return '<'; }
">"         { return '>'; }
";"         { return ';'; }
"("         { return '('; }
")"         { return ')'; }

[ \t\n]     ;

.           { return yytext[0]; }

%%

int yywrap()
{
    return 1;
}
```

### YACC File Overview (`exp10_1276.y`)
```yacc
%{
#include <stdio.h>
#include <stdlib.h>
int yylex(void);
void yyerror(char *s);
%}

%token FOR ID NUM INC DEC

%%

program:
    for_loop
    {
        printf("Valid for loop\n");
    }
    ;

for_loop:
    FOR '(' initialization ';' condition ';' increment ')'
    ;

initialization:
    ID '=' NUM
    ;

condition:
    ID '<' NUM
    | ID '>' NUM
    ;

increment:
    ID INC
    | ID DEC
    ;

%%

void yyerror(char *s)
{
    printf("Invalid for loop\n");
}

int main()
{
    printf("Enter a for loop:\n");
    yyparse();
    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Generate parser C file and header from YACC specification
yacc -d exp10_1276.y

# Step 2: Generate scanner C file from LEX specification
lex exp10_1276.l

# Step 3: Compile both source files linking the lex library
gcc y.tab.c lex.yy.c -o exp10_1276 -ll

# Step 4: Execute the validator
./exp10_1276
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim exp10_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim exp10_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ yacc -d exp10_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex exp10_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ gcc y.tab.c lex.yy.c -o exp10_1276 -ll
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./exp10_1276
Enter a for loop:
for(i=0; i<10; i++)
Valid for loop
for(i=0 i<10; i++)
Invalid for loop
```

---

## 5. Explanation
- **Lexical Tokens:** Recognizes keyword `for`, unary operators `++` and `--`, identifiers (`ID`), numbers (`NUM`), and punctuation symbols `(`, `)`, `;`, `=`, `<`, `>`.
- **Grammar Structure:**
  - `FOR '(' initialization ';' condition ';' increment ')'` enforces the exact syntax order.
  - `initialization`: Requires an assignment like `i = 0`.
  - `condition`: Supports relational conditions like `i < 10` or `i > 0`.
  - `increment`: Expects `i++` or `i--`.
- **Validation:**
  - If the input strictly conforms to the grammar (e.g., `for(i=0; i<10; i++)`), `yyparse()` succeeds and prints `"Valid for loop"`.
  - If a required token or semicolon is omitted (e.g., `for(i=0 i<10; i++)`), `yyerror()` triggers and prints `"Invalid for loop"`.
