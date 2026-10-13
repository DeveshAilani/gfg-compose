# GFG Compose & Android Jetpack Compose Interview Masterclass

An Android application built with **Jetpack Compose** and **Material 3**, featuring modern navigation and integrated tutorials from [GeeksforGeeks](https://www.geeksforgeeks.org/android/android-jetpack-compose-tutorial/).

This README serves as both project documentation and an **ultimate Jetpack Compose Interview Preparation Guide**.

---

## Table of Contents
1. [Project Overview & Tech Stack](#project-overview--tech-stack)
2. [Jetpack Compose Fundamentals](#1-jetpack-compose-fundamentals)
3. [State Management & State Hoisting](#2-state-management--state-hoisting)
4. [Recomposition & Performance Optimization](#3-recomposition--performance-optimization)
5. [Layouts & Modifiers](#4-layouts--modifiers)
6. [Side-Effects API](#5-side-effects-api)
7. [Theming & Material 3](#6-theming--material-3)
8. [Top Android Jetpack Compose Interview Questions & Answers](#7-top-android-jetpack-compose-interview-questions--answers)

---

## Project Overview & Tech Stack

- **Jetpack Compose**: Android's modern declarative UI toolkit.
- **Material 3**: Latest Material Design components and adaptive navigation (`NavigationSuiteScaffold`).
- **WebView Integration**: Loads the official [GeeksforGeeks Jetpack Compose Tutorial](https://www.geeksforgeeks.org/android/android-jetpack-compose-tutorial/) in-app.
- **Architecture**: Unidirectional Data Flow (UDF), Kotlin Coroutines, StateFlow.

---

## 1. Jetpack Compose Fundamentals

### What is Jetpack Compose?
Jetpack Compose is Android’s recommended modern toolkit for building native UI. It simplifies and accelerates UI development on Android by combining a reactive programming model with the concise and easy-to-use features of the Kotlin programming language.

### Imperative vs. Declarative UI
- **Imperative UI (Traditional Android XML)**: You manually instantiate UI widgets (`TextView`, `Button`) and mutate their state throughout their lifecycle using setter methods (e.g., `textView.setText("Hello")`). This often leads to synchronization bugs when UI state outpaces data state.
- **Declarative UI (Jetpack Compose)**: The UI is stateless and reimagined anew each time state changes. You describe *what* the UI should look like for a given state, and the framework takes care of updating it efficiently.

### What is a Composable Function?
A Composable function is a regular Kotlin function annotated with `@Composable`. It tells the Compose compiler that this function is intended to convert data into UI.
```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!")
}
```

---

## 2. State Management & State Hoisting

### What is State in Compose?
State in Compose is any value that can change over time. When state changes, Compose triggers **recomposition** of the affected composables.

### State Holders (`remember` vs `rememberSaveable`)
- `remember`: Stores state in the composition tree during the current lifecycle. Does **not** survive configuration changes (like screen rotation).
- `rememberSaveable`: Automatically saves state in a `Bundle` (via SavedInstanceState), surviving configuration changes and process death.

```kotlin
// Survives recomposition only
var count by remember { mutableStateOf(0) }

// Survives configuration changes (e.g. rotation)
var count by rememberSaveable { mutableStateOf(0) }
```

### What is State Hoisting?
State hoisting is a programming pattern in Compose where you move state out of a composable to make it **stateless**. 
- A **stateful** composable holds its own state (using `remember`).
- A **stateless** composable accepts state as a parameter and exposes events (callbacks like `onValueChange`) so the parent can manage the state.

**Benefits of State Hoisting:**
1. **Single Source of Truth**: Centralizes state management.
2. **Reusability**: Stateless composables are easier to reuse and test.
3. **Testability**: UI can be tested independently of state logic.

---

## 3. Recomposition & Performance Optimization

### What is Recomposition?
Recomposition is the process where Compose re-executes `@Composable` functions when their input state changes, updating the UI tree if outputs have changed.

### The Three Phases of Compose
1. **Composition**: *What* UI to show (executing composables).
2. **Layout**: *Where* to place UI elements (measuring and placing).
3. **Drawing**: *How* to render UI elements (drawing to canvas).

### How to Optimize Performance & Avoid Unnecessary Recompositions
- **Stability**: Compose compiler classifies types as **Stable** or **Unstable**.
  - Primitive types (`Int`, `String`, `Boolean`) are stable.
  - Collections (`List`, `Map`) are considered unstable by default because they are interfaces and might be mutable. Use immutable collections (`kotlinx.collections.immutable`) or annotate classes with `@Stable` / `@Immutable`.
- **Lambda Memoization**: Pass lambdas instead of heavy objects to child composables to enable skipping recomposition.
- **derivedStateOf**: Use `derivedStateOf` when a state calculation depends on another frequently changing state (e.g., scrolling offset to show/hide a scroll-to-top button) to prevent unnecessary recompositions.

---

## 4. Layouts & Modifiers

### Core Layouts
- **Column**: Arranges items vertically (like `LinearLayout` vertical).
- **Row**: Arranges items horizontally (like `LinearLayout` horizontal).
- **Box**: Stacks items on top of each other (like `FrameLayout` or `RelativeLayout`).
- **LazyColumn / LazyRow**: High-performance scrolling lists (equivalent to `RecyclerView`). Only renders visible items.

### Understanding Modifiers
Modifiers let you decorate or augment a composable. **Order matters!**
```kotlin
Box(
    modifier = Modifier
        .padding(16.dp) // Outer padding
        .background(Color.Blue) // Background applied after outer padding
        .padding(16.dp) // Inner padding
)
```

---

## 5. Side-Effects API

Side-effects are changes to the state of the app that happen outside the scope of a composable function.
- **`LaunchedEffect`**: Runs coroutines in a composable's lifecycle. Restarts when keys change.
- **`rememberCoroutineScope`**: Obtains a coroutine scope tied to the composition to launch coroutines from non-composable callback events (e.g., button click).
- **`DisposableEffect`**: Used for effects that require cleanup (e.g., registering and unregistering lifecycle observers or listeners).
- **`rememberUpdatedState`**: Captures a value that may change inside a long-running `LaunchedEffect` without restarting the effect.

---

## 6. Theming & Material 3

Jetpack Compose provides built-in support for Material Design 3 (`MaterialTheme`), including:
- **Color Schemes**: Dynamic color support (Android 12+), dark and light themes.
- **Typography**: Scalable type scales (`headlineLarge`, `bodyMedium`, etc.).
- **Shapes**: Consistent corner radius styling (`shapes.small`, `medium`, `large`).

---

## 7. Top Android Jetpack Compose Interview Questions & Answers

### Q1: What is the difference between Imperative UI and Declarative UI?
- **Imperative UI**: The developer maintains references to UI views and explicitly mutates their properties (`textView.text = "New Text"`) whenever state changes.
- **Declarative UI**: The developer describes the UI hierarchy for any given state. The framework automatically computes differences and updates only the necessary UI components.

### Q2: What are the phases of Jetpack Compose execution?
1. **Composition**: Executes composable functions and builds a tree of UI nodes.
2. **Layout**: Measures and places nodes in 2D space.
3. **Drawing**: Renders nodes onto the screen canvas.

### Q3: Explain State Hoisting and why it is important.
State hoisting is moving local state out of a composable to make it stateless. It promotes the Unidirectional Data Flow (UDF) pattern, making components reusable, testable, and easier to maintain.

### Q4: Why does Compose need the `@Composable` annotation?
It signals to the Compose Compiler plugin that this function can emit UI nodes, participate in recomposition, and is subject to Compose runtime tracking.

### Q5: What is the difference between `remember` and `rememberSaveable`?
- `remember` retains state across recompositions during the lifecycle of the composable, but is lost on configuration changes (e.g., screen rotation).
- `rememberSaveable` stores state in a `Bundle`, preserving it across configuration changes and activity recreations.

### Q6: How does Compose handle Lazy lists (`LazyColumn`) compared to traditional `RecyclerView`?
`LazyColumn` does not require a `ViewHolder` pattern boilerplate. It automatically recycles item composables as they scroll off-screen, handles diverse item types naturally without custom view type IDs, and integrates seamlessly with Kotlin coroutines and states.

### Q7: What is the purpose of `@Stable` and `@Immutable` annotations?
They are hints to the Compose compiler. `@Stable` tells the compiler that the value of the property/class can change, but the runtime will be notified of changes. `@Immutable` guarantees that the properties of the class will never change after creation. This allows Compose to skip recomposition when inputs haven't changed.

### Q8: What is `derivedStateOf` and when should you use it?
`derivedStateOf` converts one or more state objects into another state. It prevents unnecessary recompositions when a state changes frequently (like scroll position), but the UI derived from it only needs to update when a threshold is crossed (e.g., showing a button after scrolling past 500px).
