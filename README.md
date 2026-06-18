<h1 align="center">Tic Tac Toe Backend</h1>

<p align="center">
  <strong>A lightweight Ktor backend for a real-time multiplayer Tic Tac Toe game.</strong>
</p>

<p align="center">
  This server hosts the WebSocket game session used by the Android client. It keeps the shared 3x3 board state in memory, assigns players as <code>X</code> and <code>O</code>, validates turns, broadcasts updates, and starts a fresh round after a win or draw.
</p>

<p align="center">
  Repository: <a href="https://github.com/mauli-waghmore/tic-tac-toe-backend">mauli-waghmore/tic-tac-toe-backend</a>
</p>

<p align="center">
  <img alt="Ktor" src="https://img.shields.io/badge/Ktor-2.3.12-087CFA?logo=ktor&logoColor=white">
  <img alt="Kotlin" src="https://img.shields.io/badge/Kotlin-2.0.0-7F52FF?logo=kotlin&logoColor=white">
  <img alt="WebSocket" src="https://img.shields.io/badge/WebSocket-Realtime-4B5563">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green.svg">
</p>

---

## Highlights

- **Real-time multiplayer** - two connected players receive the same game state over a Ktor WebSocket route.
- **Simple game session** - the backend assigns `X` and `O`, tracks connected players, validates moves, and detects wins or draws.
- **State broadcasting** - `MutableStateFlow` publishes every `GameState` change to active WebSocket sessions.
- **Serializable payloads** - Kotlinx Serialization keeps turn actions and game state messages typed and compact.
- **Auto reset** - completed rounds reset after a short delay so players can continue without restarting the server.
- **Ktor server setup** - Netty, WebSockets, content negotiation, YAML config, and call logging are configured through Ktor plugins.

## Tech Stack

| Layer | Choice |
|---|---|
| Platform | JVM backend |
| Language | [Kotlin 2.0.0](https://kotlinlang.org) |
| Server | [Ktor 2.3.12](https://ktor.io) + Netty |
| Realtime | Ktor Server WebSockets |
| Serialization | Kotlinx Serialization JSON |
| State | Kotlin coroutines, `MutableStateFlow`, `ConcurrentHashMap` |
| Build | Gradle Kotlin DSL |

## Getting Started

```bash
git clone https://github.com/mauli-waghmore/tic-tac-toe-backend.git
cd tic-tac-toe-backend
./gradlew run
```

The server starts on:

```text
http://0.0.0.0:8080
```

The game WebSocket endpoint is:

```text
ws://localhost:8080/play
```

Clients should send turns in this format:

```text
make_turn#{"x":0,"y":0}
```

The server broadcasts serialized `GameState` updates to every connected player.

Other commands:

```bash
./gradlew test       # run unit tests
./gradlew build      # compile, test, and package the server
./gradlew installDist
```

## Project Structure

```text
src/
  main/
    kotlin/example/com/
      Application.kt        Ktor entry point and plugin wiring
      SocketRoute.kt        /play WebSocket route and incoming turn parsing
      models/
        GameState.kt        serializable board state sent to clients
        MakeTurn.kt         serializable turn action received from clients
        TicTacToeGame.kt    player assignment, turn validation, win checks, broadcasts
      plugins/
        Monitoring.kt       request logging setup
        Routing.kt          route registration
        Serialization.kt    JSON content negotiation
        Sockets.kt          WebSocket server configuration
    resources/
      application.yaml      Ktor module and deployment port
      logback.xml           logging configuration
  test/
    kotlin/example/com/     server tests

build.gradle.kts           Gradle plugins, dependencies, and application setup
settings.gradle.kts        project name
gradle.properties          Kotlin, Ktor, and Logback versions
```

## Conventions

- **Keep game rules in `TicTacToeGame`** - route handlers should stay thin and delegate player state, validation, and reset behavior to the game model.
- **Broadcast through state updates** - change `GameState` through the state flow so connected clients receive consistent updates.
- **Preserve the WebSocket protocol** - Android clients expect `make_turn#{...}` messages and serialized `GameState` responses.
- **Treat the server as an in-memory session** - game state is not persisted and resets when the process restarts.
- **Verify before merging** - run `./gradlew build` for backend changes.

## Contributing

Contributions are welcome. Whether it is a bug fix, protocol improvement, test coverage, or gameplay enhancement:

1. **Fork** the repository and create a branch from `main` (`feat/your-feature` or `fix/your-fix`)
2. Make focused changes that follow the project conventions above
3. Verify locally with `./gradlew build`
4. **Open a pull request** with a clear description of what changed and why

Found a bug or have a suggestion? [Open an issue](https://github.com/mauli-waghmore/tic-tac-toe-backend/issues) and include steps to reproduce, client details, and any relevant server logs.

## License

This project is open source under the [MIT License](LICENSE) - (c) 2026 [Mauli Waghmore](https://github.com/mauli-waghmore).

---

Made for classic game lovers.
