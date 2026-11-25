# Cellular Automata, Interactive Game Engines & Algorithms

Low-level systems programming in C and modular object-oriented implementations in Java, encompassing cellular automaton simulation, interactive state-machine strategy games, linear algebra matrix solvers, and retail domain models.

---

## Directory Architecture

```
projects/SimpleProjects/
├── FallingKingdom.c       # Interactive kingdom strategy state machine with binary decision branching
├── gameOfLife.c           # Conway's Game of Life cellular automaton with dynamic grid expansion
├── InternetShop.java      # Object-oriented retail inventory and sales accounting engine
├── matrixCalculator.java  # Core matrix linear algebra evaluator with dimensional error handling
└── mineGame.java          # Proximity matrix generator for discrete grid mine detection
```

---

## System Overviews

### 1. Conway's Game of Life with Dynamic Perimeter Expansion (`gameOfLife.c`)
An implementation of John Conway's two-dimensional cellular automaton written in ANSI C.

- **Cellular Transition Dynamics:**
  For a grid state $S_t(i, j) \in \{0, 1\}$, the number of living neighbors in the 8-connected Moore neighborhood is evaluated as:
  $$k(i, j) = \sum_{di=-1}^1 \sum_{dj=-1}^1 S_t(i+di, j+dj) - S_t(i, j)$$
  State transitions follow standard deterministic rules:
  $$S_{t+1}(i, j) = \begin{cases} 
  1 & \text{if } S_t(i, j) = 1 \land k(i, j) \in \{2, 3\} \\
  1 & \text{if } S_t(i, j) = 0 \land k(i, j) = 3 \\
  0 & \text{otherwise}
  \end{cases}$$
- **Dynamic Bounding Grid Expansion:**
  Unlike static toroidal or hard-boundary implementations, the engine monitors active perimeter boundaries. When living cells populate outer perimeter limits, the internal coordinate matrix automatically resizes and expands dynamically, preventing artificial edge absorption.

### 2. Fallen Kingdom Governance Simulation (`FallingKingdom.c`)
An interactive decision-tree strategy simulation written in C.

- **Macroeconomic State Vector:**
  The state of the kingdom is modeled by three normalized parameters bounded within $[0, 100]$:
  $$\mathbf{S} = \begin{pmatrix} P \\ C \\ T \end{pmatrix} = \begin{pmatrix} \text{Citizenry / Population Well-being} \\ \text{Court / Aristocratic Satisfaction} \\ \text{Treasury / Financial Solvency} \end{pmatrix}$$
- **Decision Branching & Differential Updates:**
  Events are randomly sampled from an external scenario pool. Each scenario presents the sovereign with binary choices that apply delta state vectors:
  $$\mathbf{S}_{t+1} = \operatorname{clip}_{[0, 100]}(\mathbf{S}_t + \mathbf{\Delta}_k)$$
  where $\mathbf{\Delta}_k = (\Delta P_k, \Delta C_k, \Delta T_k)^\top$.
- **Persistence & Serialization:**
  Includes binary state serialization (`save.bin`) allowing session persistence across executions. Terminal conditions trigger if any state variable collapses to zero or reaches maximum threshold limits.

### 3. Matrix Linear Algebra Evaluator (`matrixCalculator.java`)
- Encapsulates discrete linear algebra operations within a dedicated `Matrix` data structure.
- Implements arithmetic addition, element-wise subtraction, and classical triple-loop matrix multiplication ($\mathcal{O}(R_1 \cdot C_2 \cdot C_1)$).
- Performs explicit inner-dimension compatibility assertions, rejecting invalid combinations with explicit error handling.

### 4. Grid Proximity Mine Sweeper (`mineGame.java`)
- Reads an $M \times N$ discrete character grid representing unmined spaces (`'0'`) and mines (`'*'`).
- Concurrently scans all spatial coordinates, replacing empty cells with character digits corresponding to the local mine density count.

### 5. Retail Transaction & Order Management (`InternetShop.java`)
- Object-oriented retail inventory model tracking customer profiles, product SKUs, warehouse repositories, discount matrices, and aggregate revenue.
- Maintains fast $\mathcal{O}(1)$ lookup across entities via hash mappings while maintaining order history logs.

---

## Build & Execution Instructions

### C Applications
Compile with `gcc` (or `clang`):

```bash
# Build Conway's Game of Life
gcc -O2 gameOfLife.c -o gameOfLife
./gameOfLife

# Build Fallen Kingdom Simulation
gcc -O2 FallingKingdom.c -o FallingKingdom
./FallingKingdom
```

### Java Applications
Compile and run with OpenJDK:

```bash
# Build and run Matrix Calculator
javac matrixCalculator.java
java Matrix

# Build and run Retail Shop
javac InternetShop.java
java Shop
```
