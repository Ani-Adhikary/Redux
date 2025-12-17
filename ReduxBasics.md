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

