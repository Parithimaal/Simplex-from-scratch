# Simplex Algorithm Implementation in Python

## Introduction

The Simplex Algorithm is an optimization method used to solve linear programming (LP) problems. It works iteratively by moving along the edges of the feasible region to find the optimal solution.

This implementation follows the two-phase Simplex method:
1. **Phase I:** Finds an initial feasible solution by introducing artificial variables.
2. **Phase II:** Optimizes the original objective function.

---

## Class: `SimplexSolver`

### `__init__(self, A, b, c)`
Initializes the Simplex solver with the LP problem parameters.

- `A`: Constraint coefficient matrix
- `b`: Right-hand side (RHS) vector
- `c`: Cost coefficient vector

---

### `initialize_phase1(self)`
Sets up the Phase I problem by introducing artificial variables.

- Extends `A` by adding an identity matrix (one artificial variable per constraint).
- Defines the cost function to minimize the sum of artificial variables.
- Initializes the basis with artificial variables.

**Returns:**
- Extended constraint matrix `A_ext`
- Modified cost vector `c_ext`
- Initial basic variable indices `basis`

---

### `compute_reduced_costs(self, A, c, basis)`
Computes the reduced cost vector to determine whether the current solution is optimal.

- If the basis matrix is singular, it uses `np.linalg.pinv(B)` (pseudoinverse) instead of `np.linalg.inv(B)`.

**Returns:**
- Reduced cost vector `c_bar`

---

### `select_entering_variable(self, c_bar, basis)`
Selects the entering variable using the smallest index rule among variables with negative reduced cost.

**Returns:**
- Index of the entering variable, or `None` if optimal

---

### `compute_direction_vector(self, A, basis, entering_var)`
Computes the direction vector, which determines the effect of adding a new variable to the basis.

- Uses `np.linalg.pinv(B)` if the basis matrix is singular.

**Returns:**
- Direction vector `d`

---

### `select_leaving_variable(self, b, d, basis)`
Uses the minimum ratio test to determine which basic variable should leave.

**Returns:**
- Index of the leaving variable, or `None` if unbounded

---

### `update_basis(self, basis, entering_var, leaving_var)`
Updates the basis by swapping out the leaving variable with the entering variable.

---

### `phase1(self)`
Executes Phase I to find an initial feasible basis.

- If artificial variables remain in the basis at the end of Phase I, the LP problem is infeasible.

**Returns:**
- Initial feasible basis, or `None` if infeasible

---

### `phase2(self, A, c, basis, phase1=False)`
Executes Phase II to optimize the original problem.

- Iterates until an optimal solution is found or the problem is unbounded.

**Returns:**
- Optimal basis, or `None` if the problem is unbounded

---

### `solve(self)`
Runs both phases of the Simplex algorithm.

- Calls `phase1()` to find a feasible starting point.
- If feasible, runs `phase2()` to optimize the objective function.
- Detects infeasibility and unboundedness.

**Returns:**
- Optimal basis if a solution exists.
- `None` if the LP problem is infeasible.

**Edge Cases Handled**
- Singular Matrix Handling: Uses np.linalg.pinv() instead of np.linalg.inv() when needed.
- Infeasibility Detection: If artificial variables remain after Phase I, the problem is infeasible.
- Unbounded Solution Detection: If the direction vector is non-negative for all basic variables, the problem is unbounded.
- Degeneracy Handling: The solver continues iterating even when pivot steps do not improve the objective function.
