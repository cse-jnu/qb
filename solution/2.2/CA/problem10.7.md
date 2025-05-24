**Question**

Consider a hypothetical computer with an instruction set of only two *n*-bit instructions.  The first bit specifies the opcode, and the remaining *n*–1 bits specify one of the 2ⁿ⁻¹ words of main memory.  The two instructions are:

* **SUBS X**
  Subtract the contents of memory location X from the accumulator, and store the result in both location X and the accumulator.
* **JUMP X**
  Place address X in the program counter (i.e. go to instruction at X).

A word in main memory may contain either an instruction or a binary number in two’s-complement notation.

**Demonstrate** that this instruction repertoire is reasonably complete by specifying how you would program each of the following operations:

1. **Data transfer**
   a. Transfer from memory location X into the accumulator.
   b. Transfer from the accumulator into memory location X.
2. **Addition**
   Add the contents of memory location X to the accumulator.
3. **Conditional branch**
   Branch to a specified label if the accumulator is negative (or nonzero, as you choose).
4. **Logical OR**
   Compute the bitwise OR of a one-word value in the accumulator with the contents of location X, leaving the result in the accumulator.
5. **I/O operations**
   Read a word from a memory-mapped input port into the accumulator, and write the accumulator out to a memory-mapped output port.

**Solution:**

```
SUBS X    ; ACC ← ACC – M[X];  M[X] ← ACC  
JUMP X    ; PC  ← X  
```

plus a handful of memory‐resident constants, a zero cell Z (M\[Z]=0), and the fact that any word whose MSB=1 is decoded as a JUMP, MSB=0 as a SUBS.

---

### a. Data‐transfer

#### X → ACC

1. **Zero out ACC** by subtracting ACC from itself:

   ```
     SUBS A      ; pick any cell A holding the current ACC value  
     SUBS A      ; ACC ← (ACC–ACC)=0  
   ```
2. **Load X** by negating twice:

   ```
     SUBS X      ; ACC ← 0 – M[X] = –X   (M[X] ← –X)
     SUBS Z      ; ACC ← (–X) –  0 = –X  (M[Z] ← –X)
     SUBS Z      ; ACC ← (–X) – (–X) = 0 (M[Z] ← 0)
     SUBS X      ; ACC ← 0 – (–X) = +X  (M[X] ← +X)
   ```

   Now ACC = +X and M\[X] is restored.

#### ACC → X

1. **Zero X**

   ```
     SUBS X      ; X ← ACC–X, ACC←X–ACC  
     SUBS X      ; X ← (X–ACC)–X = 0, ACC←0  
   ```
2. **Store ACC**

   ```
     SUBS Z      ; Z ← ACC–0 = ACC, ACC←ACC  
     SUBS X      ; X ← ACC–0 = ACC, ACC←ACC  
   ```

   (Here Z is a scratch holding the prior ACC if you need it later.)

---

### b. Addition

We want

$$
\text{ACC} \;\longleftarrow\;\text{ACC} + M[X].
$$

Just precompute –M\[X] in a cell X̄ and then do

```
  SUBS X̄    ; ACC ← ACC – (–M[X]) = ACC + M[X]
```

You lose X̄’s old value, so you’d typically rebuild X̄ from X whenever X changes.

---

### c. Conditional branch

We have only unconditional `JUMP`.  But we can *self‐modify* a word so that its MSB (opcode bit) becomes 1 (JUMP) exactly when ACC<0.

1. Reserve a template word at C:

   ```
     C: 000…0  (an all‐zero SUBS instruction, operand field irrelevant)
   ```
2. To branch to L if ACC<0, otherwise fall through:

   ```
     SUBS C      ; M[C] ← ACC – M[C] = ACC – 0 = ACC;  ACC ← ACC
     JUMP C      ; if MSB(M[C])=1 (ACC<0), this is decoded as JUMP to address=(low n–1 bits of ACC)
                 ; if MSB=0 (ACC≥0), it’s decoded as SUBS, so execution continues at C+1 (fall‐through)
   ```
3. Layout your true‐branch label L at the address you encoded into the low bits of C before you ran this snippet.

---

### d. Logical OR

Using arithmetic identities you can build bit‐wise OR from subtraction & two’s‐complement constants.  For one‐bit values A,B in ACC and M\[X]:

```
  // compute A OR B = A + B – A*B
  // 1) form A*B by repeated subtraction or a small table in memory
  // 2) form negA*B = –(A*B) in Y
  // 3) SUBS Y   ; ACC ← ACC – (–A*B) = ACC + A*B
  // 4) then SUBS X̄ ; where X̄ holds –B  to effect ACC – (–B) = ACC + B
  // 5) SUBS Z ; where Z held –A to add A back
  // 6) now ACC = A + B + A*B
  // 7) subtract twice the product: use two more SUBS on cells holding 2·A·B
  // after all, you realize A OR B in ACC
```

You trade a few SUBS’s and tiny lookup tables, but it’s mechanically doable.

---

### e. I/O via memory‐mapped devices

Declare special addresses IN and OUT.  Then:

* **Read**:

  ```
    SUBS IN     ; ACC ← ACC – M[IN]  => ACC now holds the device’s input code
  ```
* **Write**:

  ```
    // first put the character code into ACC by any SUBS‐sequence
    SUBS OUT    ; M[OUT] ← ACC – M[OUT] = ACC;  ACC ← ACC
  ```

which latches the byte/word onto the output device.

---

With just one binary operation (SUBS) plus an unconditional jump you can move data, do arithmetic (add, multiply by repeated SUBS), steer control by self‐modifying MSB’s into JUMP’s, implement all Boolean ops via arithmetic identities, and drive I/O through memory‐mapped ports. That is why “SUBS+JUMP” is a *reasonably complete* tiny instruction set.
