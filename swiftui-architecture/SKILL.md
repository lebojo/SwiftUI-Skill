---
name: swiftui-architecture
description: Architecture standards and best practices for native SwiftUI projects. Use when creating, structuring, or reviewing a SwiftUI app — covers directory layout, SwiftData persistence, state management with @Observable, and xcassets resource conventions.
---

# SwiftUI Native Architecture Guidelines

This skill defines architectural standards and best practices for native SwiftUI projects. Adherence to these rules ensures consistency, maintainability, and optimal performance across the application.

## 🏗 Directory Structure & Clean Architecture

Projects follow a strictly modular structure to separate concerns and adhere to **Clean Architecture** principles:

- **`Views/`**: Contains all native SwiftUI views.
    - Subdivide by feature or domain (e.g., `DashboardViews/`, `ProfileViews/`).
    - Keep views small and focused. Always decompose a view into the smallest possible subviews.
    - **Subview Folders**: Whenever a view needs to be split into subviews, create a dedicated subfolder for it: `Views/MyNewView/MyNewView.swift`. Place that view AND all of its associated subviews inside this folder.
        - **Exception**: Top-level navigation views (e.g., the main `TabView`) may remain directly in `Views/` without their own folder.
    - **Check Before Creating**: Before creating any new view, verify that a similar one does not already exist. If it does, always try to factorize as much as possible.
        - **Factored Components**: When a view is successfully factored into something reusable, give it an abstract, generic name and place it in `Views/Components/`.
- **`Models/`**: Data definitions.
    - **`UI/`**: Models specifically tailored for view representation. These models should only contain what the UI needs.
    - **`Data/`**: Models representing core business entities, DTOs, or database entities (Back models).
    - **Separation**: Always maintain a clear distinction between models in these two folders. Map between them as needed.
- **`Core/`**: The "Brain" of the application.
    - Logic for API clients, authentication managers, WebSockets, and services.
    - No UI code allowed here.
- **`Extensions/`**: Reusable SwiftUI styles and type enhancements.

## ⚡️ Type Selection & Performance

- **Prioritize Value Types**: Follow this hierarchy when choosing a type:
    1. **`enum`**: Use for almost everything (state, logic, configurations). If a task can be done with an enum, do it with an enum.
    2. **`struct`**: Use when you need to store data that doesn't fit well into enum associated values or when you need unique data structures.
    3. **`class`**: Use ONLY as a last resort when reference semantics are strictly required (e.g., specific lifecycle needs or shared mutable state that cannot be handled by an actor).
- **`actor`**: Use actors when it is the most optimized way to handle shared mutable state and concurrency (e.g., secure storage, session management).
- **Nested Enums**: Do not hesitate to define `enum` types within Views or Structs for local state (e.g., `enum Status`, `enum ViewState`, `enum Style`) that only makes sense in that specific context.
- **Simple Views**: Keep views as simple as possible. Prefer simple data structures (Plain Old Data / POD) over complex structs to reduce compilation times and improve performance.
- **Minimal Data**: Pass only the minimum required data to a View to avoid unnecessary diffing and re-renders.

## 🎨 Design & Resources

- **Styling Abstraction**: Abstract all colors and fonts using extensions to ensure a type-safe and semantic design system.
    - **Color Extensions**: Extend `Color` with static properties for all brand and semantic colors (e.g., `static let brandPrimary = Color("brandPrimary")`).
    - **Font Extensions**: Extend `Font` with static properties or functions for the app's typography scale (e.g., `static let titleLarge = Font.system(size: 24, weight: .bold)`).
    - **Usage**: Always use these extensions in Views (e.g., `.foregroundColor(.brandPrimary)`) instead of string literals or raw system calls.
- **Resource Management**: ALL colors, images, and icons MUST be declared and managed within `.xcassets` catalogs. No hardcoded hex colors in code.
- **Branding**: Use the system `AccentColor` defined in `Assets.xcassets` as the primary brand color.
- **Native First**: Prioritize native SwiftUI components over custom-built alternatives.

## 💾 Data & State Management

- **SwiftData**: Use for local persistence and caching.
- **State Management**:
    - Prefer `@State`, `@Binding`, and `@Environment` for view-local state.
    - Use `@Observable` for shared business logic in `Core/`.
    - **Property Wrappers**: Keep `@State` and other `DynamicProperty` types (like `@AppStorage`, `@Query`) within SwiftUI Views. Do not use them inside business logic classes or other non-view objects.
- **State Visibility**: Always declare `@State` properties as `private`.

## 🛠 Engineering Standards

- **Surgical Edits**: When modifying existing code, prioritize minimal, high-impact changes.
- **Protocol Orientation**: Use protocols for services to facilitate testing and modularity.
- **Documentation**: Provide concise documentation for complex logic in `Core/`.

## 🖋 Coding Standards

- **Naming Conventions**:
    - `class`, `struct`, `enum`: `CamelCase` (e.g., `RandomStruct`).
    - `var`, `func`: `camelCase` (e.g., `randomStructVar`, `randomFunc()`).
- **Property Wrapper Order**: Maintain the following order for declarations (use `private` where specified):
    1. `@LazyInjectService private var ...` (ATTENTION: See point 8)
    2. `@AppStorage private var ...`
    3. `@Environment private var ...`
    4. `@EnvironmentObject private var ...`
    5. `@State private var ...`
    6. `@StateObject private var ...`
    7. `@FocusState private var ...`
    8. `@Binding var ...`
    9. `@ObservedObject var ...`
    10. `@ObservedRealmObject var ...`
    11. `@ObservedResults private var ...`
    12. `let ...`
    13. `private var ... { ... }` (Computed properties)
- **Concurrency & Safety**:
    - When creating a new `class`, `struct`, or `enum`, annotate with `Sendable` or `@MainActor` where appropriate.
    - Use `@MainActor` as a last resort and only for UI-linked objects.
