# Using an Enum

To really get organized we can use an enum. An enum is a construct that is used when you have a fixed set of choices. Its often a better choice than using strings or numbers. Enums are descriptive which makes it easier for you to write, and are recognized by the compiler and code editor, which catches errors. 

Since there are a fixed number of keys on calculator and each key will be referenced in a few places an enum make s a good choice to represent the keys. 

Add a new swift file, named CalcKeys. 

Write an enum to represent all of the keys: 

```Swift 
enum CalcKey: String {
  case clear = "C"
  case decimal = "."
  case divide = "÷"
  case multiply = "×"
  case subtract = "-"
  case plusMinus = "+/-"
  case percent = "%"
  case add = "+"
  case equals = "="
  case one = "1"
  case two = "2"
  case three = "3"
  case four = "4"
  case five = "5"
  case six = "6"
  case seven = "7"
  case eight = "8"
  case nine = "9"
  case zero = "0"
}
```

Modify `makeButton()`, this needs a parameter to accept the enum case for each of the keys. 

```Swift
// Add key: Calckeys as a parameter
func makeButton(key: CalcKeys) -> some View {
    return Button {
      print("Tapped Hello World")
    } label: {
        Text(key.rawValue) // use the rawvalue of key for the label
          .font(.system(size: 40))
            .frame(width: 80, height: 80, alignment: .center)
              .foregroundColor(.white)
                .background(.gray)
                  .cornerRadius(40)
      }
}
```

Now modify the display code to show the first row of buttons. The first row should show: Clear, +/-, %, and ÷. 

You started with: 

```Swift
HStack {
    makeButton()
    makeButton()
    makeButton()
    makeButton()
}
```

Which now becomes: 

```Swift
HStack {
    makeButton(key: .clear)
    makeButton(key: .plusMinus)
    makeButton(key: .percent)
    makeButton(key: .divide)
}
```

Challenge set up the rest of the keys. The keys should be arranged like this: 

|:---:|:---:|:---:|:---:|
| C   | +/- | %   | ÷   |
| 7   | 8   | 9   | ×   |
| 4   | 5   | 6   | -   |
| 1   | 2   | 3   | +   |
| 0   | .   | =   |     | 