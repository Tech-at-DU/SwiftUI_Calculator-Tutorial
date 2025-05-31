# Creating and Evaluating Expressions 

We need our calculator to generate a mathematical expression and evaulate it. This is what the Calculator app does with some exceptions. 

There are a few edge cases such as %, +/-, the decimal, and a couple more. For now lets concentrate on getting simple expressions to worj with integers. 

Luckily for us Apple provides: `NSExpression`. We can use: `NSExpression(format:argumentArray:)`. Read about it here: 

https://developer.apple.com/documentation/foundation/nsexpression/init(format:argumentarray:)

Here is a good introduction to the topic: https://medium.com/@elifedman/understanding-nsexpression-in-swift-a-beginners-guide-50b7baa36271

## Entering the Expression

The first step, before we can evaluate the expression, is entereing it with the keys. 

Modify `keyPressed(key:)` in `GlobalState`. 

```Swift
func keyPressed(_ key: CalcKey) {
  switch key {
  case .clear:
    if display == "0" {
      return
    }
    display.removeLast(1)
    if display.isEmpty {
      display = "0"
    }
    break
  case .decimal:
    //
    break
  case .plusMinus:
    break
  case .one, .two, .three, .four, .five, .six, .seven, .eight, .nine, .zero:
    if display == "0" {
      display = key.rawValue
    } else {
      display += key.rawValue
    }
    break
  case .divide, .multiply, .subtract, .add, .percent:
    display += key.rawValue
    break
  case .equals:
    
    break
  }
}
```

This should allow you to enter something like: "8×3+2". The decimal, equal and, percent have not been handled yet. 

The clear key should work, removing the last character of the displayed expression.

Also, typing an extra operator will not replace the last operator, but append another operator, for example, "8×3+2+×-", we need to fix this in the future. 

For now this will allow us to try out `NSExpression`, later we will return and clean up the other issues. 

## Evaluate an expression with NSExpression

Add another method to `GlobalState`. 

```Swift
func evaluate() -> Double {
    let expr = display
      .replacingOccurrences(of: "×", with: "*")
      .replacingOccurrences(of: "÷", with: "/")
      .replacingOccurrences(of: "−", with: "-")
    
    print(expr)
    
    let answer = (NSExpression(format: expr)
      .expressionValue(with: nil, context: nil) as? NSNumber)?.doubleValue ?? 0
    
    return answer
  }
```

This function uses display as the expression and returns a Double. 

It starts by replacing the characters: "×", "÷", and "−" with the cahracters: "*", "/", and "-". 

NOTE! I had a big problem with "-" and "-", While these characters look the same, they are different! The first is the hypehn, and the second is the minus symbol. `NSExpression` wants the "-" (minus symbol), if it sees the hyphen in an expression is crashes!  

Last, it evaluates the expression string to a Double, and returns the Double. 

Find where the `case .equals` is handedled in `keyPressed(_ key:)`.

```Swift
case .equals:
    let answer = evaluate()
    display = String(answer)
    break
```

Here, when press "=" we call `evaluate()` which returns a double, which is the answer to the expression displayed, next we convert the `answer` Double to a String and set `display`.

With this in place, you should be able to enter: "3*2+7", and pressing "=" see "13.0"

This is pretty good so far. 

