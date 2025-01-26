# PairSAT CNF Formula Generator

This repository contains a Python script (`cnf-fuzz-pairSAT.py`) that generates Conjunctive Normal Form (CNF) formulas in the DIMACS format. The generator is specifically designed for model counting. The name “PairSAT” comes from its pair-wise clause generation strategy: 

- **Odd-numbered clauses** are generated randomly.
- **Even-numbered clauses** are derived from the previous clause, with random polarity flips or literal changes.

## Features

- Generates CNF formulas in **DIMACS** format.
- Configurable through a variety of parameters to suit different experimental needs.
- Randomness is tied to a **seed** for reproducibility.
- Designed to integrate with [SharpVelvet](https://github.com/meelgroup/SharpVelvet) as a generator in `src/generators`.

## Parameters

The following parameters can be set when running the script:

| Parameter                | Type     | Default | Description                                                                 |
|--------------------------|----------|---------|-----------------------------------------------------------------------------|
| `-s`, `--seed`           | `int`    | `None`  | Set the random seed for reproducibility.                                    |
| `--instances`            | `int`    | `1`     | Number of CNF instances to generate.                                        |
| `--threads`              | `int`    | `1`     | Number of threads for parallel generation.                                  |
| `--min-clauses`          | `int`    | `400`   | Minimum number of clauses per instance.                                     |
| `--max-clauses`          | `int`    | `400`   | Maximum number of clauses per instance.                                     |
| `--min-vars`             | `int`    | `90`    | Minimum number of variables per instance.                                   |
| `--max-vars`             | `int`    | `90`    | Maximum number of variables per instance.                                   |
| `--min-clause-len`       | `int`    | `2`     | Minimum number of literals in a clause.                                     |
| `--max-clause-len`       | `int`    | `6`     | Maximum number of literals in a clause.                                     |
| `--min-refs`             | `int`    | `1`     | Minimum number of references for each variable across all clauses.          |
| `--max-refs`             | `int`    | `50`    | Maximum number of references for each variable across all clauses.          |
| `--allow-taut`           | `flag`   | `False` | Allow tautological clauses (e.g., containing both `x` and `-x`).            |
| `--balanced`             | `flag`   | `False` | Distribute variable usage more evenly.                                      |
| `--ratio`                | `float`  | `None`  | Clause-to-variable ratio. Overrides `--min-clauses` and `--max-clauses`.    |

## Integration with SharpVelvet

PairSAT is designed to integrate with SharpVelvet. To use this generator within SharpVelvet:

1. Place `cnf-fuzz-pairSAT.py` in the `src/generators` directory of SharpVelvet.

2. Update the SharpVelvet configuration to recognize PairSAT as a new generator according to SharpVelvet Instructions.

## Output Format

Generated CNF formulas are output in the standard DIMACS format. Example:

```
p cnf 3 5
c t mc
-3 -1 0
2 -1 0
-2 0
-2 1 0
1 3 2 0
```

This specifies a formula with 3 variables and 5 clauses.

## Notes
- The generator ensures formulas are satisfiable by modifying literals if necessary.
- Paired clause generation can lead to possibly higher solution counts if not carefully configured. We found generator to perform the best in clause to variable ratio between 4 and 5.

Citing

```
@software{PairSAT_2025,
  author = {Vuk Jurišić},
  title = {PairSAT},
  url = {https://github.com/Chevuu/PairSAT},
  date = {2025-01-26},
}
```
