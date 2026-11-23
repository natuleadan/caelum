<p align="center"><img src="https://raw.githubusercontent.com/natuleadan/.github/main/profile/assets/banners/caelum.svg" alt="Caelum Constellation" width="100%" /></p>

<h1 align="center">Caelum</h1>
<p align="center"><strong>Sovereign spatial physics and tactile perception engine in pure Rust</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-Rust_2024-orange?style=flat" alt="Rust 2024" />
  <img src="https://img.shields.io/badge/Toolchain-Cargo_xtask-blue?style=flat" alt="Cargo xtask" />
  <img src="https://img.shields.io/badge/Domain-Tactile_Motor_Robotics-8B5CF6?style=flat" alt="Tactile Motor Robotics" />
  <img src="https://img.shields.io/badge/Validation-8_Gates_Pass-green?style=flat" alt="8 Gates Pass" />
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat" alt="Apache 2.0 License" /></a>
  <a href="https://natuleadan.com"><img src="https://img.shields.io/badge/Organization-Natuleadan-orange.svg?style=flat" alt="Natuleadan" /></a>
</p>

## 1. What is Caelum?

Caelum is a sovereign, pure-Rust physical-world interaction, tactile perception, and **spatial physics and tactile perception engine** built for autonomous agents. Built on deterministic spatial contracts and multi-hardware compatibility layers, Caelum lets agents sense force, contact and spatial state—communicating with sibling constellations strictly over decoupled network protocols (REST, WebSocket/WSS, gRPC) without direct Cargo coupling.

### What is inside

Caelum implements a layered six-file modular domain layout (`entity`, `dto`, `repository`, `service`, `controller`, `mod`) atop a pure-Cargo workspace (`crates/core` and `crates/xtask`), governed by eight native validation gates and a 3-day dependency cooldown.

> **Current scope:** This release ships the native validation harness, the six-file modular reference domain (`modules/math`), and the crate contracts. Transports, sandboxes, domain engines and protocol integrations described below are planned milestones, not shipped capabilities.

## 2. Why Caelum?

Autonomous agents usually operate purely in text, lacking a deterministic spatial model or structured tactile feedback loop to interact safely with physical hardware.

- **Tactile & spatial awareness**: Models contact surfaces, force thresholds, and spatial transforms cleanly in native Rust.
- **100% Pure native Rust**: Zero Node.js, zero Python, and zero opaque binary dependencies.
- **Strict layer isolation**: Controllers, simulation services, and state repositories are decoupled by automated boundary tests.
- **Zero formatter drift**: Formatters are excluded from hooks and CI so diffs remain focused on engineering logic.

## 3. Highlights

- **Deterministic spatial contracts**: Type-safe tactile events, collision envelopes, and physical scene graphs.
- **Pure Cargo automation via `xtask`**: Native `cargo xtask validate`, `notices`, and `release` subcommands.
- **Eight standardized validation gates**: Full verification across lint, typecheck, unit tests, architecture tripwires, cooldown, hygiene, notices, and SemVer checks.
- **Supply-chain hardening**: Enforced 3-day dependency publish cooldown (`resolver.min-publish-age="3 days"`), CodeQL, and SBOM artifacts.
- **Apache-2.0 licensed**: Clear commercial terms with automated `ThirdPartyNotices.txt` verification.

## 4. Quick Start

Ensure Rust `1.85+` (Rust 2024 edition / stable, matching `rust-version = "1.85"`) is installed:

```bash
# Clone the repository
git clone https://github.com/natuleadan/caelum.git
cd caelum

# Activate local hooks
git config core.hooksPath .githooks

# Run the complete 8-gate native validation suite
cargo xtask validate
```

Other native `xtask` operations:

```bash
cargo xtask notices          # Regenerate ThirdPartyNotices.txt from cargo metadata
cargo xtask release --check  # Verify Cargo.toml version and CHANGELOG parity
cargo test --workspace       # Execute unit, integration, and architecture tests
```

## 5. Use Cases

- **Tactile agent feedback**: Translate physical sensor arrays and force-feedback actuators into structured spatial events for Pulsar agents.
- **Physical-world digital twins**: Maintain deterministic collision and spatial state models exposed to Pulsar and authorized consumers.
- **Multi-device edge simulation**: Run lightweight spatial ticks across Linux servers and embedded controllers.

## 6. How It Works

```mermaid
sequenceDiagram
    participant Sensor as Physical Sensor / Hardware
    participant Ctrl as Spatial Controller
    participant Svc as Tactile Physics Service
    participant Repo as World State Repo
    participant Agent as Pulsar

    Sensor->>Ctrl: streamContactReading(dto)
    Ctrl->>Ctrl: validateBounds(dto)
    Ctrl->>Svc: evaluateTactileStep(dto)
    Svc->>Repo: updateSpatialState()
    Repo-->>Svc: worldSnapshot
    Svc-->>Ctrl: tactileEvent
    Ctrl-->>Agent: physicalFeedbackPayload
```

Hardware contact readings enter through the `controller`, validate safety bounds via `dto`, update deterministic spatial state inside `service`, and emit structured tactile feedback to agents.

## 7. Status and Roadmap

- **Status**: Initial foundation — validation harness and modular reference skeleton in place; domain implementation in progress.
- **Milestone 1**: Core ECS-inspired spatial world, tactile event schemas, and deterministic tick scheduler.
- **Milestone 2**: Hardware-agnostic force/contact sensor traits and collision envelope solvers.
- **Milestone 3**: Decoupled wire-protocol (REST, WSS, gRPC) bridges for Pulsar harnesses and authorized consumers.

## 8. Repository Layout & Validation Gates

```text
.
├── .cargo/                  # Cargo configuration and 3-day cooldown resolver
├── .githooks/               # Versioned commit-msg, pre-commit, and pre-push hooks
├── .github/                 # Issue templates, dependabot config, and CI workflows
├── crates/
│   ├── core/                # Core spatial/tactile library (modules/math reference domain, lib.rs)
│   │   └── tests/           # integration.rs and immutable architecture.rs tripwires
│   └── xtask/               # Pure-Rust automation CLI (cargo xtask <cmd>)
├── .env.example             # Environment contract template (copy to git-ignored .env)
├── Cargo.toml               # Workspace manifest (rust-version = "1.85", [workspace.lints])
├── clippy.toml              # Strict Clippy policy (max-fn-params-bools = 0)
├── ARCHITECTURE.md          # Modular domain architecture specifications
├── AGENTS.md                # Autonomous AI agent operating guidelines
├── CONTRIBUTING.md          # Contribution workflow and commit grammar
├── SECURITY.md              # Supply-chain policies and vulnerability reporting
├── LICENSE                  # Apache-2.0 License
├── NOTICE                   # Copyright (c) 2026 Natuleadan.
└── ThirdPartyNotices.txt    # Automated third-party license inventory
```

| Gate | Command | Purpose |
| :--- | :--- | :--- |
| `lint` | `cargo clippy --workspace --all-targets --all-features -- -D warnings` + `cargo doc -D warnings` | Strict zero-warning static analysis and rustdoc checks |
| `typecheck` | `cargo check --workspace --all-targets` | Workspace type verification |
| `unit tests` | `cargo test --workspace --lib` + `--doc` | Library unit tests and runnable doc examples |
| `integration and architecture tests` | `cargo test --workspace --test '*'` | Integration flows and layer tripwires |
| `cooldown` | `cargo --config 'resolver.min-publish-age="3 days"'` | 3-day package release cooldown |
| `hygiene` | `crates/xtask/src/hygiene.rs` | 500-line limit, anti-leak, and syntax checks |
| `notices` | `crates/xtask/src/notices.rs` | License inventory parity check |
| `release` | `crates/xtask/src/release.rs` | SemVer and CHANGELOG consistency |

## 9. Documentation

- [ARCHITECTURE.md](ARCHITECTURE.md): Modular architecture, layer boundaries, and design invariants.
- [CONTRIBUTING.md](CONTRIBUTING.md): Issue-first contribution policy, PR workflow, and commit grammar.
- [SECURITY.md](SECURITY.md): Vulnerability disclosure procedures and supply-chain controls.
- [AGENTS.md](AGENTS.md): Machine-facing instructions for autonomous engineering.

## 10. Ecosystem & Community

Caelum is the spatial physics and tactile perception constellation of Natuleadan—communicating over decoupled wire protocols (REST, WSS, gRPC) with Orion, Andromeda, Aurora, Pleaides, Pulsar, Lyra (acoustic perception), and Skills.

<p align="left">
  <a href="https://github.com/natuleadan"><img src="https://avatars.githubusercontent.com/u/287601518?v=4&s=48" width="48" height="48" alt="natuleadan" title="natuleadan"/></a>
  <a href="https://github.com/leojara95"><img src="https://avatars.githubusercontent.com/u/268038834?v=4&s=48" width="48" height="48" alt="leojara95" title="leojara95"/></a>
</p>

Bug reports and feature proposals are tracked via structured GitHub Issues.

## 11. Contributing

External pull requests are restricted to maintainers (`collaborators_only`). Please open an issue describing your bug or proposal. See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 12. License & Star History

This project is licensed under the Apache License 2.0. Copyright (c) 2026 Natuleadan.

Terms are in [LICENSE](LICENSE), attribution in [NOTICE](NOTICE), and third-party dependency notices in [ThirdPartyNotices.txt](ThirdPartyNotices.txt).

<a href="https://www.star-history.com/#natuleadan/caelum&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/image?repos=natuleadan/caelum&type=date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/image?repos=natuleadan/caelum&type=date&theme=light" />
    <img alt="Star History Chart" src="https://api.star-history.com/image?repos=natuleadan/caelum&type=date" />
  </picture>
</a>
