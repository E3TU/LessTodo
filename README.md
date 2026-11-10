# LessTodo

A clean, Material 3 todo app for Android built with Jetpack Compose. Fewer taps, less clutter — just what you need to get things done.

![Kotlin](https://img.shields.io/badge/Kotlin-2.2.10-7F52FF?logo=kotlin&logoColor=white)
![Compose](https://img.shields.io/badge/Compose-BOM%202026.02.01-4285F4?logo=jetpackcompose&logoColor=white)
![minSdk](https://img.shields.io/badge/minSdk-24-3DDC84?logo=android&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue)

## Screenshots

<p align="center">
<img src="docs/todos.png" width="32%" alt="Active todos" />
<img src="docs/completed.png" width="32%" alt="Completed todos" />
<img src="docs/sheet.png" width="32%" alt="Add todo" />
</p>

<p align="center">
<em>Active todos · Completed todos · Add</em>
</p>

## Features

- **Separate tabs** for active and completed todos, with a slide-and-fade transition between them
- **Add and edit** todos in a modal bottom sheet — title plus an optional description
- **One-tap complete** — toggling a checkbox moves the todo to the Completed tab, with animated row transitions
- **Material You** dynamic color on Android 12+, toggleable from Settings
- **Dark mode** that follows your system theme, with a matching launch screen so there's no white flash
- **Edge-to-edge** layout that respects system bars and the keyboard
- **Offline-first** — everything lives in a local Room database, no network permission required

## Tech stack

| Layer | Library | Version |
|---|---|---|
| UI | Jetpack Compose (BOM) | 2026.02.01 |
| Design system | Material 3 | BOM-managed |
| Navigation | Navigation Compose | 2.10.0 |
| Persistence | Room | 2.8.5 |
| Preferences | DataStore | 1.1.7 |
| Async | Kotlin Coroutines + Flow | — |
| Build | AGP / Kotlin / KSP | 9.5.1 / 2.2.10 / 2.3.9 |

Compose animations, `ViewModel` + `StateFlow`, and single-Activity architecture throughout.

## Project structure

```
app/src/main/java/com/example/todoapp/
├── MainActivity.kt              # Single activity, edge-to-edge, theme wrapper
└── ui/
    ├── home/HomeScreen.kt       # Tabbed todo list, rows, FAB
    ├── navigation/              # NavHost, routes, drawer, top bar
    ├── settings/                # Settings screen + DataStore wrapper
    ├── theme/                   # Color, Type, Material 3 theme
    └── todo/                    # Entity, DAO, Room DB, repository, ViewModel, bottom sheet
```

## Architecture

Single activity, unidirectional data flow:

```
Room DB ──▶ TodoRepository ──▶ TodoViewModel ──▶ StateFlow<List<Todo>> ──▶ Compose UI
```

- `TodoDao` exposes a `Flow` that Room updates automatically on any write.
- `TodoViewModel` converts it to a `StateFlow` with `stateIn(WhileSubscribed(5000))`.
- The UI collects with `collectAsStateWithLifecycle()`, so flows stop while the app is backgrounded.
- Theme preference is stored in DataStore and read as a `Flow`, so the Material You toggle applies instantly.

## Getting started

### Prerequisites

- Android Studio (latest stable)
- JDK 17+
- Android SDK with API 37 platform installed

### Build

```bash
git clone https://github.com/E3TU/LessTodo.git
cd LessTodo
./gradlew assembleDebug
```

The APK lands in `app/build/outputs/apk/debug/`.

You'll need a `local.properties` file at the repo root with your SDK path:

```properties
sdk.dir=/path/to/Android/Sdk
```

Android Studio creates this automatically on first open. It is git-ignored and must not be committed.

### Install

```bash
./gradlew installDebug
```

## Project status

Early development, actively worked on. Data is stored locally only — there is no sync, export, or backup yet.

## License

MIT — see [LICENSE](LICENSE).
