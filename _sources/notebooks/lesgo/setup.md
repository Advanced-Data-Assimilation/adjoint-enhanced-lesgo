# Setup
## Domain
This file describes the simulation domains for LESGO code and parameters as well as subroutines for the non-dimensionlization.

## Problem Setup
![](../figures/setup-1.png)


Parameters of the simulation are listed in following table.

| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- |
| 2$\pi$ | $\pi$ | 1   | 128 | 128 | 64  | 180 | 0.0625 | 1   |


Stability criterion for the mesh griding and time step could be [here](Stability_Criterion.md).



## Coordinates
LESGO originally has two mesh grids to store the data: uv-grid(same as $(i, j, k)$ coordinate in {numref}`fig:velocity_coords`).  and w-grid(same as $(i, j, kk)$ coordinate in {numref}`fig:velocity_coords`). Here, I introduce two more grid locations, $(ii, j, k)$ and $(i, jj, k)$.


```{figure} ../drawings/mesh/velocity.png
---
width: 300px
name: fig:velocity_coords
---
Vectors coordinates, etc. velocity.
```

```{figure} ../drawings/mesh/scalar.png
---
width: 300px
name: scalar_coords
---
Scalars coordinates, etc. temperature.
```




### Vertical stretching

## Fringe Region



# Variable

```{Note}
what BOGUS do?
```
## Problem
### Why w locates differently with respect to u, v?
