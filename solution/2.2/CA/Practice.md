**Problem Statement**

Compare zero-, one-, two-, and three-address machines by writing programs to compute

$$
X = \frac{A + B \times C}{D - E \times F}
$$

for each of the four machines.

The instructions available for use are:

| 0-Address                       | 1-Address | 2-Address        | 3-Address      |
| ------------------------------- | --------- | ---------------- | -------------- |
| PUSH M                          | LOAD M    | MOVE (X ← Y)     | MOVE(X ← Y)    |
| POP M                           | STORE M   | ADD  (X ← X + Y) | ADD(X ← Y + Z) |
| ADD    (pop two, push sum)      | ADD M     | SUB  (X ← X − Y) | SUB(X ← Y − Z) |
| SUB    (pop two, push diff)     | SUB M     | MUL  (X ← X × Y) | MUL(X ← Y × Z) |
| MUL    (pop two, push prod)     | MUL M     | DIV  (X ← X ÷ Y) | DIV(X ← Y ÷ Z) |
| DIV    (pop two, push quotient) | DIV M     |                  |                |


**Solution:**
Here’s how you’d compute

$$
X = \frac{A + B \times C}{\,D - E \times F\,}
$$

on each machine style:

---

### 0-Address (stack) machine

```
    PUSH A        ; stack: [A]
    PUSH B        ; stack: [A, B]
    PUSH C        ; stack: [A, B, C]
    MUL           ; pop B,C → push B*C         → [A, B*C]
    ADD           ; pop A,(B*C) → push A+B*C    → [A+B*C]
    PUSH D        ; → [A+B*C, D]
    PUSH E        ; → [A+B*C, D, E]
    PUSH F        ; → [A+B*C, D, E, F]
    MUL           ; pop E,F → push E*F           → [A+B*C, D, E*F]
    SUB           ; pop D,(E*F) → push D−E*F      → [A+B*C, D−E*F]
    DIV           ; pop (A+B*C),(D−E*F) → push (A+B*C)/(D−E*F)
    POP X         ; store result in X
```

---

### 1-Address (accumulator) machine

We use two temporaries, N and T:

```
    LOAD B        ; AC ← B
    MUL C         ; AC ← B*C
    ADD A         ; AC ← A + B*C
    STORE N       ; N ← numerator

    LOAD E        ; AC ← E
    MUL F         ; AC ← E*F
    STORE T       ; T ← E*F

    LOAD D        ; AC ← D
    SUB T         ; AC ← D − T
    STORE T       ; T ← denominator

    LOAD N        ; AC ← numerator
    DIV T         ; AC ← N / T
    STORE X       ; X ← result
```

---

### 2-Address machine

All operations are of the form `OP(dest, src)` meaning `dest ← dest OP src`.
We’ll use registers/memory locations T1–T3:

```
    MOVE T1, B    ; T1 ← B
    MUL  T1, C    ; T1 ← T1 * C
    ADD  T1, A    ; T1 ← T1 + A      ← numerator in T1

    MOVE T2, E    ; T2 ← E
    MUL  T2, F    ; T2 ← T2 * F      ← E*F in T2

    MOVE T3, D    ; T3 ← D
    SUB  T3, T2   ; T3 ← T3 − T2     ← denominator in T3

    MOVE X, T1    ; X ← numerator
    DIV  X, T3    ; X ← X / T3       ← final result in X
```

---

### 3-Address machine

Here you can do everything in one go with `OP(dest, src1, src2)`:

```
    MUL  T1, B, C   ; T1 = B * C
    ADD  T2, A, T1  ; T2 = A + T1           ← numerator

    MUL  T3, E, F   ; T3 = E * F
    SUB  T4, D, T3  ; T4 = D – T3           ← denominator

    DIV  X,  T2, T4 ; X  = T2 / T4          ← result
```
