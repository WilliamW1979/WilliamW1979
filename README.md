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

All libraries share a consistent design philosophy: zero external dependencies, pluggable interfaces for extensibility (encryption, logging), and clean disposal patterns.

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

**Areas:** Distributed systems architecture, multi-process IPC, library design, asynchronous programming, plugin architecture, encryption integration, AI/simulation systems, system administration, Windows & Linux, data operations

**Tools:** Visual Studio, Git, VS Code, Ollama (local AI), Microsoft Office, Google Workspace

---

## About Me

I work independently and remotely. I'm available for contract, part-time, or full-time remote roles in software development, IT support, or technical operations.

📧 WilliamW1979@netscape.net · 📍 Berwick, PA (Eastern Time) · 💼 [LinkedIn](https://www.linkedin.com/in/william-glenn-ward/)

---

> *All libraries are free for personal and commercial use. Credit appreciated. No warranty provided.*
