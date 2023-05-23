# Domain
This file describes the simulation domains for LESGO code and parameters as well as subroutines for the non-dimensionlization.

## Problem Setup
![](imgs/setup-1.png)


Parameters of the simulation are listed in following table.

| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- |
| 2$\pi$ | $\pi$ | 1   | 128 | 128 | 64  | 180 | 0.0625 | 1   |


Stability criterion for the mesh griding and time step could be [here](Stability_Criterion.md).


## Fringe Region


## Problem
### Why w locates differently with respect to u, v?