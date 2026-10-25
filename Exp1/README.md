# Experiment 1: Number Word to Digit Replacement using LEX

## 1. Objective
To write and execute a LEX program that recognizes number words in text (such as `ONE`, `Two`, `Zero`, etc.) in different case variants and replaces them with their corresponding numeric digits (`1`, `2`, `0`, etc.).

---

## 2. Source Code
- **LEX Source File:** [`number_1276.l`](file:///c:/Users/aksha/OneDrive/Desktop/Akshan/Compiler%20Construction%20Lab/Exp1/number_1276.l)

### Code Overview
```lex
%{
// Comment here
%}
%%
ONE|one|One {printf("1");}
TWO|two|Two {printf("2");}
THREE|three|Three {printf("3");}
FOUR|four|Four {printf("4");}
FIVE|five|Five {printf("5");}
SIX|six|Six {printf("6");}
SEVEN|seven|Seven {printf("7");}
EIGHT|eight|Eight {printf("8");}
NINE|nine|Nine {printf("9");}
TEN|ten|Ten {printf("10");}
ZERO|zero|Zero {printf("0");}
%%
void main(){
    yylex();
}
```

---

## 3. Compilation and Execution Steps

Run the following commands in the terminal:

```bash
# Step 1: Generate the C source file from LEX specification
lex number_1276.l

# Step 2: Compile the generated C code with gcc / cc linking the lex library (-ll)
cc lex.yy.c -ll

# Step 3: Run the compiled executable
./a.out
```

---

## 4. Terminal Session & Sample Output

As captured in the terminal session screenshot:

```text
sit@sit-OptiPlex-3280-AIO:~$ vim number_1276.l
sit@sit-OptiPlex-3280-AIO:~$ lex number_1276.l
sit@sit-OptiPlex-3280-AIO:~$ cc lex.yy.c -ll
sit@sit-OptiPlex-3280-AIO:~$ ./a.out
ONE
1
One
1
Two
2
Five
5
Zero
0
Ten
10
```

---

## 5. Explanation
- **Lexical Rules:** Each rule matches specific string patterns representing numbers (case variations like `ONE`, `one`, `One`).
- **Action:** When a pattern matches, `printf()` outputs the corresponding integer representation.
- **Default Action:** Any character not matching any explicit rule is echoed to standard output by Lex's default rule.
- **Entry Point:** The `main()` function invokes `yylex()` to begin lexical processing until EOF (`Ctrl+D`).
