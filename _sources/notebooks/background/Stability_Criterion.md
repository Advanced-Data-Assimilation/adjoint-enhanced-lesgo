# Stability Criterion
This file is to tackle the stability problem.



## Reference
1. [Stability of Finite Difference Schemes on the Diffusion Equation with Discontinuous Coefficients](https://math.mit.edu/research/highschool/rsi/documents/2017Lee.pdf)
2. [Lecture Note](https://math.mit.edu/~stoopn/18.086/Lecture7.pdf)

## Criterion
**CFL**
For advection process, the criterion is usually called CFL:

$$
r = \frac{u\Delta t}{\Delta x} \le 1
$$

**Forward Euler Difussion**
For diffusion process, lesgo use spectrum method on xy-plane and finite difference method in z direction. So we only need to consider the criterion on z direction.

$$
R = \frac{1}{Re \cdot Pr}\frac{\Delta t}{\Delta x^2} \le \frac{1}{2}
$$


**Cell Peclet Number**

## Examples
### Setup 1
| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  | u       | v       | w       | theta  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- | ------- | ------- | ------- | ------ |
| 2$\pi$ | $\pi$ | 1   | 128 | 128 | 64  | 180 | 0.0625 | 1   | [0, 23] | [-5, 5] | [-5, 5] | [0, 1] |

- velocity
  - dt = 6.687330163043478e-05
  - R = 0.0006134498888122076
- scalar
  - CFL = 0.008559782608695651
  - Pe = 2.21484375


### Setup 2
| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- |
| 2$\pi$ | $\pi$ | 1   | 256 | 256 | 128 | 180 | 0.0625 | 1   |