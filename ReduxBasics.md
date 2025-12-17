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
