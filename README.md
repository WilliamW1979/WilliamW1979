# William Ward
**C/C++ and C# Developer · .NET Library Author · Remote Technical Specialist**

I've been writing software in C and C++ since 1997 and have been working in C# and .NET as the ecosystem has matured. I build practical, reusable libraries — tools I'd actually want to use myself — and release them publicly for others to build on.

---

## Open Source Libraries

| Repository | Description | Language |
|---|---|---|
| [Logger](https://github.com/WilliamW1979/Logger) | Async/sync logging DLL with optional encryption and five severity levels | C# / .NET 10 |
| [SettingsFile](https://github.com/WilliamW1979/SettingsFile) | INI-style config file wrapper with typed Get/TryGet and optional encryption | C# / .NET 10 |
| [PluginManager](https://github.com/WilliamW1979/PluginManager) | Runtime plugin loader with service injection, hot-reload, and lifecycle management | C# / .NET 10 |
| [Card-Deck-Handler](https://github.com/WilliamW1979/Card-Deck-Handler) | High-performance card hand evaluation library using 64-bit integer representation | C# / .NET 10 |

All libraries share a consistent design philosophy: zero external dependencies, pluggable interfaces for extensibility, and clean disposal patterns.

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

## Current Project — Journey of the Forgotten *(private)*

A large-scale MMORPG server written in C# with a distributed multi-process architecture designed for high availability and zero-downtime updates.

**Key architectural decisions:**
- **Multi-process server design** — game systems run as independent executables (world simulation, AI, networking, data) communicating over custom binary TCP, so any single process can be updated or restarted without taking the whole server offline
- **AI-driven world simulation** — NPC behavior, ecosystems, and world events are driven by autonomous AI systems running in dedicated processes, keeping simulation load isolated from player-facing services
- **Independent update cycles** — because each subsystem is a separate process, patches can be deployed surgically; a bug fix in the AI layer never requires a full server restart
- **Distributed load splitting** — compute-heavy workloads are distributed across processes rather than bottlenecked in a monolith, allowing horizontal scaling of individual systems

This project applies the same library infrastructure from the public repos above — Logger, SettingsFile, and PluginManager are all in active use as internal dependencies.

---

## Skills & Tools

**Languages:** C (1997–present), C++ (1997–present), C# / .NET (current)

**Areas:** Distributed systems architecture, multi-process IPC, library design, bit manipulation and performance optimization, asynchronous programming, plugin architecture, encryption integration, AI/simulation systems, card game logic and equity calculation, system administration, Windows & Linux, data operations

**Tools:** Visual Studio, Git, VS Code, Ollama (local AI), Microsoft Office, Google Workspace

---

## About Me

I work independently and remotely. I'm available for contract, part-time, or full-time remote roles in software development, IT support, or technical operations.

📧 WilliamW1979@netscape.net · 📍 Berwick, PA (Eastern Time) · 💼 [LinkedIn](https://www.linkedin.com/in/william-glenn-ward/)

---

> *All libraries are free for personal and commercial use. Credit appreciated. No warranty provided.*
