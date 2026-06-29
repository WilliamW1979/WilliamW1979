# William Ward
**C/C++ and C# Developer · .NET Library Author · Remote Technical Specialist**

I've been writing software in C and C++ since 1997 and have been working in C# and .NET as the ecosystem has matured. I build practical, reusable libraries — tools I'd actually want to use myself — and release them publicly for others to build on.

---

## Featured Application — WindowlessCursorLock

**[⬇ Download v1.0.0](https://github.com/WilliamW1979/WindowlessCursorLock/releases/tag/v1.0.0) · [View Source](https://github.com/WilliamW1979/WindowlessCursorLock)**

A system tray utility for Windows that confines the mouse cursor to whichever monitor a borderless-fullscreen game currently occupies — eliminating the accidental alt-tab caused by a cursor drifting to a second monitor mid-session.

Runs silently in the background. No configuration required for most games. Alt-tabbing releases the clip instantly; the cursor is never restricted system-wide.

**Key design decisions:**
- **Heuristic borderless detection** — Compares the focused window's bounds against its monitor's full resolution within a configurable pixel tolerance, and explicitly excludes maximized normal windows, which share similar bounds but are a distinct Win32 window state
- **Dynamic monitor tracking** — If the game is moved to a different monitor via `Win+Shift+Left/Right`, the clip region follows on the next poll cycle with no user action required
- **Force-watch list** — Executables that don't trigger auto-detect (e.g. windowed-mode games) can be registered by selecting from the live process list or browsing to an `.exe` on disk
- **TOML-persisted settings** — Poll interval, startup behavior, and the watched executable list are stored in `%AppData%\WindowlessCursorLock\settings.toml`
- **Optional startup integration** — Windows startup registration is managed entirely from the tray context menu

**Stack:** C# · .NET 10 · Win32 API (P/Invoke) · System Tray (NotifyIcon) · TOML · Windows 10/11

---

## Open Source Libraries

| Repository | Description | Language |
|---|---|---|
| [Logger](https://github.com/WilliamW1979/Logger) | Async/sync logging DLL with optional encryption and five severity levels | C# / .NET 10 |
| [SettingsFile](https://github.com/WilliamW1979/SettingsFile) | INI-style config file wrapper with typed Get/TryGet and optional encryption | C# / .NET 10 |
| [PluginManager](https://github.com/WilliamW1979/PluginManager) | Runtime plugin loader with service injection, hot-reload, and lifecycle management | C# / .NET 10 |
| [Card-Deck-Handler](https://github.com/WilliamW1979/Card-Deck-Handler) | High-performance card hand evaluation library using 64-bit integer representation | C# / .NET 10 |
| [GameTime](https://github.com/WilliamW1979/GameTime) | Universal game time management library with configurable calendars, event scheduling, and binary persistence | C# / .NET 10 |
| [MySqlDatabase](https://github.com/WilliamW1979/MySqlDatabase) | Async-first MySQL library with fluent query builder, transactions, bulk inserts, and schema management | C# / .NET 8+ |

All libraries share a consistent design philosophy: zero external dependencies where possible, pluggable interfaces for extensibility, and clean disposal patterns.

---

## Card Deck Handler

A bit-manipulation-first card game library where an entire hand is packed into a single `ulong`. Every card is one bit. Every evaluation is a mask, a shift, or a hardware population count — no heap allocations, no sorting, no object comparisons.

**Supported games:**
- **Poker** — 5-card draw with full wild card support (jokers, deuces wild, one-eyed jacks, custom rules). Ranks from High Card through Five of a Kind.
- **Texas Hold'em** — equity calculator for 2–10 players. Exact enumeration at the Turn and River, Monte Carlo (100,000 iterations) at earlier streets. All percentages use `decimal` arithmetic to 3 decimal places.
- **Seven Card Stud** — per-card face-up/face-down visibility tracking, discard and draw support, best 5-of-7 evaluation, opponent visible-hand modeling.
- **Gin Rummy** — partition solver minimizes deadwood across all possible run/set groupings. Knock, Gin, and Big Gin detection with full scoring.
- **Rummy 500** — Ace high or low, variable hand sizes (7 or 13 cards), net round scoring.
- **Blackjack** — configurable 1–8 deck shoe, complete basic strategy table, outcome evaluation, Hi-Lo card counting with true count as `decimal`.

**Core bit tricks:**
- Flush detection: `PopCount` on a 16-bit suit block, low-Ace bit stripped to avoid double-counting
- Straight detection: OR all four suit blocks into a 14-bit rank word, slide a 5-bit mask — 10 iterations maximum
- Pair/set/quads: `PopCount` of a rank mask applied across all four suit blocks simultaneously
- Hold'em hand merge: `holeCards | communityCards` — a single bitwise OR

---

## GameTime

A universal game time management library built for distributed game server architectures. Converts real wall-clock time into game time at any configurable ratio and drives a fully customizable calendar — any number of months, day names, week lengths, leap year intervals, and holidays defined entirely through configuration.

**Key design decisions:**
- **Zero required dependencies** — settings, logging, and event storage are all pluggable interfaces. Bring your own INI files, JSON, a database, or hardcoded values. The library compiles and runs with nothing else referenced.
- **Opaque event payloads** — events carry `byte[]` that the library stores and fires without interpreting. Each service serializes its own data however it wants; GameTime just delivers the bytes at the right moment.
- **Thread-safe registration** — designed for a dedicated time server process where network handler threads register and unregister events while the tick loop runs independently. All registry operations are lock-protected.
- **Binary event persistence** — events survive process restarts via a versioned binary format with magic header validation. Corrupt records are skipped and logged rather than crashing the process.
- **Isolated handler invocation** — each subscriber is invoked individually. One throwing handler never prevents others from firing.

**Scheduling capabilities:**
- Once, daily, weekly, monthly, and yearly event repeat modes
- Per-minute bucket dispatch — O(1) event lookup at fire time regardless of how many events are registered
- Daily scheduled tasks firing at specific game hours and minutes
- Per-day-name subscriptions (e.g. fire every time it is Monday in game time)
- Second, minute, hour, day, month, year, and holiday change events

**Time ratio examples:** `1h_24h` (1 real hour = 24 game hours), `30m_1d` (30 real minutes = 1 game day), `1m_1h` (1 real minute = 1 game hour). Supports hours, minutes, and days with decimal values.

---

## MySqlDatabase

An async-first MySQL library for .NET built on [MySqlConnector](https://mysqlconnector.net/). Wraps connection management, parameterized queries, transactions, schema introspection, and bulk operations behind a clean API — no boilerplate, no string-concatenated SQL.

**Key design decisions:**
- **Parameterized by default** — SQL injection is structurally prevented; there is no code path that requires raw string interpolation into queries
- **Pluggable logging** — implements the same `IDbLogger` interface pattern used across the library suite; ships with `NullDbLogger`, `ConsoleDbLogger`, and opt-in `MicrosoftLoggerAdapter` for `Microsoft.Extensions.Logging`
- **Fluent `SelectBuilder`** — composes `WHERE`, `JOIN`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`, and `OFFSET` without string manipulation, and executes directly against a `MySqlDatabase` or open `MySqlDatabaseTransaction`
- **`DbRow` result type** — typed `Get<T>`, `TryGet<T>`, and named helpers (`GetString`, `GetInt`, `GetLong`, etc.) with consistent null handling; no object mapping framework required
- **Schema management** — `EnsureTableAsync` creates tables or adds missing columns to existing ones; `EnsureIndexAsync` adds indexes idempotently; both are safe to call on every startup

**Bulk insert variants:**
- `BulkInsertAsync` — single-statement batch for small sets
- `BulkInsertChunkedAsync` — splits large sets into configurable chunks
- `BulkInsertTransactedAsync` — chunked inside a single transaction for all-or-nothing guarantees

**Transaction support:** `TransactAsync` callback (commit/rollback handled automatically), `BeginTransactionAsync` for manual control, and savepoint API (`SavepointAsync`, `RollbackToAsync`, `ReleaseSavepointAsync`).

---

## Current Project — Journey of the Forgotten *(private)*

A large-scale MMORPG server written in C# with a distributed multi-process architecture designed for high availability and zero-downtime updates.

**Key architectural decisions:**
- **Multi-process server design** — game systems run as independent executables (world simulation, AI, networking, data) communicating over custom binary TCP, so any single process can be updated or restarted without taking the whole server offline
- **AI-driven world simulation** — NPC behavior, ecosystems, and world events are driven by autonomous AI systems running in dedicated processes, keeping simulation load isolated from player-facing services
- **Independent update cycles** — because each subsystem is a separate process, patches can be deployed surgically; a bug fix in the AI layer never requires a full server restart
- **Distributed load splitting** — compute-heavy workloads are distributed across processes rather than bottlenecked in a monolith, allowing horizontal scaling of individual systems

This project applies the same library infrastructure from the public repos above — Logger, SettingsFile, PluginManager, GameTime, and MySqlDatabase are all in active use as internal dependencies.

---

## Skills & Tools

**Languages:** C (1997–present), C++ (1997–present), C# / .NET (current)

**Areas:** Distributed systems architecture, multi-process IPC, library design, bit manipulation and performance optimization, asynchronous programming, plugin architecture, encryption integration, AI/simulation systems, card game logic and equity calculation, game time and calendar systems, binary serialization, database access layers, system administration, Windows & Linux, data operations

**Tools:** Visual Studio, Git, VS Code, Ollama (local AI), Microsoft Office, Google Workspace

---

## About Me

I work independently and remotely. I'm available for contract, part-time, or full-time remote roles in software development, IT support, or technical operations.

📧 WilliamW1979@netscape.net<br>
📍 Berwick, PA (Eastern Time)<br>
💼 [LinkedIn](https://www.linkedin.com/in/william-glenn-ward/)

---

> *All libraries are free for personal and commercial use. Credit appreciated. No warranty provided.*
