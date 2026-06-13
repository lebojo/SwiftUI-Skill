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

## 🎨 Design & Resources

- **Resource Management**: ALL colors, images, and icons MUST be declared and managed within `.xcassets` catalogs. No hardcoded hex colors in code.
- **Branding**: Use the system `AccentColor` defined in `Assets.xcassets` as the primary brand color.
- **Dark Mode**: Always ensure assets and colors in `.xcassets` have appropriate variants for Light and Dark modes.
- **Native First**: Prioritize native SwiftUI components (e.g., `List`, `NavigationStack`, `Label`) over custom-built alternatives unless specific requirements dictate otherwise.

## 💾 Data & State Management

- **SwiftData**: Use for local persistence and caching.
- **State**: Prefer `@State`, `@Binding`, and `@Environment` for view-local state. Use `@Observable` for shared business logic in `Core/`.

## 🛠 Engineering Standards

- **Surgical Edits**: When modifying existing code, prioritize minimal, high-impact changes.
- **Protocol Orientation**: Use protocols for services to facilitate testing and modularity.
- **Documentation**: Provide concise documentation for complex logic in `Core/`.
