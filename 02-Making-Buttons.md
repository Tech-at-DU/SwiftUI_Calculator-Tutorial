Below is a **proof-read and lightly polished** version with spelling, grammar, and minor accuracy fixes.
(➜ Changes are inline; key corrections are **bolded**.)

---

# Making Buttons

The calculator needs some buttons—**20, in fact!** While the buttons are mostly the same, they also differ:

* Each button shows different label text.
* Some buttons are light gray, others dark gray, and the operators are orange.

Delete everything inside the `VStack`, then add a button:

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Button {
                print("Tapped Hello World")
            } label: {
                Text("Hello World")
            }
        }
        .padding()
    }
}

#Preview {
    ContentView()
}
```

Here the button uses **two trailing closures**:
the *first* closure is the **action** and the *second* is the **label**.

You could also write it with explicit parameter labels:

```swift
Button(
    action: {                         // action closure
        print("Tapped Hello World")
    },
    label: {                          // label closure
        Text("Hello World")
    }
)
```

`Button` has several initializers; we’ll stick with the first style.

---

## Making a Calculator Button

A calculator button needs a background color, rounded corners, and a fixed size.
We can achieve all of that by modifying the `Text` label:

```swift
Button {
    print("Tapped 9")
} label: {
    Text("9")
        .font(.system(size: 40))
        .frame(width: 80, height: 80)
        .foregroundColor(.white)
        .background(Color.gray)
        .cornerRadius(40)
}
```

**Experiment with the modifiers:**

* Change the font size.
* Adjust the `frame` dimensions.
* Modify `foregroundColor` (text) or `background`.

  * Swift’s built-in colors (`.black`, `.blue`, etc.) work, or try custom values:

    * `Color(red: 1.0, green: 0.8, blue: 0.1)` (yellow)
    * `Color(hue: 1.0, saturation: 0.5, brightness: 1.0)` (tomato)
* Change the `cornerRadius`.

---

## Laying Out Multiple Buttons

Copy the button four times. If you place them directly inside the `VStack`, they stack **vertically**. We need them **horizontally**, so nest them in an `HStack`:

```swift
VStack {
    HStack {
        Button { /* ... */ } label: { /* ...7... */ }
        Button { /* ... */ } label: { /* ...8... */ }
        Button { /* ... */ } label: { /* ...9... */ }
        Button { /* ... */ } label: { /* ...×... */ }
    }
}
```

The outer `VStack` arranges rows vertically; each inner `HStack` arranges its four buttons horizontally. Five such rows will give us a 5×4 grid.

---

## Cleaning This Up

Hard-coding 10 lines per button means **40 lines** for one row and **200 lines** for all 20 buttons—far too much duplication.

Because SwiftUI is just Swift, we can create a **helper function**:

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            HStack {
                makeButton(label: "7")
                makeButton(label: "8")
                makeButton(label: "9")
                makeButton(label: "×")
            }
            // additional rows here …
        }
        .padding()
    }

    /// Returns a styled calculator button.
    func makeButton(label: String) -> some View {
        Button {
            print("Tapped \(label)")
        } label: {
            Text(label)
                .font(.system(size: 40))
                .frame(width: 80, height: 80)
                .foregroundColor(.white)
                .background(Color.gray)
                .cornerRadius(40)
        }
    }
}
```

**Challenge:** further refactor `makeButton` to accept parameters for color and action so you can reuse it for the orange operator buttons as well.

With a small helper like this, your UI code stays concise and maintainable—even with 20 buttons!
