# Wrap up

The calculator is working though, not totally functional. There are a few things that need to be taken care of. 

- % has not been implemented
- +/- has not been implemented

There are also some issues around how numbers are displayed. If we get a long decimal it runs off the screen, which is less than ideal. 

Calculations on integers return an integer answer. For example: 22/7 returns 3.0. But 22.0/7 returns 3.14285. 

What we have, so far, is a good proof of concept that shows how to build a user interface with SwiftUI and connect that interface to code written in Swift using `ObservableObject`, `@Published`, and `environmentObject`. 

To turn this into a bullet proof calculator will require a refactor. You could continue working with a string expression but, a better solution would be to store tokens representing each part of the expression in an array. Call each of the parts of an expression a "token". These tokens can be assembled as a string for display, and processed to create an expression for mathematical evaluation. 

## 6.0 Going further ▶︎ Token Array Approach

So far we’ve manipulated the **display string** directly.
For robust behaviour—especially when you add `%`, `±`, parentheses, history, or undo—store the input as an **array of tokens** and generate the display string from that array.

### 6.1 Define a `Token` type

```swift
enum Token: CustomStringConvertible {
  case number(String)
  case op(CalcKey)
  
  var description: String {
    switch self {
    case .number(let str):
      return str
    case .op(let key):
      return key.rawValue
    }
  }
  
  var doubleValue: Double? {
    guard case .number(let string) = self else { return nil }
    return Double(string)
  }
}
```

### 6.2 Store tokens in `GlobalState`

```swift
@Published private(set) var tokens: [Token] = [.number(0)]
var display: String { tokens.map(\.description).joined() }
```

* `display` becomes a **computed property**—no more manual string editing.

### 6.3 Building the token array

```swift
func keyPressed(_ key: CalcKey) {
    print(tokens)
    
    switch key {
      
      // digits
    case .zero, .one, .two, .three, .four,
        .five, .six, .seven, .eight, .nine:
      appendDigit(key)
      
      // operators
    case .add, .subtract, .multiply, .divide:
      replaceOrAppend(.op(key))
      
    case .decimal:
      appendDecimal()
      
    case .percent:
      //      applyPercent()
      print("% no implemented")
      
    case .plusMinus:
      //      toggleSign()
      print("+/- not implemented")
      
    case .clear:
      tokens = [.number("0")]
      
    case .equals:
      let result = evaluateTokens()
      let text = fomrat(result)
      tokens = [.number(String(text))]
    }
  }
```

#### Helper snippets

```swift
/// Add a decimal to the current number
private func appendDecimal(_ digit: String) {

    if case .number(var text) = tokens.last! {
        if text == "0" && !text.contains(".") {
        text = digit
        } else {
        text += digit
        }
        tokens[tokens.count - 1] = .number(text)
    } else {
        tokens.append(.number(digit))
    }
}

/// Replace trailing operator or append a new one.
private func replaceOrAppend(_ token: Token) {
    if case .op = tokens.last {
        tokens[tokens.count - 1] = token
    } else {
        tokens.append(token)
    }
}

/// Add a decimal to the current number
private func appendDecimal() { /* similar idea to currentNumberHasDot */ }
```

### 6.4 Evaluating tokens

*Join tokens into a string **after** glyph replacement, then feed `NSExpression`:*

```swift
private func evaluateTokens() -> Double {
    let expr = tokens.map(\.description)
        .joined()
        .replacingOccurrences(of: "×", with: "*")
        .replacingOccurrences(of: "÷", with: "/")
        .replacingOccurrences(of: "−", with: "-")

    return (NSExpression(format: expr)
              .expressionValue(with: nil, context: nil) as? NSNumber)?
              .doubleValue ?? 0
}
```

---

### Why tokens are better

| String-first                                  | Token array                                         |
| --------------------------------------------- | --------------------------------------------------- |
| Easy to start with                            | Scales to complex rules (%, ±, precedence override) |
| Harder to validate (double “..”, “++”)        | Each edit touches one token—validation is localized |
| Parsing required every time you need a number | Numbers are already `Double`                        |

**Class suggestion:**
*Assign students to refactor their calculator using `Token`.* Provide starter code for `enum Token` and ask them to implement `appendDigit`, `replaceOrAppend`, and `evaluateTokens()` step by step.

---

### Next steps

The code here is incomplete, you'll need to do some work to finish it up. The idea is to keep a list of "tokens" representing the math expression you are building.