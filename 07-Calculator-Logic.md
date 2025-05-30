# Calculator Logic

Before we write the code that **manages** the calculator logic, we need to understand *what that logic is*.

You’ve used a calculator before, but now you should observe it like a debugger.
Open the macOS Calculator and perform each step **slowly**, noting exactly how the display changes. Build a **mental model** you can later translate into code.

> **Note** These observations match the **current macOS Calculator** (Sonoma).
> Older handheld calculators behave a little differently.

---

## First exploration

1. Display shows **“0”**.
2. Press **7** → display: **“7”**.
3. Press **×** → display: **“7×”**.
4. Press **6** → display: **“7×6”**.
5. Press **=** → display: **“42”**.

> Every key press is **concatenated** to the display; **=** evaluates the full expression.

---

## Multi-digit entry

1. Clear → display “0”.
2. Press **2** → “2”.
3. Press **5** → “25”.
4. Press **×** → “25×”.
5. Press **3** → “25×3”.
6. Press **=** → “75”.

Digits typed in succession build multi-digit numbers—no extra spacing or delimiters.

---

## Continuing after a result

1. Display shows **“75”**.
2. Press **+** → “75+”.
3. Press **2** → “75+2”.
4. Press **÷** → “75+2÷”.
5. Press **2** → “75+2÷2”.
6. Press **=** → “76”.

macOS Calculator **respects order of operations**: 75 + (2 ÷ 2) = 76.

---

## Another order-of-operations check

1. Press **×** → “76×”.
2. Press **2** → “76×2”.
3. Press **+** → “76×2+”.
4. Press **8** → **“76×2+8”** 🔧 (typo corrected).
5. Press **=** → “160”.

Evaluated as (76 × 2) + 8 = 160. Good—precedence confirmed.

---

## Percent key

1. Press **%** → “160%”.
2. Press **=** → “1.6”.

On macOS, **% divides the immediately preceding number by 100** (160 % → 1.6).
Further testing is needed to see how `%` interacts with other operators.

---

## Plus-minus (change sign)

1. Clear → “0”.
2. Press **9** → “9”.
3. Press **±** → **“(-9)”** (calculator wraps the number in parentheses).
4. Press **+** → “(-9)+”.
5. Press **1** → “(-9)+1”.
6. Press **=** → “-8”.

Parentheses avoid the confusing **double minus** you’d get with “9-–1”.

---

## Sequential operators

1. Clear → “0”.
2. Press **2** → “2”.
3. Press **+** → “2+”.
4. Press **×** → **“2×”** — the second operator **replaces** the first.

---

## An interesting mixed example

1. Clear → “0”.
2. Press **8** → “8”.
3. Press **−** → “8−”.
4. Press **5** → “8−5”.
5. Press **×** → “8−5×”.
6. Press **3** → “8−5×3”.
7. Press **=** → “-7”.

macOS Calculator treats this as **8 + (−5 × 3)**, giving −7.
The minus key toggles the sign of the *current* number, so the pending operator is still addition.

---

## Your tasks

### **Challenge 1 – Observe**

Systematically test **every** key:

* multiple decimals,
* leading zeros,
* chaining `%=`,
* pressing `=` repeatedly,
* divide-by-zero, etc.

### **Challenge 2 – Pseudo-code**

Write pseudo-code for each behaviour you observe. Example blocks:

```text
// 2 + 2 = 4
start: "0"
tap 2   → "2"
tap +   → "2+"
tap 2   → "2+2"
tap =   → "4"
```

```text
// pressing two operators, second replaces first
start: "0"
tap 2 → "2"
tap + → "2+"
tap × → "2×"  // '+' replaced by '×'
```

Translate these pseudo-code snippets into Swift once your mental model is clear.

> **Don’t skip the observation phase.**
> Reverse-engineering the real calculator builds intuition and leads to cleaner code.
