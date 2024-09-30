# SAT Solver

A C++ [SAT solver](http://en.wikipedia.org/wiki/Boolean_satisfiability_problem#Algorithms_for_solving_SAT) implementation, based on the [DPLL](http://en.wikipedia.org/wiki/DPLL_algorithm) algorithm.

## Table of Contents

1. Implemented Features
2. Results
3. Source and Scripts
4. Compiling and Executing the Solver

Please refer to the *Results* and *Source and Scripts* sections to get an overview of the project's structure.

## Implemented Features

Several optimizations and improvements were introduced to enhance the performance of the SAT solver. These include:

### Occurrence Lists

The Boolean Constraint Propagation (BCP) procedure, implemented in the `propagateGivesConflict()` function, was optimized to avoid scanning all clauses during each propagation. Instead, two lists called *occurrence lists* are maintained for each variable.

- `positiveClauses`: a list of clauses where the variable appears as a positive literal.
- `negativeClauses`: a list of clauses where the variable appears as a negative literal.

These lists are built during initialization and store pointers to the respective clauses for efficient access. When propagating a literal, the algorithm only needs to inspect clauses that contain the literal, reducing the overhead of BCP.

### Activity-Based Decision Heuristic

The decision heuristic for selecting the next literal to be decided in the DPLL algorithm was updated to use an *activity-based heuristic*:

- Each literal (both positive and negative) has an associated *activity score*, which tracks how frequently the literal is involved in conflicts.
- When a conflict is encountered, the activity score of each literal in the conflicting clause is incremented.
- To give preference to more recent conflicts, the activity scores are periodically decayed by dividing them by 2 every 1000 conflicts.
- During decision making, the literal with the highest activity score among undefined variables is chosen.

This heuristic improves performance by prioritizing literals that are likely to lead to rapid conflict resolution and backtracking.


## Results

The `results` directory contains the results for the sample problem instances located in the `sample_problems` directory. The key files include:

- `results.txt`: A quick reference to the expected results for each problem.

Additionally, the `output` directory contains raw output from the solver for each problem instance.

## Source and Scripts

### Source

The main source code for the SAT solver is located in the `sat_solver` directory in the `sat.cpp` file. The implementation includes the DPLL algorithm, the optimized BCP with occurrence lists, and the activity-based heuristic.

### Scripts

Several scripts are provided to simplify the compilation and execution of the solver:

- `Makefile`: A `make` script to compile the solver.
- `sat.sh`: A wrapper script that compiles and runs the solver with a specified input file.
- `runner.sh`: A script to run the solver on all problem files in the `sample_problems` directory.

## Compiling and executing the solver

To compile the solver, execute:

```bash
./sat.sh compile
```

To run the solver with an example input problem:

```bash
./sat.sh run sample_problems/vars-100-1.cnf
```
 -->