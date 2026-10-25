# Experiment 8: Postfix Expression Evaluator using LEX and YACC

## 1. Objective
To construct and implement an interactive Postfix (Reverse Polish Notation) Expression Evaluator using LEX (for tokenization) and YACC (for syntactic grammar rules and semantic value computation).

---

## 2. Source Files
- **LEX Source File:** [`postfix_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp8/postfix_1276.l)
- **YACC Source File:** [`postfix_1276.y`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp8/postfix_1276.y)

### LEX File Overview (`postfix_1276.l`)
```lex
%{
#include "y.tab.h"
#include <stdlib.h>
%}

%%
[0-9]+      { yylval = atoi(yytext); return NUMBER; }
[ \t]       { /* Ignore spaces and tabs */ }
\n          { return '\n'; }
.           { return yytext[0]; }
%%

int yywrap() {
    return 1;
}
```

### YACC File Overview (`postfix_1276.y`)
```yacc
%{
#include <stdio.h>
#include <stdlib.h>
int yylex(void);
void yyerror(char *s);
%}

%token NUMBER

%%

lines: /* empty */
    | lines line
    ;

line:   '\n'
    | exp '\n'       { printf("Result = %d\n", $1); }
    | error '\n'     { yyerrok; printf("Syntax Error! Please re-enter.\n"); }
    ;

exp:    exp exp '+'   { $$ = $1 + $2; }
    | exp exp '-'   { $$ = $1 - $2; }
    | exp exp '*'   { $$ = $1 * $2; }
    | exp exp '/'   {
            if($2 == 0) {
                printf("Error: Division by zero\n");
                $$ = 0;
            } else {
                $$ = $1 / $2;
            }
        }
    | NUMBER        { $$ = $1; }
    ;

%%

void yyerror(char *s) {
    /* yyerror is handled manually in the rules for better recovery */
}

int main() {
    printf("Postfix Evaluator: Enter expressions (Ctrl+C to exit)\n");
    yyparse();
    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Run yacc with -d flag to generate y.tab.c and y.tab.h
yacc -d postfix_1276.y

# Step 2: Run lex to produce lex.yy.c
lex postfix_1276.l

# Step 3: Compile both C files linking the lex library
gcc y.tab.c lex.yy.c -o postfix_1276 -ll

# Step 4: Run the executable
./postfix_1276
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim postfix_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim postfix_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ yacc -d postfix_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex postfix_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ gcc y.tab.c lex.yy.c -o postfix_1276 -ll
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./postfix_1276
Postfix Evaluator: Enter expressions (Ctrl+C to exit)
5 4 +
Result = 9
15 5 * 10 /
Result = 7
10 0 /
Error: Division by zero
Result = 0
10+5
Syntax Error! Please re-enter.
12 6 * 75 + 10 5 / -
Result = 145
```

---

## 5. Explanation
- **Lexical Analysis:** Numbers are converted to integer values and assigned to `yylval`, then `NUMBER` token is returned. Individual operators (`+`, `-`, `*`, `/`) and newline (`\n`) are returned directly as token ASCII characters.
- **Postfix Grammar:** In postfix notation, the operator appears after both operands: `exp exp '+'`, `exp exp '-'`, `exp exp '*'`, and `exp exp '/'`.
- **Division by Zero Protection:** When the divisor `$2` is `0`, an error is printed and `$$` evaluates to `0`.
- **Error Recovery:** The YACC rule `error '\n'` using `yyerrok` enables gracefully displaying syntax errors without terminating the program, prompting the user to re-enter.
