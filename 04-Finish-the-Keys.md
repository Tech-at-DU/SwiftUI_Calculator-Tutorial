# Finish the Keypad

Below is one solution to the challenge from the previous section.

```swift
//
//  ContentView.swift
//  SwiftUI-Calculator-Solution
//
//  Created by Mitchell Hudson on 5/27/25.
//

import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack {
            HStack {
                makeButton(key: .clear)
                makeButton(key: .plusMinus)
                makeButton(key: .percent)
                makeButton(key: .divide)
            }

            HStack {
                makeButton(key: .seven)
                makeButton(key: .eight)
                makeButton(key: .nine)
                makeButton(key: .multiply)
            }

            HStack {
                makeButton(key: .four)
                makeButton(key: .five)
                makeButton(key: .six)
                makeButton(key: .subtract)
            }

            HStack {
                makeButton(key: .one)
                makeButton(key: .two)
                makeButton(key: .three)
                makeButton(key: .add)
            }

            HStack {
                makeButton(key: .zero)
                makeButton(key: .decimal)
                makeButton(key: .equals)
            }
        }
        .padding()
    }

    /// Helper that builds a single calculator button.
    func makeButton(key: CalcKey) -> some View {
        Button {
            print("Tapped \(key.rawValue)")
        } label: {
            Text(key.rawValue)
                .font(.system(size: 40))
                .frame(width: 80, height: 80)
                .foregroundColor(.white)
                .background(Color.gray)
                .cornerRadius(40)
        }
    }
}

#Preview { ContentView() }
```

Looking at Apple’s Calculator, you’ll notice the operator keys and digit keys use different colors.

The last row still looks awkward. In this solution we’ll make the **0** button span two columns, just like the classic iOS calculator.

---

## Adding Configuration Parameters

Because the `Text` label is what the user sees, we need to set its background color, width, and height. Let’s add parameters to `makeButton`.

```swift
func makeButton(
    key: CalcKey,
    color: Color = .gray,        // key color
    width: CGFloat = 80,         // width
    height: CGFloat = 80         // height
) -> some View {
    Button {
        print("Tapped \(key.rawValue)")
    } label: {
        Text(key.rawValue)
            .font(.system(size: 40))
            .frame(width: width, height: height)
            .foregroundColor(.white)
            .background(color)
            .cornerRadius(width / 2)
    }
}
```

Defaults cover common cases, so most calls stay short.

---

### A note on `.darkGray`

`Color(.darkGray)` is unavailable directly in SwiftUI, but you can bridge from UIKit:

```swift
Color(UIColor.darkGray)
```

---

### Example layout with colors and a wide zero

```swift
VStack {
    HStack {
        makeButton(key: .clear,     color: Color(UIColor.darkGray))
        makeButton(key: .plusMinus, color: Color(UIColor.darkGray))
        makeButton(key: .percent,   color: Color(UIColor.darkGray))
        makeButton(key: .divide,    color: .orange)
    }

    HStack {
        makeButton(key: .seven)
        makeButton(key: .eight)
        makeButton(key: .nine)
        makeButton(key: .multiply, color: .orange)
    }

    HStack {
        makeButton(key: .four)
        makeButton(key: .five)
        makeButton(key: .six)
        makeButton(key: .subtract, color: .orange)
    }

    HStack {
        makeButton(key: .one)
        makeButton(key: .two)
        makeButton(key: .three)
        makeButton(key: .add, color: .orange)
    }

    HStack {
        makeButton(key: .zero,   width: 168)       // spans two columns
        makeButton(key: .decimal)
        makeButton(key: .equals, color: .orange)
    }
}
```

Because Swift supports **named parameters** and **default values**, you only supply what you need—clean and concise.

---

Now your keypad looks much closer to the real iOS calculator, with colored operator keys and a wide zero button.
