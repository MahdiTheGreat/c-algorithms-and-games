# Cellular Automata & Interactive Game Engines in C

Low-level ANSI C implementations encompassing cellular automaton simulations with dynamic boundary expansion and interactive decision-tree strategy games with pointer-based linked list architectures.

---

## Architecture & Source Overview

```text
projects/SimpleProjects/
├── FallingKingdom.c       # Sovereign governance state machine with linked list dilemma dispatching
├── gameOfLife.c           # Conway's Game of Life cellular automaton with dynamic perimeter expansion
└── README.md              # Technical specifications & system architecture
```

---

## Core Systems & Implementation Details

### 1. Fallen Kingdom Sovereign Governance Simulation (`FallingKingdom.c`)
An interactive sovereign decision-tree strategy simulation written in ANSI C, modeling the complexities of imperial statecraft (inspired by games like *Reigns*).

- **Macroeconomic State Vector:**
  The health and stability of the realm are modeled by a normalized 3-dimensional state vector bounded strictly within $[0, 100]$:
  $$\mathbf{S} = \begin{pmatrix} P \\ C \\ T \end{pmatrix} = \begin{pmatrix} \text{People / Citizenry Satisfaction} \\ \text{Court / Aristocratic Loyalty} \\ \text{Treasury / Imperial Solvency} \end{pmatrix}$$
  Initial state for a new sovereign: $P = 50, C = 50, T = 50$.

- **Custom Pointer-Based Linked List Engine:**
  - Implements a dynamic singly linked list built from raw C pointers (`struct problem *next`) without third-party or standard library container dependencies.
  - Ingests dilemma definitions from an index registry (`CHOICES.txt`) referencing external scenario files (`c1.txt`, `c2.txt`, etc.).
  - Each dilemma file specifies:
    - Line 1: Scenario narrative statement (`story`, $\le 200$ chars).
    - Line 2: Decision option 1 text (`op1`, $\le 200$ chars).
    - Lines 3–5: Integer deltas $(\Delta P_1, \Delta C_1, \Delta T_1)$ applied by Option 1.
    - Line 6: Decision option 2 text (`op2`, $\le 200$ chars).
    - Lines 7–9: Integer deltas $(\Delta P_2, \Delta C_2, \Delta T_2)$ applied by Option 2.
- **Dynamic Dilemma Quotas & List Pruning:**
  - Each scenario node initializes with an occurrence quota (`chance = 3`).
  - When selected, a scenario's `chance` decrements by 1; upon reaching 0, the node is dynamically unlinked and removed from the active linked list.
  - If all dilemmas are exhausted while the sovereign remains in power, the linked list dynamically regenerates from disk, resetting all scenario quotas to 3.
- **Terminal Defeat Conditions:**
  The reign collapses immediately if either of the following conditions is met:
  1. Any individual parameter reaches zero: $P \le 0 \lor C \le 0 \lor T \le 0$.
  2. The overall realm health deteriorates below the critical average: $\frac{P + C + T}{3} < 10$.
- **Binary State Serialization & Session Persistence:**
  - Supports mid-game saves (invoked by entering `-1`) and post-defeat saves.
  - Serializes sovereign profile data to binary files (`save.bin`) using a dedicated C `struct`:
    - Player name.
    - Termination status flag (saved mid-game vs. saved on defeat).
    - Array of remaining dilemma chance allocations.
    - Current metric values for People, Court, and Treasury.
  - **Resume Logic:** Restores active mid-game states with remaining dilemma pools intact; resets fallen sovereigns to the baseline $50/50/50$ configuration.
- **Leaderboard Telemetry:** Tracks and ranks the top 10 sovereigns based on aggregate state metrics ($\sum(P, C, T)$) and tie-breaking hierarchical criteria ($P \to C \to T$).

---

### 2. Conway's Game of Life with Dynamic Perimeter Expansion (`gameOfLife.c`)
An ANSI C implementation of John Horton Conway's two-dimensional zero-player cellular automaton, enhanced with dynamic coordinate boundary expansion.

- **Cellular Transition Dynamics:**
  For any cell $(i, j)$ in a discrete grid $S_t(i, j) \in \{0, 1\}$, the number of living neighbors within its 8-connected Moore neighborhood is:
  $$k(i, j) = \sum_{di=-1}^1 \sum_{dj=-1}^1 S_t(i+di, j+dj) - S_t(i, j)$$
  State transitions execute synchronously across the entire grid:
  $$S_{t+1}(i, j) = \begin{cases} 
  1 & \text{if } S_t(i, j) = 1 \land k(i, j) \in \{2, 3\} & \text{(Survival)} \\
  1 & \text{if } S_t(i, j) = 0 \land k(i, j) = 3 & \text{(Reproduction)} \\
  0 & \text{if } S_t(i, j) = 1 \land (k(i, j) < 2 \lor k(i, j) > 3) & \text{(Under/Overpopulation)} \\
  0 & \text{otherwise} & \text{(Stasis)}
  \end{cases}$$
- **Dynamic Perimeter Expansion:**
  - Unlike conventional static grids that clip or wrap at boundaries, this engine continuously inspects perimeter cells during each generation.
  - If a living cell appears on any outer boundary, the grid automatically reallocates and expands by 1 unit along the affected dimension (e.g., expanding both rightward and upward for a top-right corner boundary cell) before the next cycle computes.
  - **Memory Guard:** Enforces an expansion ceiling (up to $2\times$ the initial dimension, $2N+1$) to prevent memory exhaustion, notifying the user when the spatial threshold is reached.
- **Interactive Coordinate Setup & Animation:**
  - Prompts for initial grid size $N$ and execution generations $t$.
  - Reads seed cell coordinates $(x, y)$ interactively until terminated by an out-of-bounds coordinate ($> N$).
  - Renders the grid via terminal characters (`|*|` for living cells, `| |` for empty cells), incorporating 1-second generation delays and screen clearing for smooth terminal animations.

---

## Build & Execution Instructions

Compile with standard C compilers (`gcc`, `clang`):

```bash
# Build Conway's Game of Life
gcc -O2 gameOfLife.c -o gameOfLife
./gameOfLife

# Build Fallen Kingdom Sovereign Simulation
gcc -O2 FallingKingdom.c -o FallingKingdom
./FallingKingdom
```
