# Creating a SwiftUI Project

SwiftUI is a **declarative** framework developed by Apple for building user interfaces. It runs on iOS, iPadOS, watchOS, tvOS, visionOS, and macOS, so you can use it to build applications for any device in the Apple **ecosystem**.

If you have built iOS projects in the past, you may have used **Storyboard**. Storyboard is a graphical tool that lets you build user interfaces visually by dragging, dropping, and arranging elements on the screen.

In SwiftUI you write code that **generates** your user interface, and you can see the results as you work in Xcode’s *Canvas* preview.

---

## Create a SwiftUI Project

1. Choose **File ▸ New ▸ Project** in Xcode.
2. Select **iOS ▸ App** and click **Next**.
3. Fill in the options:

| Setting                 | Value                |
| ----------------------- | -------------------- |
| **Product Name**        | `SwiftUI-Calculator` |
| **Interface**           | SwiftUI              |
| **Language**            | Swift                |
| **Tests**               | None                 |
| **Core Data / Storage** | None                 |

4. Click **Next**, then choose a location to save the project.

Xcode creates a default SwiftUI project.
If you’re used to Storyboards, you’ll notice there’s no `.storyboard` file. Instead, you’ll see **`ContentView.swift`**, which defines a SwiftUI view. Selecting it shows code on the left and a live preview on the right.

---

## Default SwiftUI Code

```swift
import SwiftUI

struct ContentView: View {        // Main screen
    var body: some View {
        VStack {                  // Vertical stack
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundStyle(.tint)
            Text("Hello, world!")
        }
        .padding()                // Outer spacing
    }
}

#Preview {                        // Live preview
    ContentView()
}
```

### What’s happening, step-by-step

1. **`import SwiftUI`** – brings in `View`, `Text`, `Image`, `VStack`, etc.
2. **`ContentView`** conforms to `View`, making it a reusable component.
3. **`body`** returns a hierarchy that SwiftUI renders.
4. **`VStack`** stacks its children vertically, center-aligned by default.
5. **`Image(systemName:)`** displays an SF Symbol (“globe”).
6. **`Text`** shows a simple string.
7. **`.padding()`** adds default (16-pt) spacing around the stack.
8. **`#Preview`** (Xcode 15+) renders the view live in the canvas.

---

## Experiment in the Preview

Try changing the code and watch the Canvas update instantly:

```swift
Text("SwiftUI is the Best!")
    .font(.system(size: 40, weight: .black))
    .padding(40)

Text("Foo Bar")
    .font(.system(size: 30, weight: .light))
```

SwiftUI is composed of *views* (`Text`, `Image`, etc.) modified with *modifiers* (`.font`, `.padding`, `.foregroundStyle`, …). Some modifiers take simple values (`.padding(40)`); others take more detailed arguments (`.font(.system(size: 40))`).

With SwiftUI’s extensive library of views and modifiers, you can create virtually any user interface you can imagine!
