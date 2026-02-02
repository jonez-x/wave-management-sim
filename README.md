# Wave Management Simulator

An interactive educational tool for learning League of Legends wave management mechanics, built with the [Effekt](https://effekt-lang.org/) programming language. Observe and manipulate minion waves to understand how wave states like freezes, slow pushes, and crashes work in practice.

Developed as part of the EPE course at the University of Tübingen.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
- [Usage Guide](#usage-guide)
- [Project Structure](#project-structure)
- [Development](#development)
- [CI/CD](#cicd)
- [License](#license)

---

## Features

### Core Simulation

- **Mid-lane simulation** with turrets on both sides
- **Three minion types** with distinct stats:
  - **Melee** -- short range, durable frontline units
  - **Caster** -- long range, high damage, low HP backline units
  - **Cannon** -- tanky siege unit, spawns every 3rd wave
- **Wave spawning** every 30 seconds (3 melee + 3 caster per team, plus cannon on every 3rd wave)
- **Combat AI** with aggro targeting, turret prioritization, and projectile simulation
- **Turret mechanics** with realistic damage (152 AD), attack speed (0.83), and range indicators

### Interactive Tools

| Key | Tool | Description |
|-----|------|-------------|
| `Q` | **Last-Hit** | Kills enemy minions below 50 HP within a 30-unit radius of your click. Simulates last-hitting for gold. |
| `W` | **AoE Damage** | Deals 150 damage in an 80-unit radius. Non-lethal (leaves minions at 1 HP). Used to soften a wave. |
| `E` | **Kill Casters** | Instantly kills all enemy caster minions within 200 units. Sets up a slow push by creating a numbers advantage. |
| `R` | **Full Clear** | Kills all enemy minions within 200 units. Simulates hard-pushing or full-clearing a wave. |
| `T` | **Tank** | Places an aggro point that draws minion attacks. Simulates tanking a wave to hold or freeze it. |

### Wave State Detection

The simulator analyzes the minion wave each frame and displays the current state:

| State | Meaning |
|-------|---------|
| **Neutral** | Even forces meeting in the middle |
| **Pushing Blue/Red** | Wave moving towards one base |
| **Slow Push Blue/Red** | A growing wave building recursively towards one side |
| **Frozen Blue/Red** | Wave held static near a turret |
| **Crashing** | Wave hitting a turret |
| **Bouncing** | Wave rebounding after a turret crash |

### Visualization

- 2D lane view rendered on HTML5 Canvas (1200x400)
- Color-coded minions by team (blue/red) and type
- Health bars with color thresholds (green > 60%, yellow > 30%, red)
- Turret range indicators
- Projectile animations (distinct visuals for turret, caster, and melee)
- Tool preview overlays showing area of effect
- Visual effects: click ripples, AoE explosions, last-hit markers, kill markers, tank activation
- HUD: game time, wave number, minion counts, speed, selected tool, wave state

### Controls

| Key | Action |
|-----|--------|
| `Space` | Pause / Resume |
| `1` | Set speed to 1x |
| `2` | Set speed to 2x |
| `3` | Set speed to 4x |
| `Q/W/E/R/T` | Select tool |
| Mouse click | Use selected tool at cursor position |

---

## Getting Started

### Prerequisites

This project uses the [Effekt](https://effekt-lang.org/) programming language, which compiles to JavaScript. You need one of the following setups:

**Option 1: Nix (recommended)**

Install [Nix](https://nixos.org/download.html) with flakes enabled. All dependencies (including the Effekt compiler) are managed automatically via `flake.nix`.

**Option 2: Manual Effekt installation**

Install the Effekt compiler by following the [official getting-started guide](https://effekt-lang.org/docs/getting-started). Optionally install the Effekt VSCode extension for editor support.

### Installation

```sh
git clone https://github.com/jonez-x/wave-management-sim.git
cd wave-management-sim
```

If using Nix, enter the development shell:

```sh
nix develop
```

### Running the Simulator

**With Effekt directly:**

```sh
effekt src/main.effekt
```

**With Nix:**

```sh
nix run
```

Both compile the project to JavaScript (`out/main.js`) and open it in your browser via `index.html`.

**Building without running:**

```sh
effekt --build src/main.effekt   # output in out/
nix build                        # output in result/bin/
```

Then open `index.html` in any browser.

### Running Tests

```sh
effekt src/test.effekt
```

---

## Usage Guide

### Basic Workflow

1. **Start the simulator** -- the first wave spawns immediately; subsequent waves spawn every 30 seconds.
2. **Observe the default behavior** -- with no intervention, waves meet in the middle and trade evenly (Neutral state).
3. **Select a tool** by pressing `Q`, `W`, `E`, `R`, or `T`. The current tool is shown in the top-right HUD.
4. **Click on the lane** to apply the tool at the cursor position. Tool previews (range circles) appear for AoE, Full Clear, and Tank.
5. **Watch the wave state indicator** (top-right, color-coded) to see how your actions change the wave dynamics.

### Example Scenarios

**Setting up a slow push:**

1. Wait for waves to meet and fight in the middle.
2. Press `E` (Kill Casters) and click on the enemy wave.
3. This removes the enemy backline, giving your wave a numbers advantage.
4. The wave state should change to "Slow Push" -- your wave grows larger each time a new wave joins.

**Freezing a wave:**

1. Let the enemy wave push slightly past the middle towards your turret.
2. Press `T` (Tank) and click in front of your turret to absorb minion aggro.
3. Keep the wave just outside turret range. New waves from both sides will arrive and maintain the freeze.
4. The wave state should show "Frozen" near your side.

**Crashing a wave into turret:**

1. Press `R` (Full Clear) and click on the enemy wave to instantly clear it.
2. Your surviving minions walk uncontested into the enemy turret.
3. The wave state changes to "Crashing."

**Using AoE without killing:**

1. Press `W` (AoE) and click on a wave.
2. Deals 150 damage but never kills (leaves at 1 HP).
3. This softens minions so they can be last-hit with `Q`.

---

## Project Structure

```
wave-management-sim/
├── src/
│   ├── main.effekt               # Entry point, browser bridge, game loop setup
│   ├── types.effekt               # Domain models, enums, records, constants
│   ├── effects.effekt             # Algebraic effect interface definitions
│   ├── handlers.effekt            # Effect handler implementations
│   ├── helpers.effekt             # Utility functions
│   ├── lib.effekt                 # Library code
│   ├── test.effekt                # Test suite
│   │
│   ├── game/                      # Core game logic
│   │   ├── simulation.effekt      # Main game loop orchestration
│   │   ├── wave.effekt            # Wave spawning, scheduling, state analysis
│   │   ├── minion.effekt          # Movement physics, separation forces
│   │   ├── combat.effekt          # Targeting, damage, projectile simulation
│   │   ├── turret.effekt          # Turret creation helpers
│   │   └── state.effekt           # Initial state factory
│   │
│   ├── tools/                     # Interactive tool implementations
│   │   ├── dispatcher.effekt      # Routes clicks to active tool
│   │   ├── lasthit.effekt         # Last-Hit tool (Q)
│   │   ├── aoe.effekt             # AoE Damage tool (W)
│   │   ├── clear.effekt           # Kill Casters (E) and Full Clear (R)
│   │   ├── tank.effekt            # Tank tool (T)
│   │   └── utils.effekt           # Tool utility functions
│   │
│   ├── ffi/                       # Foreign Function Interface (JS bindings)
│   │   ├── canvas.effekt          # HTML5 Canvas 2D context bindings
│   │   └── events.effekt          # Browser event bindings (mouse, keyboard)
│   │
│   └── utils/                     # Shared utilities
│       ├── math.effekt            # Vector math (Vec2), collision detection
│       ├── lists.effekt           # Functional list operations
│       └── game.effekt            # Domain-specific helpers (team swap, speed names)
│
├── index.html                     # Browser entry point (styled with LoL theme)
├── out/                           # Compiled JavaScript output
│   └── main.js
├── flake.nix                      # Nix package configuration
├── flake.lock                     # Nix dependency lock file
├── Architecture.md                # Developer architecture documentation
└── README.md                      # This file
```

---

## Development

### Useful Commands

| Command | Description |
|---------|-------------|
| `effekt src/main.effekt` | Compile and run |
| `effekt --build src/main.effekt` | Build to `out/` |
| `effekt src/test.effekt` | Run tests |
| `effekt` | Open the Effekt REPL |
| `effekt --help` | Show compiler options |
| `nix develop` | Enter dev shell with all dependencies |
| `nix run` | Build and run via Nix |
| `nix build` | Build via Nix (output in `result/bin/`) |
| `nix flake update` | Update Nix dependencies |

For architecture details and developer guidelines, see [Architecture.md](Architecture.md).

---

## CI/CD

Two GitHub Actions workflows are configured:

1. **`flake-check`** -- Checks `flake.nix`, builds, and tests the project. Runs on pushes to `main` and on pull requests.

2. **`update-flake-lock`** -- Updates Nix dependency versions weekly (Tuesdays at 00:00 UTC) and automatically creates PRs with Effekt version updates.

### Enabling Auto-Update CI

1. Go to **Settings > Actions > General**
2. Set **Workflow permissions** to "Read and write permissions"
3. Check **"Allow GitHub Actions to create and approve pull requests"**

---

## License

This project is part of the EPE course at the University of Tübingen.

---

**Built with [Effekt](https://effekt-lang.org/)** -- A language with effect handlers and lightweight effect polymorphism.
