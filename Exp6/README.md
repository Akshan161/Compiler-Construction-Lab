# Experiment 6: Decimal to Hexadecimal Conversion using LEX

## 1. Objective
To implement a LEX program that accepts decimal numbers entered by the user, converts them into their equivalent hexadecimal format, and prints the result.

---

## 2. Source Code
- **LEX Source File:** [`exp6_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp6/exp6_1276.l)

### Code Overview
```lex
%{
#include <stdio.h>
#include <stdlib.h>
%}

%%
[0-9]+ {
    int n = atoi(yytext);
    int r;
    char hex[20];
    int i = 0;
    while (n > 0) {
        r = n % 16;
        if (r < 10)
            hex[i++] = r + '0';
        else
            hex[i++] = r - 10 + 'A';

        n = n / 16;
    }
    printf("Hexadecimal = ");
    while (i > 0)
        printf("%c", hex[--i]);
    printf("\n");
}
%%

int yywrap() {
    return 1;
}

int main() {
    printf("Enter decimal: ");
    yylex();
    return 0;
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Run lex compiler
lex exp6_1276.l

# Step 2: Compile the generated C file with lex library
cc lex.yy.c -ll

# Step 3: Run the output binary
./a.out
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ vim exp6_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ lex exp6_1276.l
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ cc lex.yy.c -ll
lab-03-22@lab-03-22-OptiPlex-3280-AIO:~$ ./a.out
Enter decimal: 8743
Hexadecimal = 2227
```

---

## 5. Explanation
- **Token Recognition:** `[0-9]+` matches sequences of one or more decimal digits.
- **Conversion Algorithm:**
  - `atoi(yytext)` parses the matched string into an integer `n`.
  - The loop iteratively computes remainder `r = n % 16`.
  - For digits `0-9`, `'0' + r` is stored in the buffer `hex`.
  - For values `10-15`, `'A' + (r - 10)` is stored to map to letters `A-F`.
  - `n` is updated via integer division by 16 (`n = n / 16`).
- **Output:** The remainder characters are printed in reverse order to form the correct hexadecimal number. E.g., `8743` in base 10 equals `2227` in hexadecimal ($2 \times 16^3 + 2 \times 16^2 + 2 \times 16^1 + 7 \times 1 = 8192 + 512 + 32 + 7 = 8743$).
