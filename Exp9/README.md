# Experiment 9: Desk Calculator (Infix Arithmetic Evaluator) using LEX and YACC

## 1. Objective
To design and implement an interactive Desk Calculator using LEX and YACC that evaluates standard infix arithmetic expressions with operator precedence (`*` and `/` higher than `+` and `-`), parentheses handling, and error recovery.

---

## 2. Source Files
- **LEX Source File:** [`cal_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp9/cal_1276.l)
- **YACC Source File:** [`cal_1276.y`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp9/cal_1276.y)

### LEX File Overview (`cal_1276.l`)
```lex
%{
#include "y.tab.h"
#include <stdlib.h>
%}

%%
[0-9]+      { yylval = atoi(yytext); return NUMBER; }
[+\-*/\n()] { return yytext[0]; }
.           { return yytext[0]; }
%%

int yywrap() {
    return 1;
}
```

### YACC File Overview (`cal_1276.y`)
```yacc
%{
#include <stdio.h>
#include <stdlib.h>

int yylex();
int yyerror(char *s);
%}

%token NUMBER

%left '+' '-'
%left '*' '/'

%%
input : /* empty */
      | input line
      ;

line  : expr '\n'
        { printf("Result = %d\n", $1); }
      | error '\n'
        { printf("Syntax Error! Please re-enter.\n"); yyerrok; }
      ;

expr  : expr '+' expr
        { $$ = $1 + $3; }
      | expr '-' expr
        { $$ = $1 - $3; }
      | expr '*' expr
        { $$ = $1 * $3; }
      | expr '/' expr
        {
          if ($3 == 0) {
              printf("Error: Division by zero!\n");
              $$ = 0;
          }
          else
              $$ = $1 / $3;
        }
      | '(' expr ')'
        { $$ = $2; }
      | NUMBER
        { $$ = $1; }
      ;

%%

int yyerror(char *s) {
    return 0;
}

int main() {
    printf("Desk Calculator: Enter expressions (Ctrl+C to exit)\n");
    yyparse();
    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Run yacc with -d flag to produce y.tab.c and y.tab.h
yacc -d cal_1276.y

# Step 2: Run lex to produce lex.yy.c
lex cal_1276.l

# Step 3: Compile generated C source files linking lex library (-ll)
gcc y.tab.c lex.yy.c -o cal_1276 -ll

# Step 4: Run desk calculator executable
./cal_1276
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim cal_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim cal_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ yacc -d cal_1276.y
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex cal_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ gcc y.tab.c lex.yy.c -o cal_1276 -ll
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./cal_1276
Desk Calculator: Enter expressions (Ctrl+C to exit)
10+50
Result = 60
50*40
Result = 2000
45+32*50
Result = 1645
```

---

## 5. Explanation
- **Precedence and Associativity:**
  - `%left '+' '-'` specifies that addition and subtraction are left-associative.
  - `%left '*' '/'` declared later gives multiplication and division higher precedence than addition and subtraction. For example, `45+32*50` evaluates $32 \times 50 = 1600$ first, then $45 + 1600 = 1645$.
- **Parentheses Support:** The rule `'(' expr ')'` allows grouping to override default operator precedence.
- **Error Handling:** When an invalid expression is entered, `error '\n'` traps the syntax failure and invokes `yyerrok` to allow continuous line-by-line calculations without crashing.
