---
name: swiftui-architecture
description: Architecture standards and best practices for native SwiftUI projects. Use when creating, structuring, or reviewing a SwiftUI app — covers directory layout, SwiftData persistence, state management with @Observable, and xcassets resource conventions.
---

# SwiftUI Native Architecture Guidelines

This skill defines architectural standards and best practices for native SwiftUI projects. Adherence to these rules ensures consistency, maintainability, and optimal performance across the application.

## 🏗 Directory Structure

Projects follow a strictly modular structure to separate concerns:

- **`Views/`**: Contains all native SwiftUI views.
    - Subdivide by feature or domain (e.g., `DashboardViews/`, `ProfileViews/`).
    - Keep views small and focused.
- **`Models/`**: Data definitions.
    - **Persistence**: SwiftData entities (e.g., `JobEntity`).
    - **DTOs**: API models and transfer objects (e.g., `TwitchVideo`).
- **`Core/`**: The "Brain" of the application.
    - Logic for API clients, authentication managers, WebSockets, and services.
    - No UI code allowed here.
- **`Extensions/`**: Reusable SwiftUI styles and type enhancements.
    - Standardize common UI patterns (e.g., `CardModifier`).

## ⚡️ Views & Performance

- **Simple Views**: Keep views as simple as possible. Prefer simple data structures (Plain Old Data / POD) over complex structs to reduce compilation times and improve performance.
- **Minimal Data**: Pass only the minimum required data to a View to avoid unnecessary diffing and re-renders.
- **Simple Body**: Keep the `body` property clean. Avoid large closures; instead, pass functions to modifiers (e.g., `.onChange(of: foo, perform: handleFooChange)`).

## 🎨 Design & Resources

- **Resource Management**: ALL colors, images, and icons MUST be declared and managed within `.xcassets` catalogs. No hardcoded hex colors in code.
- **Branding**: Use the system `AccentColor` defined in `Assets.xcassets` as the primary brand color.
- **Dark Mode**: Always ensure assets and colors in `.xcassets` have appropriate variants for Light and Dark modes.
- **Native First**: Prioritize native SwiftUI components (e.g., `List`, `NavigationStack`, `Label`) over custom-built alternatives unless specific requirements dictate otherwise.

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
