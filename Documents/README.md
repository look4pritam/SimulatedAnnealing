# Simulated Annealing

Simulated Annealing (SA) is a probabilistic optimization algorithm used to find good or near-global optimal solutions for complex optimization problems. It is inspired by the physical annealing process in metallurgy, where a material is heated and then slowly cooled so that its atoms settle into a stable, low-energy structure.

In optimization:

- **Energy** → Objective or cost function
- **Temperature** → Control parameter
- **Cooling** → Gradual reduction of search randomness

---

## 1. Definition

Simulated Annealing is a **probabilistic optimization technique** that searches for a high-quality solution by exploring the solution space and occasionally accepting worse solutions.

For a minimization problem:

\[
x^* = \arg\min_x f(x)
\]

where:

- \(x\) = candidate solution
- \(f(x)\) = objective/cost function
- \(x^*\) = best solution

The ability to temporarily accept worse solutions helps the algorithm escape local minima.

---

## 2. Why Simulated Annealing is Needed

Many optimization problems contain:

- Multiple local minima
- Very large search spaces
- Discrete variables
- Non-differentiable objective functions
- Complex constraints

A conventional greedy optimization method may become trapped in a local optimum.

Simulated Annealing addresses this by allowing the algorithm to **temporarily accept worse solutions**.

---

## 3. Local Minimum vs Global Minimum

A solution landscape can contain several valleys:

```text
Objective
   │
   │       /\                /\
   │      /  \      /\      /  \
   │_____/    \____/  \____/    \____
   │          ↑             ↑
   │      Local minimum   Global minimum
   │
   └──────────────────────────────────→ Solution
```

A greedy algorithm may settle at the first local minimum.

Simulated Annealing can move temporarily toward a worse objective value and potentially reach a better valley.

---

## 4. Basic Principle

The algorithm starts with a relatively high temperature.

At high temperature:

- Exploration is strong.
- Many different solutions are considered.
- Worse solutions can be accepted with relatively high probability.

As the temperature decreases:

- Exploration decreases.
- The algorithm becomes more selective.
- Worse solutions become increasingly unlikely to be accepted.

Conceptually:

```text
High Temperature
       ↓
Strong exploration
       ↓
Accept good + some bad moves
       ↓
Temperature decreases
       ↓
Reduced exploration
       ↓
Mostly accept better moves
       ↓
Low Temperature
       ↓
Converge toward a good solution
```

---

## 5. Acceptance Probability

The most important equation in Simulated Annealing is the acceptance probability.

For a minimization problem:

\[
\Delta E = E_{new} - E_{current}
\]

If the new solution is better:

\[
\Delta E \leq 0
\]

the new solution is accepted.

If the new solution is worse:

\[
\Delta E > 0
\]

it can still be accepted with probability:

\[
P = e^{-\Delta E/T}
\]

where:

- \(P\) = probability of accepting the worse solution
- \(\Delta E\) = increase in objective/energy
- \(T\) = current temperature
- \(e\) = Euler's number

---

## 6. Example of Acceptance Probability

Suppose:

\[
\Delta E = 10
\]

### High temperature

If:

\[
T = 100
\]

then:

\[
P=e^{-10/100}\approx0.905
\]

There is approximately a **90.5% probability** of accepting the worse solution.

### Low temperature

If:

\[
T=1
\]

then:

\[
P=e^{-10}\approx0.000045
\]

The probability is almost zero.

Therefore:

> **High temperature → exploration**

> **Low temperature → exploitation**

---

## 7. Algorithm Steps

A typical Simulated Annealing algorithm follows these steps:

1. Generate an initial solution.
2. Calculate its objective value.
3. Set an initial temperature \(T\).
4. Generate a neighboring solution.
5. Calculate the new objective value.
6. If the new solution is better, accept it.
7. If the new solution is worse, accept it probabilistically.
8. Reduce the temperature.
9. Repeat the process.
10. Stop when the temperature reaches the minimum temperature or another stopping criterion is satisfied.
11. Return the best solution found.

---

## 8. Pseudocode

```text
Current = Initial Solution
Best = Current
T = Initial Temperature

while T > Tmin:

    New = GenerateNeighbor(Current)

    ΔE = Cost(New) - Cost(Current)

    if ΔE < 0:
        Current = New

    else:
        Generate random number r ∈ [0,1]

        if r < exp(-ΔE/T):
            Current = New

    if Cost(Current) < Cost(Best):
        Best = Current

    T = CoolingSchedule(T)

return Best
```

---

## 9. Cooling Schedule

The cooling schedule determines how quickly the temperature decreases.

A commonly used schedule is **geometric cooling**:

\[
T_{k+1}=\alpha T_k
\]

where:

\[
0 < \alpha < 1
\]

For example:

\[
T_0=1000,\qquad \alpha=0.95
\]

The temperatures become approximately:

```text
1000
 ↓
950
 ↓
902.5
 ↓
857.4
 ↓
814.5
 ↓
...
```

### Common Cooling Schedules

- Geometric cooling
- Linear cooling
- Logarithmic cooling
- Adaptive cooling

---

## 10. Important Parameters

| Parameter | Purpose |
|---|---|
| Initial temperature | Controls initial exploration |
| Final temperature | Determines when to stop |
| Cooling rate | Controls cooling speed |
| Number of iterations | Controls search effort |
| Neighbor function | Generates candidate solutions |
| Acceptance function | Determines whether worse solutions are accepted |
| Stopping criterion | Determines termination |

---

## 11. Neighbor Generation

The neighborhood function determines how a new candidate solution is generated.

### Travelling Salesman Problem

Suppose the current route is:

```text
A → B → C → D → E
```

Swap two cities:

```text
A → D → C → B → E
```

The new route is a neighboring solution.

### Continuous Optimization

For a continuous variable:

\[
x_{new}=x_{current}+\epsilon
\]

where \(\epsilon\) is a small random perturbation.

The choice of neighborhood function can have a major impact on performance.

---

## 12. Exploration vs Exploitation

Simulated Annealing provides a natural balance between exploration and exploitation.

```text
Beginning
   │
   │ High T
   ↓
Strong exploration
Many solutions considered
Worse solutions frequently accepted
   │
   ↓
Cooling
   │
   ↓
Reduced exploration
   │
   ↓
Low T
   │
   ↓
Strong exploitation
Mostly better solutions accepted
   │
   ↓
Final solution
```

---

## 13. Advantages

Simulated Annealing has several important advantages:

- Can escape local minima
- Does not require derivatives
- Can handle discrete and continuous variables
- Can work with complicated objective functions
- Relatively simple to implement
- Can handle very large search spaces
- Flexible neighborhood definitions
- Useful when exact optimization is computationally expensive

---

## 14. Disadvantages

Simulated Annealing also has limitations:

- Can be computationally expensive
- Performance depends on parameter selection
- Cooling too quickly can produce poor solutions
- Cooling too slowly can require a very long runtime
- No practical finite-time guarantee of finding the global optimum
- Performance depends heavily on the neighborhood function
- Different runs can produce different solutions

---

## 15. Simulated Annealing vs Hill Climbing

| Feature | Hill Climbing | Simulated Annealing |
|---|---|---|
| Accepts better solution | Yes | Yes |
| Accepts worse solution | No | Sometimes |
| Escapes local minima | Poorly | Better |
| Randomness | Low | High |
| Temperature | No | Yes |
| Global search capability | Limited | Better |

### Key Difference

> **Hill Climbing always moves toward a better solution, whereas Simulated Annealing can temporarily move toward a worse solution to escape a local optimum.**

---

## 16. Simulated Annealing vs Genetic Algorithm

| Feature | Simulated Annealing | Genetic Algorithm |
|---|---|---|
| Search type | Single-solution trajectory | Population-based |
| Randomness | Yes | Yes |
| Uses temperature | Yes | No |
| Uses crossover | No | Yes |
| Uses mutation | Usually neighbor moves | Yes |
| Local search | Strong | Moderate |
| Population | No | Yes |
| Parallel population search | No | Yes |

---

## 17. Applications

Simulated Annealing can be applied to many optimization problems.

### Engineering

- Structural optimization
- Circuit design
- Parameter optimization
- Antenna design

### Computer Science

- Scheduling
- Graph optimization
- Network design
- Clustering
- Routing

### Operations Research

- Travelling Salesman Problem
- Vehicle routing
- Resource allocation
- Job-shop scheduling

### Machine Learning

- Hyperparameter optimization
- Feature selection
- Neural-network parameter optimization
- Clustering

### Scientific Computing

- Molecular optimization
- Protein-related optimization
- Material design
- Scientific parameter fitting

---

## 18. Example: Travelling Salesman Problem

Suppose there are five cities.

An initial route might be:

```text
A → B → C → D → E → A
Distance = 500 km
```

A neighboring route could be:

```text
A → C → B → D → E → A
Distance = 450 km
```

Since:

\[
450 < 500
\]

the new route is accepted.

Now suppose another route has:

```text
A → C → D → B → E → A
Distance = 470 km
```

This is worse than the current 450 km solution.

However, at high temperature, Simulated Annealing may still accept it.

That temporary move to a worse solution could eventually lead to:

```text
A → D → B → C → E → A
Distance = 430 km
```

This demonstrates how Simulated Annealing can escape local optima.

---

## 19. Mathematical Formulation

For a minimization problem:

\[
\Delta E=f(x_{new})-f(x_{current})
\]

### Better solution

If:

\[
\Delta E\leq0
\]

then:

\[
P=1
\]

The new solution is always accepted.

### Worse solution

If:

\[
\Delta E>0
\]

then:

\[
P=e^{-\Delta E/T}
\]

The new solution is accepted probabilistically.

---

## 20. Convergence

Under appropriate theoretical conditions, particularly sufficiently slow cooling, Simulated Annealing can converge toward a global optimum.

However, the cooling schedule required for strong theoretical guarantees can be extremely slow.

Practical implementations therefore generally use faster cooling schedules and aim for a **high-quality approximate solution** within a reasonable amount of computation.

---

## 21. Important Terminology

| Term | Meaning |
|---|---|
| **State** | Current candidate solution |
| **Energy** | Objective/cost function |
| **Temperature** | Controls probability of accepting worse solutions |
| **Neighbor** | Candidate solution close to the current solution |
| **Cooling schedule** | Rule for reducing temperature |
| **Acceptance probability** | Probability of accepting a worse solution |
| **Local optimum** | Best solution within a local region |
| **Global optimum** | Best solution in the entire search space |
| **Iteration** | One optimization step |
| **Annealing** | Gradual reduction of temperature |

---

## 22. Overall Flow

```text
             INITIAL SOLUTION
                    │
                    ▼
             HIGH TEMPERATURE
                    │
                    ▼
          Generate Neighbor
                    │
                    ▼
             Calculate ΔE
             /          \
        Better          Worse
          │               │
          ▼               ▼
       Accept       Accept with
                    probability
                    e^(-ΔE/T)
             \          /
              ▼        ▼
              Update Solution
                    │
                    ▼
             Reduce Temperature
                    │
                    ▼
              T > Tmin?
              /       \
            YES        NO
             │          │
             └──────────┘
                        ▼
                 BEST SOLUTION
```

---

## 23. Key Characteristics

The core characteristics of Simulated Annealing are:

1. **Probabilistic search**
2. **Single-solution based optimization**
3. **Random neighborhood exploration**
4. **Temporary acceptance of worse solutions**
5. **Temperature-controlled exploration**
6. **Gradual cooling**
7. **Ability to escape local minima**
8. **Applicable to discrete and continuous optimization**

---

## 24. Simulated Annealing in One Sentence

> **Simulated Annealing is a randomized optimization algorithm that occasionally accepts worse solutions at high temperature, allowing it to escape local minima, and gradually becomes more selective as the temperature decreases.**

---

## 25. Relation to Quantum Annealing

Simulated Annealing is a **classical optimization algorithm**.

A closely related concept is **Quantum Annealing**, which uses quantum-mechanical effects to search for low-energy solutions of certain optimization problems.

A useful progression for studying optimization is:

```text
Optimization Problem
        │
        ├── Classical Optimization
        │      ├── Hill Climbing
        │      ├── Simulated Annealing
        │      ├── Genetic Algorithms
        │      └── Particle Swarm Optimization
        │
        └── Quantum Optimization
               ├── Quantum Annealing
               ├── QUBO
               └── Quantum Approximate Optimization Algorithm (QAOA)
```

Understanding **Simulated Annealing → QUBO → Quantum Annealing** provides a useful foundation for studying quantum optimization.
