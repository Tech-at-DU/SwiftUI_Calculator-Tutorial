# Observable Objects

Although you *can* put logic inside a SwiftUI view, that isn’t best practice.
Views should focus on presentation, while models and controllers handle the data and business rules. Apple encourages MVC / MVVM, and we’ll follow the same pattern.

We’ll keep all calculation logic in a separate class and expose its state to the view. Two SwiftUI concepts make this easy:

1. `ObservableObject` and `@Published`
2. `EnvironmentObject`

---

## 1. `ObservableObject`

A type that adopts the **`ObservableObject`** protocol can emit change notifications.
Mark each value you want to broadcast with **`@Published`**:

```swift
final class GlobalState: ObservableObject {
    // MARK: Published UI state
    @Published var display = "0"
    // …other properties and functions…
}
```

Whenever `display` changes, any view that observes this object redraws automatically.

---

## 2. `EnvironmentObject`

`EnvironmentObject` is a *property wrapper* that injects a shared `ObservableObject` into the view hierarchy:

```swift
struct ContentView: View {
    @EnvironmentObject var env: GlobalState   // shared model
    // …
}
```

---

## 3. Create `GlobalState`

Add a new Swift file named **`GlobalState.swift`** and start with a simple proof of concept:

```swift
final class GlobalState: ObservableObject {
    @Published var display = "1.618"
    // more logic coming soon…
}
```

---

## 4. Inject `GlobalState` into your app

You must supply the environment object **twice**:

1. **Canvas preview**
2. **App entry point** (when the app runs on a device or simulator)

### a. Preview

Open **`ContentView.swift`** and update the preview:

```swift
#Preview {
    ContentView()
        .environmentObject(GlobalState())   // inject model for Canvas
}
```

### b. App entry point

Open **`SwiftUI_CalculatorApp.swift`** (or whatever Xcode named it) and inject the same object:

```swift
@main
struct SwiftUI_CalculatorApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(GlobalState())   // inject for the real app
        }
    }
}
```

---

## 5. Display the published value

Back in **`ContentView`**, declare the environment object and use it:

```swift
struct ContentView: View {
    @EnvironmentObject var env: GlobalState

    var body: some View {
        Text(env.display)   // shows “1.618” initially
    }
}
```

Whenever `env.display` changes, the text updates automatically.

---

## 6. Handling key presses

For now, just append each key’s raw value to the display string.

### `GlobalState`

```swift
final class GlobalState: ObservableObject {
    @Published var display = "0"

    /// Update the display when a key is pressed.
    func keyPressed(_ key: CalcKey) {        // note: CalcKey, not CalcKeys
        display += key.rawValue
    }
}
```

### Update `makeButton` in **`ContentView`**

```swift
func makeButton(
    key: CalcKey,
    color: Color = .gray,
    width: CGFloat = 80,
    height: CGFloat = 80
) -> some View {
    Button {
        env.keyPressed(key)                  // send key to the model
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

---

### Test

1. The display starts at **`0`**.
2. Tap **8** → display shows **`08`** (we haven’t trimmed the leading zero yet).
3. Each tap appends more characters, and the view updates instantly.

> In a full calculator you would replace the leading zero, but for this proof-of-concept appending is fine.

---

Now you have:

* A **model/controller** (`GlobalState`) that owns the calculator state.
* A **view** (`ContentView`) that reads published values and forwards user events back to the model.
* A clean separation of concerns that scales to more complex features.

*(Next step: implement real arithmetic so the display shows calculated results.)*
