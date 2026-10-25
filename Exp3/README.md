# Experiment 3: Pattern Recognition for Words Starting with 'A' or 'a' using LEX

## 1. Objective
To write and execute a LEX program that recognizes tokens / words starting with the letter `'A'` or `'a'` (case-insensitive) followed by alphanumeric characters, differentiates them from words starting with other characters, and tallies the total counts for both matched and unmatched words.

---

## 2. Source Code
- **LEX Source File:** [`match_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp3/match_1276.l)

### Code Overview
```lex
%{
#include <stdio.h>

int match = 0;
int notmatch = 0;
%}

%%

[Aa][a-zA-Z0-9]* {
    printf("Matched     : %s\n", yytext);
    match++;
}

[B-Zb-z0-9][a-zA-Z0-9]* {
    printf("Not Matched : %s\n", yytext);
    notmatch++;
}

[ \t\n]+    ;   /* Ignore spaces, tabs and newlines */

.           ;   /* Ignore other characters */

%%

int yywrap()
{
    return 1;
}

int main()
{
    printf("Enter input (Press Ctrl+D to stop):\n");

    yylex();

    printf("\n----- Result -----\n");
    printf("Total Matched     = %d\n", match);
    printf("Total Not Matched = %d\n", notmatch);

    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Run lex on the specification file
lex match_1276.l

# Step 2: Compile generated C file with the lex library (-ll)
cc lex.yy.c -ll

# Step 3: Execute the binary
./a.out
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
sit@sit-OptiPlex-3280-AIO:~$ vim match_1276.l
sit@sit-OptiPlex-3280-AIO:~$ lex match_1276.l
sit@sit-OptiPlex-3280-AIO:~$ cc lex.yy.c -ll
sit@sit-OptiPlex-3280-AIO:~$ ./a.out
Enter input (Press Ctrl+D to stop):
Apple Ball ant Cat Zebra apple Banana A123 123 abc bcd
Matched     : Apple
Not Matched : Ball
Matched     : ant
Not Matched : Cat
Not Matched : Zebra
Matched     : apple
Not Matched : Banana
Matched     : A123
Not Matched : 123
Matched     : abc
Not Matched : bcd

----- Result -----
Total Matched     = 5
Total Not Matched = 6
sit@sit-OptiPlex-3280-AIO:~$
```

---

## 5. Explanation
- **Matched Pattern (`[Aa][a-zA-Z0-9]*`):** Matches any token that begins with uppercase `'A'` or lowercase `'a'` followed by zero or more alphanumeric characters. Increments the `match` counter.
- **Unmatched Pattern (`[B-Zb-z0-9][a-zA-Z0-9]*`):** Matches tokens starting with any other alphabetical or numeric character and increments `notmatch`.
- **Delimiter Handling:** Spaces, tabs, and newlines are skipped via `[ \t\n]+ ;`.
- **Result Summary:** Upon encountering EOF (`Ctrl+D`), the totals for matched and unmatched tokens are neatly printed.
