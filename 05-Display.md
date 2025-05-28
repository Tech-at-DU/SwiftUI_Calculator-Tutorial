# Display

Before working on the display, let’s set the **background color**. We’ll add a `ZStack`.
Where `HStack` and `VStack` arrange views horizontally and vertically, `ZStack` arranges them front-to-back (perpendicular to the screen).

Wrap the entire `VStack` in a `ZStack`:

```swift
ZStack {                      // back-to-front layout
    VStack {
        HStack { … }
        HStack { … }
        HStack { … }
        HStack { … }
        HStack { … }
    }
}                             
```

Now set the background to black by inserting `Color.black` as the bottom layer:

```swift
ZStack {
    Color.black
    VStack { … }
}
```

That colors most of the screen, but you’ll still see white bars in the **safe area** (where the notch, status bar, or home indicator lives). Tell SwiftUI to extend into the safe area:

```swift
ZStack {
    Color.black.ignoresSafeArea()   // fills the whole display
    VStack { … }
}
```

The background should now cover the entire screen.

---

## The Display

The calculator’s display shows the current input and results. A `Text` view works well:

```swift
VStack {
    Spacer()   // pushes everything downward

    Text(env.display)
        .font(.system(size: 60))
        .foregroundColor(.white)
        .frame(maxWidth: .infinity,               // stretch horizontally
               maxHeight: 100,                    // fixed height
               alignment: .bottomTrailing)        // bottom-right corner
        .padding()                                // same inset as buttons

    HStack { … }
    HStack { … }
    HStack { … }
    HStack { … }
    HStack { … }
}
```

* `font(.system(size: 60))` makes the digits large.
* The `frame` expands horizontally but caps height at 100 pt.
* `alignment: .bottomTrailing` pins the numbers to the lower-right, matching Apple’s calculator.
* `.padding()` adds outer spacing so the text aligns with button edges.

---

## Spacers

Without a spacer, `VStack` centers its children vertically, leaving unwanted space above the keypad.
Placing `Spacer()` **before** the `Text` pushes the entire stack down, mimicking the real calculator layout.

`Spacer()` automatically consumes any extra space, but you can give it a fixed height if needed:

```swift
Spacer(minLength: 40)   // exactly 40 pt tall
```

Now the UI has:

* A full-screen black background.
* A right-aligned display at the top of the keypad.
* Buttons arranged neatly below.

You’re ready to wire up the buttons and calculation logic!
