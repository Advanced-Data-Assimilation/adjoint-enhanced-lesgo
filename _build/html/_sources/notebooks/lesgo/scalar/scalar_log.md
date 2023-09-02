# Scalar Log

This file is created to demonstrate which variables are interested to scalar solvers and embedded in scalar log module.

## Conservation
Priority concern is if the scalar solver is obeying the conservation law. So there are two functions that could be used to calculate the flux on lbc and ubc (regardless of the boundary conditions).

**Governing Equation**
The transportation of passive scalar is governed by the advection-diffusion equation,
$$
\left(\frac{\partial }{\partial t} + \boldsymbol{u}\cdot\nabla - \frac{1}{Pe} \nabla^2\right) c = \phi(\boldsymbol{x}, t).
$$

For the stable solution, time derivative is zero so that there is a balance between diffusion plus advection processes across the boundary and the source term,
$$
\begin{aligned}
\iiint \nabla \cdot \left( \boldsymbol{u}c - \frac{1}{Pe} \nabla c \right) \mathrm{d}V& = \iiint \phi(\boldsymbol{x}, t)\mathrm{d}V,\\
\oiint_{S} \left( \boldsymbol{u}c - \frac{1}{Pe} \nabla c \right) \cdot \boldsymbol{n} \mathrm{d}S& = \iiint \phi(\boldsymbol{x}, t)\mathrm{d}V,\\
flux = \frac{1}{S}\oiint_{S} \left( \boldsymbol{u}c - \frac{1}{Pe} \nabla c \right) \cdot \boldsymbol{n} \mathrm{d}S& = \int \phi(\boldsymbol{x}, t)\mathrm{d}z,\\
\end{aligned}\\
$$