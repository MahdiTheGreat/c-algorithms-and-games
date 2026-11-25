# Cellular Automata & Interactive Game Engines in C

Low-level ANSI C implementations encompassing cellular automaton simulations and interactive decision-tree strategy games.

---

## Architecture & Source Overview

```
projects/SimpleProjects/
├── FallingKingdom.c       # Interactive kingdom strategy state machine with binary decision branching
├── gameOfLife.c           # Conway's Game of Life cellular automaton with dynamic grid expansion
└── README.md              # Project technical documentation
```

---

## Core Systems & Mathematical Formulations

### 1. Conway's Game of Life with Dynamic Perimeter Expansion (`gameOfLife.c`)
An implementation of John Conway's two-dimensional zero-player cellular automaton in ANSI C.

- **Cellular Transition Dynamics:**
  For a grid state $S_t(i, j) \in \{0, 1\}$, the number of living neighbors in the 8-connected Moore neighborhood is evaluated as:
  $$k(i, j) = \sum_{di=-1}^1 \sum_{dj=-1}^1 S_t(i+di, j+dj) - S_t(i, j)$$
  State transitions follow deterministic rules:
  $$S_{t+1}(i, j) = \begin{cases} 
  1 & \text{if } S_t(i, j) = 1 \land k(i, j) \in \{2, 3\} \\
  1 & \text{if } S_t(i, j) = 0 \land k(i, j) = 3 \\
  0 & \text{otherwise}
  \end{cases}$$
- **Dynamic Bounding Grid Expansion:**
  The engine monitors active perimeter boundaries. When living cell configurations populate outer limits, internal coordinates automatically reallocate and expand dynamically, preventing artificial edge absorption.

### 2. Fallen Kingdom Governance Simulation (`FallingKingdom.c`)
An interactive decision-tree strategy simulation written in C.

- **Macroeconomic State Vector:**
  The state of the kingdom is modeled by three normalized parameters bounded within $[0, 100]$:
  $$\mathbf{S} = \begin{pmatrix} P \\ C \\ T \end{pmatrix} = \begin{pmatrix} \text{Citizenry / Population Well-being} \\ \text{Court / Aristocratic Satisfaction} \\ \text{Treasury / Financial Solvency} \end{pmatrix}$$
- **Decision Branching & Differential Updates:**
  Events are sampled from an external scenario pool, presenting the sovereign with binary choices that apply delta state vectors:
  $$\mathbf{S}_{t+1} = \operatorname{clip}_{[0, 100]}(\mathbf{S}_t + \mathbf{\Delta}_k)$$
  where $\mathbf{\Delta}_k = (\Delta P_k, \Delta C_k, \Delta T_k)^\top$.
- **Persistence & Serialization:**
  Includes binary state serialization (`save.bin`) allowing session persistence across executions. Terminal conditions trigger if any state variable collapses to zero or reaches maximum threshold limits.

---

## Build & Execution Instructions

Compile with `gcc` (or `clang`):

```bash
# Build Conway's Game of Life
gcc -O2 gameOfLife.c -o gameOfLife
./gameOfLife

# Build Fallen Kingdom Simulation
gcc -O2 FallingKingdom.c -o FallingKingdom
./FallingKingdom
```
