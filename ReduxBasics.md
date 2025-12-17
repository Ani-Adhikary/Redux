<h1>Redux Basics </h1>

<h2>1️⃣ What is Redux? (Core Idea) </h2>

Redux is a predictable state management pattern.

<h3>Key Philosophy </h3>

<b>Single Source of Truth + Unidirectional Data Flow </b>

In simple words:

- Your entire app state lives in one place

- UI cannot modify state directly

- UI sends actions

- Actions go through a reducer

- Reducer creates a new state

- UI updates automatically

<h2>2️⃣ Redux Building Blocks (Very Important) </h2>

<h3>1. State </h3>

A struct that represents everything your screen/app needs

```
struct AppState {
    var count: Int = 0
}
```

<h3>2. Action </h3>

An enum describing what happened

```
enum AppAction {
    case increment
    case decrement
    case reset
}
```

Actions do not contain logic
They only describe intent


<h3>3. Reducer </h3>

A pure function that:

- Takes current state

- Takes an action

- Returns a new state

```
func appReducer(state: AppState, action: AppAction) -> AppState {
    var state = state

    switch action {
    case .increment:
        state.count += 1
    case .decrement:
        state.count -= 1
    case .reset:
        state.count = 0
    }

    return state
}

```

❗ Reducer rules:

- No async code

- No side effects

- No mutation outside state copy

<h3>4. Store </h3>

The brain of Redux:

- Holds the state

- Exposes dispatch(action)

- Notifies UI when state changes

```
  final class Store: ObservableObject {
    @Published private(set) var state: AppState

    init(initial: AppState) {
        self.state = initial
    }

    func dispatch(_ action: AppAction) {
        state = appReducer(state: state, action: action)
    }
}
```

<h3>5. View (SwiftUI) </h3>

- Reads state from Store

- Dispatches actions

<h2>3️⃣ Redux Data Flow (One Line) </h2>


```
View → Action → Reducer → New State → View
```

- No shortcuts.
- No two-way binding.
- Very predictable.

<h2>4️⃣ Demo SwiftUI Redux App (Counter App) </h2>

<h3>📁 Project Structure </h3>

```
ReduxDemo/
 ├─ AppState.swift
 ├─ AppAction.swift
 ├─ AppReducer.swift
 ├─ Store.swift
 └─ ContentView.swift

```

<h3>✅ AppState.swift </h3>

```
struct AppState {
    var count: Int = 0
}
```

<h3>✅ AppAction.swift </h3>

```
enum AppAction {
    case increment
    case decrement
    case reset
}
```

<h3>✅ AppReducer.swift </h3>

```
func appReducer(state: AppState, action: AppAction) -> AppState {
    var state = state

    switch action {
    case .increment:
        state.count += 1
    case .decrement:
        state.count -= 1
    case .reset:
        state.count = 0
    }

    return state
}
```

<h3>✅ Store.swift </h3>

```
import SwiftUI

final class Store: ObservableObject {
    @Published private(set) var state: AppState

    init(initial: AppState) {
        self.state = initial
    }

    func dispatch(_ action: AppAction) {
        state = appReducer(state: state, action: action)
    }
}

```

<h3>✅ ContentView.swift </h3>

```

import SwiftUI

struct ContentView: View {
    @StateObject private var store = Store(initial: AppState())

    var body: some View {
        VStack(spacing: 24) {
            Text("Redux Counter")
                .font(.largeTitle.bold())

            Text("\(store.state.count)")
                .font(.system(size: 60, weight: .bold))

            HStack(spacing: 20) {
                Button("−") {
                    store.dispatch(.decrement)
                }

                Button("+") {
                    store.dispatch(.increment)
                }
            }
            .font(.title)

            Button("Reset") {
                store.dispatch(.reset)
            }
            .foregroundColor(.red)
        }
        .padding()
    }
}

```

<h2>5️⃣ Where Are Side Effects in Redux? </h2>

Redux itself does not allow side effects inside reducers.

Examples of side effects:

- API calls

- Firebase reads/writes

- Delays / timers

- Analytics

- Notifications

In SwiftUI Redux, common solutions:

1. Middleware

2. Thunk-style async functions

3. ViewModel → dispatch results back

Example (simple async side effect):

```
func fetchData(store: Store) {
    Task {
        let result = await apiCall()
        store.dispatch(.dataLoaded(result))
    }
}

```

Reducers stay pure 👍

<h2>6️⃣ Why Redux Is Useful (Especially for You) </h2>

For apps like DharmaConnect:

- Favorites

- Auth state

- Subscription state

- Wishlist

- Premium / Free access

- Offline cache

Redux gives:

- One global app state

- Easy debugging

- Easy scaling

- Predictable UI updates

<h2>7️⃣ Redux vs MVVM (Quick Comparison) </h2>

MVVM	Redux <br />
Multiple ViewModels	Single Store  <br />
Two-way binding	One-way flow  <br />
Hard to track changes	Easy to debug  <br />
Good for small screens	Excellent for large apps  <br />


<h3>👉 Best approach: </h3>

SwiftUI + Redux-style Store + Feature reducers
(Exactly what you’re moving toward)

If you want next:

- ✅ Redux with Firebase

- ✅ Redux with Navigation

- ✅ Redux + Middleware

- ✅ Scalable Redux for DharmaConnect

  
