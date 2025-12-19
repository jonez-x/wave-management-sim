# Wave Management Simulator

An interactive educational tool for learning League of Legends wave management. Users can observe and manipulate minion waves using different tools to understand how waves behave and how to achieve different wave states (freeze, slow push, crash, etc.).

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running the Simulator](#running-the-simulator)
- [Development](#development)
  - [Project Structure](#project-structure)
  - [Useful Commands](#useful-commands)
- [CI/CD](#cicd)
- [License](#license)

---

## Features

### Core Simulation
- **Mid lane simulation** with both turrets
- **Three minion types**: Melee, Caster, and Cannon minions with realistic stats (HP, damage, attack speed)
- **Realistic spawn mechanics**: Minions spawn every 30 seconds, Cannon minions every 3rd wave
- **Combat AI**: Minions walk towards each other and engage in combat with basic aggro system
- **Turret mechanics**: Turret damage and aggro system

### Interactive Tools
- **Last-Hit Tool**: Kill minions only when below a certain threshold (simulates last-hitting)
- **AoE Tool**: Deal damage in an area that can or cannot kill minions
- **Kill Casters**: Remove all caster minions from one wave (slow-push setup)
- **Full Clear**: Kill all enemy minions in a range (hard push)
- **Tank Tool**: Absorb minion attacks when they aggro

### Visualization
- 2D GUI showing lane view with minions from both teams
- HP bars for all units
- Turrets at both ends with realistic stats
- Visual distinction between minion types and teams
- Tool interface

### Controls
- Play/Pause/Reset simulation
- Speed control (1x, 2x, 4x)
- Tool selection panel
- Time display and wave counter
- Next spawn timer

## Getting Started

### Prerequisites

This project uses the [Effekt](https://effekt-lang.org/) programming language with effects and handlers.

You have two options for setting up your development environment:

**Option 1: Using Nix (Recommended)**
- Install [Nix](https://nixos.org/download.html) with flakes enabled
- All dependencies will be managed automatically

**Option 2: Manual Installation**
- Install [Effekt](https://effekt-lang.org/docs/getting-started) manually
- Install the Effekt VSCode extension

### Installation

1. **Clone the repository:**
   ```sh
   git clone https://github.com/jonez-x/wave-management-sim.git
   cd wave-management-sim
   ```

2. **Set up your development environment:**
   
   **With Nix:**
   ```sh
   nix develop
   ```
   
   **With VSCode:**
   - Open the project in VSCode
   - Install the Effekt VSCode extension when prompted

### Running the Simulator

**Using Effekt directly:**
```sh
effekt src/main.effekt
```

**Using Nix:**
```sh
nix run
```

The simulator will compile to JavaScript and run in your browser.

## Development

### Project Structure

```
wave-management-sim/
├── .github/
│   └── workflows/        # CI/CD workflows
├── src/
│   ├── main.effekt      # Main entry point
│   ├── test.effekt      # Test suite
│   └── lib.effekt       # Library code
├── flake.nix            # Nix package configuration
├── flake.lock           # Nix dependency lockfile
├── PROJECT_SPEC.md      # Detailed project specification
└── README.md            # This file
```

### Useful Commands

#### Effekt Commands

Run the main file:
```sh
effekt src/main.effekt
```

Run the tests:
```sh
effekt src/test.effekt
```

Build the project:
```sh
effekt --build src/main.effekt
```
This builds the project into the `out/` directory, creating a runnable file `out/main`.

Open the REPL:
```sh
effekt
```

To see all available options and backends:
```sh
effekt --help
```

#### Nix Commands

Update dependencies (also runs automatically in CI):
```sh
nix flake update
```

Open a development shell with all dependencies:
```sh
nix develop
```

Run the main entry point:
```sh
nix run
```

Build the project (output in `result/bin/`):
```sh
nix build
```

## CI/CD

Two GitHub Actions are configured:

1. **`flake-check`**:
   - Checks the `flake.nix` file, builds and tests the project
   - Runs on demand, on `main`, and on PRs
   - Ensures code quality and correctness

2. **`update-flake-lock`**:
   - Updates package versions in `flake.nix`
   - Runs weekly (Tuesdays at 00:00 UTC)
   - Automatically creates PRs with Effekt version updates

### Setting up Auto-Update CI

To enable weekly automatic Effekt updates:

1. Go to **Settings → Actions → General**
2. Set **Workflow permissions** to "Read and write permissions"
3. Check **"Allow GitHub Actions to create and approve pull requests"**

## License

This project is part of the EPE course at the University of Tübingen.

---

**Built with [Effekt](https://effekt-lang.org/)** - A language with effect handlers and lightweight effect polymorphism.
