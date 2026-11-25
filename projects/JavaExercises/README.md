# Java Algorithmic Suite & Domain Implementations

A modular Java collection focusing on classic algorithmic paradigms, symbolic algebraic calculus, matrix linear algebra, discrete string algorithms, and object-oriented transaction state machines.

---

## Architecture & Module Overview

```
projects/JavaExercises/
├── Ecommerce.java                # Multi-tier e-commerce and warehouse inventory lifecycle
├── FoodVendingMachine.java       # Multi-unit automated vending machine transaction engine
├── integral.java                 # Indefinite symbolic polynomial integration and canonicalizer
├── IterativeMatrixCalculator.java# Linear algebra engine with iterative compound operations
├── minesweeper.java              # 2D discrete grid neighborhood mine proximity evaluator
├── ParenthesesBalancer.java      # Stack-based bracket validation and maximal prefix extractor
└── substring.java                # Multi-string common and inverted substring matcher
```

---

## Algorithmic Modules

### 1. Symbolic Polynomial Integration (`integral.java`)
Implements an algebraic tokenization and differentiation/integration engine.
- **Formulation:** Evaluates indefinite integrals of general polynomial expressions:
  $$\int P(x) \, dx = \int \left( \sum_{i=1}^n a_i x^{k_i} \right) dx = \sum_{i=1}^n \left( \frac{a_i}{k_i + 1} \right) x^{k_i + 1}$$
- **Canonical Normalization:**
  - Aggregates duplicate monomial terms of identical degree.
  - Sorts resulting polynomials in strictly descending order of degree.
  - Formats fractional coefficients into normalized decimal notation rounded to two decimal places while eliminating redundant zeros and degenerate signs ($+1x \to x$, $0x^k \to 0$).

### 2. Iterative Matrix Calculator (`IterativeMatrixCalculator.java`)
Object-oriented matrix computation module supporting iterative matrix algebra.
- **Core Operations:**
  - Matrix addition: $C = A + B$, defined when $\dim(A) = \dim(B)$.
  - Matrix subtraction: $C = A - B$, defined when $\dim(A) = \dim(B)$.
  - Matrix multiplication: $C = A \times B$, defined when $\operatorname{cols}(A) = \operatorname{rows}(B)$.
- **Iterative Compounding:**
  Given an iteration parameter $n$, the system evaluates the recurrence:
  $$R_k = f(R_{k-1}, M_2) \quad \text{for } k = 1, \dots, n \quad (R_0 = M_1)$$
  Strict dimension invariant checking halts computation with formal error propagation if any intermediate multiplication violates inner-dimension compatibility.

### 3. Stack-Based Parentheses Balancer (`ParenthesesBalancer.java`)
- **Deterministic Syntax Parsing:** Uses a linear-time stack evaluation algorithm to identify the longest valid contiguous balanced bracket prefix.
- Validates formal grammar conditions:
  - Every closed bracket matches a preceding unclosed open bracket.
  - Bracket nesting preserves strict first-in, last-out (FILO) ordering.
- Time complexity: $\mathcal{O}(N)$, Space complexity: $\mathcal{O}(N)$.

### 4. Canonical Substring & Inversion Search (`substring.java`)
- Given $n$ candidate strings, computes the maximal common substring that appears either in forward orientation or inverted (reversed) orientation across all sequences.
- Employs dynamic sliding window verification to isolate the longest valid sub-sequence.

### 5. Discrete Grid Proximity Scanner (`minesweeper.java`)
- Evaluates an $M \times N$ discrete 2D grid containing mine markers and open terrain.
- Computes the 8-connected Moore neighborhood kernel:
  $$N(i, j) = \sum_{di=-1}^1 \sum_{dj=-1}^1 \mathbb{I}_{[(i+di, j+dj) \in \text{Mines}]} \quad \text{for } (di, dj) \neq (0, 0)$$
  with boundary clipping to generate standard numerical proximity representations.

---

## Domain & State Machine Systems

### 6. E-Commerce & Warehouse Inventory Engine (`Ecommerce.java`)
A complete command-driven transactional domain model:
- **Hierarchical Dispatcher:** Terminal-driven event-loop processing structured administrative and customer actions.
- **Entities:**
  - Customers: Unique identifiers, account balances, and order histories.
  - Warehouses: Physical storage units governed by finite volumetric capacity constraints.
  - Catalog & Pricing: SKU identifiers, base prices, stock reserves, and percentage discount coupons.
  - Orders: Transactional cart lifecycles, reservation validation against available warehouse stock, and automated balance debiting.

### 7. Multi-Unit Automated Vending Network (`FoodVendingMachine.java`)
- Models a distributed network of automated retail units.
- Tracks per-unit inventory arrays, SKU-to-item mapping, dynamic restock ingestion, and purchasing transactions.
- Aggregates unit-level financial telemetry, reporting total transaction counts and gross sales revenue.

---

## Compilation & Execution

Compile any module with standard Java development tooling:

```bash
# Compile and run Symbolic Integrator
javac integral.java
java com.company.Main

# Compile and run Parentheses Balancer
javac ParenthesesBalancer.java
java ParenthesesBalancer

# Compile and run Iterative Matrix Calculator
javac IterativeMatrixCalculator.java
java IterativeMatrixCalculator

# Compile and run E-Commerce System
javac Ecommerce.java
java Ecommerce
```
