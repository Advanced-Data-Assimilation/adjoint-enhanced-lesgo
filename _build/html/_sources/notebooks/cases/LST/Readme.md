# Linear Stability Analysis
This part uses Linear Stability Theory(LST) to check on the validity of LESGO solver accuracy. Details and calculations are elaborated in [here](./LST.ipynb).

## Eigen-mode validation
We would like to compare the eigenvalues and eigenvectors of the solver and below we show several eigen-mode that has small wavenumber which is crucial to satisfy the boundary condition in solver.


### Half-Channel
Eigenvalues of half-channel solver are shown in below figure.

![](./imgs/omega.png)

All eigenvalues are smaller than 1 so that the solver is stable.

#### One Wave

For the eigen-modes has only one wave, corresponding eigenvalues are labeled with green and red cross in following figure, which is pretty closed to each other.

![](./imgs/omega_lesgo215_LST119.png)

Eigen-mode of LST and LESGO result are shown in following figure. There are slight difference between these two result, but I think the error is acceptable.

![](./imgs/one_wave.png)

#### Two Wave
Followed by the eigenvalues with two wave eigen-mode. For now, the eigenvalues of two different methods are far away from each other, which need to solved.


![](./imgs/omega_lesgo216_LST144.png)

On the other hand, the eigen-modes of different methods match pretty well.

![](./imgs/two_wave.png)