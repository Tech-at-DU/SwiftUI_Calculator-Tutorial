# Creating and Evaluating Expressions

To make our calculator useful we must both **build a mathematical expression** and **evaluate** it. The built-in macOS Calculator does that—and handles a few keys (`%`, `±`, decimal, etc.) with extra rules. For now we’ll concentrate on straightforward expressions.

Apple gives us a shortcut: **`NSExpression`**. It can evaluate a string such as `"8*3+2"` in one line. See:

* Apple docs ▶︎ [https://developer.apple.com/documentation/foundation/nsexpression/init(format\:argumentarray:)](https://developer.apple.com/documentation/foundation/nsexpression/init%28format:argumentarray:%29)
* Community intro ▶︎ [https://medium.com/@elifedman/understanding-nsexpression-in-swift-a-beginners-guide-50b7baa36271](https://medium.com/@elifedman/understanding-nsexpression-in-swift-a-beginners-guide-50b7baa36271)

---

## 1 – Entering the expression

In **`GlobalState`**, update `keyPressed(_:)`:

```swift
func keyPressed(_ key: CalcKey) {
    switch key {

    case .clear:
        if display != "0" {
            display.removeLast()
            if display.isEmpty { display = "0" }
        }

    case .decimal:
        // handled later
        break

    case .plusMinus:
        // handled later
        break

    // digits
    case .one, .two, .three, .four, .five,
         .six, .seven, .eight, .nine, .zero:
        display = (display == "0") ? key.rawValue : display + key.rawValue

    // operators and %
    case .divide, .multiply, .subtract, .add, .percent:
        display += key.rawValue   // 🔧 we’ll refine this to avoid double-operators

    case .equals:
        let answer = evaluate()
        display  = String(answer)
    }
}
```

You can now type **`8×3+2`**. Decimal, ±, percent, and operator replacement will be added next.

---

## 2 – Evaluating with `NSExpression`

Add to **`GlobalState`**:

```swift
func evaluate() -> Double {
    // Convert UI glyphs to symbols that NSExpression understands
    let expr = display
        .replacingOccurrences(of: "×", with: "*")
        .replacingOccurrences(of: "÷", with: "/")
        .replacingOccurrences(of: "−", with: "-")   // U+2212 → ASCII hyphen

    let result = (NSExpression(format: expr)
                    .expressionValue(with: nil, context: nil) as? NSNumber)?
                    .doubleValue ?? 0
    return result
}
```

> **Why the replacements?**
>
> * `NSExpression` recognises `* / - +`, not the typographic glyphs.
> * Passing `"−"` (U+2212) unmodified crashes the parser.

After wiring `case .equals`, entering **`3*2+7`** and pressing **=** shows **“13.0”**.

---

## 3 – Known gaps

* Decimal point does nothing yet.
* ± key does nothing.
* `22/7` yields **3.0** (integer division); `22.0/7` yields **3.142857…**
* `NSExpression` prints many decimals.
* Consecutive operators append instead of replacing.

We’ll fix these in turn.

---

## 4 – Preventing duplicate operators

Right now you can type `6+-×÷`, which crashes evaluation.
When an operator key is tapped, if **the last character is already an operator** we should replace it; otherwise, append.

In `GlobalState` add a helper and update the operator case:

```swift
private let operatorSet: Set<Character> = ["+", "−", "×", "÷", "%"]

// …

case .divide, .multiply, .subtract, .add, .percent:
    if let last = display.last, operatorSet.contains(last) {
        display.removeLast()          // replace existing operator
    }
    display += key.rawValue
```

Now **`6+` → `6×`** replaces the `+` instead of appending.

---

## 5 – Implementing the decimal key

**Goal:** allow exactly one “.” per number; if a number hasn’t started yet, insert `"0."`.

Helpers:

```swift
/// Substring representing the current number (after the last operator)
private var currentNumber: Substring {
    let i = display.lastIndex(where: { operatorSet.contains($0) }) ?? display.startIndex
    let start = (i == display.startIndex) ? i : display.index(after: i)
    return display[start...]
}

private var currentNumberHasDot: Bool {
    currentNumber.contains(".")
}
```

Decimal case:

```swift
case .decimal:
    if currentNumberHasDot { break }                     // ignore 2nd dot
    if operatorSet.contains(display.last ?? " ")         // just typed an operator
        || display == "0" {
        display += "0."
    } else {
        display += "."
    }
```

* `3 .` → `3.`
* `3 ..` → still `3.`
* `× .` → `×0.`

---

### Next steps

* **± key** – wrap the current number in parentheses and negate it.
* **Percent key** – divide the current number by 100.
* **Floating-point enforcement** – append `.0` to integer tokens before evaluation to avoid integer division.
* **Result formatting** – use `NumberFormatter` to drop trailing `.0` or limit decimals.

Each new rule should be backed by a quick manual test (e.g. `"8 + + = "` must leave display `"8"`). Keep refining `GlobalState` until behaviour matches the system Calculator.
