# Using an Enum

To keep the code organized, we can use an **enum**. An enum is ideal when you have a **fixed set of choices**. It’s usually better than hard-coding strings or numbers because:

* The cases are **descriptive**, which makes your code easier to read.
* The compiler and code editor understand them, so typos are caught at build-time.

A calculator has a fixed number of keys, and each key will be referenced in several places, so an enum is a perfect fit.

---

## 1. Create an enum for the keys

Add a new Swift file named **`CalcKey.swift`** and define:

```swift
enum CalcKey: String {
    case clear      = "C"
    case decimal    = "."
    case divide     = "÷"
    case multiply   = "×"
    case subtract   = "−"
    case plusMinus  = "±"
    case percent    = "%"
    case add        = "+"
    case equals     = "="
    
    case one   = "1", two   = "2", three = "3"
    case four  = "4", five  = "5", six   = "6"
    case seven = "7", eight = "8", nine  = "9"
    case zero  = "0"
}
```

---

## 2. Update `makeButton`

Add a parameter so `makeButton` knows which key to display:

```swift
func makeButton(key: CalcKey) -> some View { // add a parameter for the key
    Button {
        print("Tapped \(key.rawValue)") // display the key when tapped
    } label: {
        Text(key.rawValue)            // use the enum’s raw value for the label
            .font(.system(size: 40))
            .frame(width: 80, height: 80)
            .foregroundColor(.white)
            .background(Color.gray)
            .cornerRadius(40)
    }
}
```

---

## 3. Build the first row

Replace your original placeholder `HStack` with concrete keys:

```swift
HStack {
    makeButton(key: .clear)
    makeButton(key: .plusMinus)
    makeButton(key: .percent)
    makeButton(key: .divide)
}
```

---

## 4. **Challenge:** add the remaining rows

Arrange the keys like this:

| C | ± | % | ÷ |
| - | - | - | - |
| 7 | 8 | 9 | × |
| 4 | 5 | 6 | − |
| 1 | 2 | 3 | + |
| 0 | . | = |   |

*Note:* In the classic calculator, the **0** key spans two columns. Modern iOS adds a “mode” button in the blank space; we’ll handle that later.

---

### Summary

* **Enums** give you type-safe, self-documenting keys.
* `makeButton(key:)` now takes a single enum case and uses its `rawValue` for the label.
* Building the UI with `VStack` + `HStack` keeps the layout clear and concise.

Keep going—once all the rows are in place, you’ll have the full calculator keypad!
