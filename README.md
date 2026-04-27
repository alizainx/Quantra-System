# Quantra-System

> An ultra-fast, memory-safe, 100% Rust init system (PID 1), initramfs, and network stack. Features Kernel Lockdown, Landlock LSM, native healthchecks, and static execution.

**Author:** Ali Zain <alizain.arch@gmail.com>  
**Version:** 5.0.1  
**Language:** Rust (100% Memory Safe)  
**Target:** x86_64-unknown-linux-musl (Static Execution)  
**License:** GPLv3  

---

## Overview

Quantra-System is a comprehensive, zero-bloat core infrastructure stack for modern Linux environments. It unifies the critical early-boot and networking components into a single, tightly integrated monorepo. 

Engineered for stealth, extreme performance, and embedded/robotics systems, Quantra eliminates the need for legacy, fragmented tools (like systemd, busybox-based initramfs, or NetworkManager). It operates entirely without D-Bus, avoids heavy asynchronous runtimes, and compiles into 100% static musl binaries.

---

## Monorepo Architecture

This workspace is divided into highly optimized, purpose-built crates:

| Crate | Component | Description |
| :--- | :--- | :--- |
| **`quantra`** | PID 1 Daemon | The core system and service manager. Enforces security profiles, cgroup v2 limits, and parallel BFS wave dependency resolution. |
| **`quantra-ctl`** | Service CLI | The control plane for service lifecycle management, state inspection, and JSON-structured metrics. |
| **`quantra-ramfs`** | Initramfs | Stage-1 boot orchestrator. Discovers the root partition and target init binary dynamically without external shell scripts. |
| **`zai-netd`** | Network Daemon | Privileged daemon that autonomously executes `rtnetlink` operations securely via Unix `peer_cred` token-less auth. |
| **`zai-net`** | Network CLI | The user-facing network configuration tool for managing Interfaces, Routes, WiFi, VPNs, and Firewalls. |
| **`common`** | Shared Library | Shared IPC data models and length-prefixed JSON protocol logic. |

---

## Core Infrastructure Highlights

- **Absolute Memory Safety:** Eradicates entire classes of vulnerabilities (buffer overflows, use-after-free) by utilizing 100% safe Rust in the execution path.
- **Advanced Kernel Security:** Enforces `integrity` Kernel Lockdown mode directly at boot. Utilizes Landlock LSM sandboxing and `mlockall` to prevent core components from paging to disk.
- **Zero-Bloat Threading:** Entirely bypasses heavy asynchronous runtimes in favor of the standard library for the core PID 1 loop, keeping binary sizes minimal.
- **Sub-100ms Boot Pipeline:** Implements a strictly ordered 12-phase boot sequence, guaranteeing deterministic state and parallel service initialization.
- **Native Health Monitoring:** Eliminates the need for external monitoring daemons by integrating Docker-style healthchecks and crash-loop breakers directly into the PID 1 service runner.

---

## Build Instructions

The entire engine is designed to be built as a standalone, statically linked toolchain requiring no external host libraries.

**Prerequisites:**
- Rust `1.82+`
- `musl-tools` (for static compilation)

**Compilation:**
The workspace is configured to target `x86_64-unknown-linux-musl` by default via `.cargo/config.toml`.

```bash
# Compile all workspace crates into optimized, static binaries
cargo build --release --workspace

# Run all test suites and security contract guards
cargo test --all

Compiled static binaries will be available in the target/x86_64-unknown-linux-musl/release/ directory.
Branching & Contribution Strategy

This repository follows a strict Git Flow to maintain absolute stability for production environments:

    contributors branch: All new features, experimental code, and Pull Requests must target this branch.

    main branch: Represents the stable, production-ready "Master Gold" version. Code is merged here only after passing comprehensive security, static analysis, and unit test suites.

    Please ensure cargo fmt --all and cargo test --all pass locally before submitting any code.

License

GNU General Public License v3.0 (GPLv3)

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

Any modifications or larger works utilizing this core engine must also be released under the same license, ensuring the code remains open and beneficial to the entire Linux community. See the LICENSE file for full details.
