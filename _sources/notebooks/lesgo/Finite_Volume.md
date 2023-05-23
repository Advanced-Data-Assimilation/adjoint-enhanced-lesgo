---
bibliography: [../ERI.bib]
---
# Finite Volume
This file is created to record how the finite volume method is applied into the scalar transport module.




## Fourier's + CN  

Fourier transform is applied on the LHS to obtain the derivative and construct the matrix of next time step.
### Governing equation

The scalar transport is governed by
$$
\begin{equation}
    \frac{\partial c}{\partial t} + \boldsymbol{u} \cdot \nabla c = \frac{1}{Pe}\nabla^2 c + \phi.
\end{equation}
$$

Integration over a finite volume $V$ with surrounding surface $S$, and adopting Adams-Bashforth and Crank-Nicolson for temporal discretization of the advection and diffusion terms respectively, we have
$$\begin{aligned}
    \frac{\delta c}{\Delta t} 
    - \frac{1}{2VPe}\iint_S (\boldsymbol{n} \cdot \nabla \delta c) \mathrm{d}S
    = & - \frac{1}{2V} \left( 
        3\underbrace{\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S \right]^n}_{\mathbf{F}^n\mathbf{c}^n} 
        - \underbrace{\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S \right]^{n-1} }_{\mathbf{F}^{n-1}\mathbf{c}^{n-1}}
        \right) \\
        & + \underbrace{\frac{1}{V}\left[\frac{1}{Pe} \iint_S (\boldsymbol{n}\cdot\nabla c) \mathrm{d}S\right]^n}_{\mathbf{D}_{ex}\mathbf{c}^n}
        + \underbrace{\frac{1}{V}\left[\int_V \phi \mathrm{d}V\right]^n}_{\phi^n}
\end{aligned}
$$

According to [Crank-Nicolson](numerical_methods.md#crank-nicolson) and [Adams-Bashforth method](numerical_methods.md#adams-bashforth), we have
$$\begin{align}
    \frac{\partial c}{\partial t} 
    =& D(\boldsymbol{u}, \boldsymbol{x}, c) + F(\boldsymbol{x}, c) + \phi,\\
    \frac{c^{n+1} - c^{n}}{\Delta t}
    \approx & \frac{1}{2}\left(D(\boldsymbol{u}^{n+1}, \boldsymbol{x}^{n+1}, c^{n+1}) + D(\boldsymbol{u}^{n}, \boldsymbol{x}^{n}, c^{n}) \right)\\
    & + \frac{1}{2}\left(3F(\boldsymbol{x}^{n}, c^{n})  - F(\boldsymbol{x}^{n-1}, c^{n-1}) \right)
    + \phi,\\
\end{align}
$$
in which
$$
\begin{equation}
    D(\boldsymbol{u}, \boldsymbol{x}, c) = \frac{1}{Pe} \iint_S (\boldsymbol{n}\cdot\nabla c) \mathrm{d}S,
    \quad
    F(\boldsymbol{x}, c)= \iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S.
\end{equation}
$$


## Problem 

### How the derivative should be calculated on the y=0 boundary. - May 5
<span style='color:red;font-weight:bold'>The index of x direction is ld = 1:nx+2 while index of y direction is 1:ny, which make the locations of extra two cell grids in x direction puzzled to me. Are cells with indices ld=0 and ld=nx+2 the ghost cells outside the domain box? Or they are those satisfy the periodic BC as f(ld=0)=f(ld=nx+1) and f(1)=f(nx+2)? Both case don't show consistence with y direction... The only possible reason that I could imagine right now is that this is the way FFTW read in the data and do the FFT. In this case, I think we mind get into trouble to get the value or derivative at y=0 and y=Ly unless it is periodic in y direction. </span>


[4.8.6 Multi-dimensional Transforms](https://www.fftw.org/fftw3_doc/Multi_002ddimensional-Transforms.html)

Related to the FFTW. Check derivative.f90.
```fortran
    ! Kill oddballs in zero padded region and Nyquist frequency
    f(ld-1:ld,:,jz) = 0._rprec
    f(:,ny/2+1,jz) = 0._rprec
```

Take f(ld=1:nx) is the data at the center in each cell for now.

### Boundary Condition on ddz for scalar.f90.
Upper BC and lower BC.


## ADI + CN + AB
The scalar transport is governed by
$$
\begin{equation}
    \frac{\partial c}{\partial t} + \boldsymbol{u} \cdot \nabla c = \frac{1}{Pe}\nabla^2 c + \phi.
\end{equation}
$$

Integration over a finite volume $V$ with surrounding surface $S$, and adopting Adams-Bashforth and Crank-Nicolson for temporal discretization of the advection and diffusion terms respectively, we have
$$\begin{aligned}
    \frac{\delta c}{\Delta t} 
    - \frac{1}{2VPe}\iint_S (\boldsymbol{n} \cdot \nabla \delta c) \mathrm{d}S
    = & - \frac{1}{2V} \left( 
        3\underbrace{\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S \right]^n}_{\mathbf{F}^n\mathbf{c}^n} 
        - \underbrace{\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S \right]^{n-1} }_{\mathbf{F}^{n-1}\mathbf{c}^{n-1}}
        \right) \\
        & + \underbrace{\frac{1}{V}\left[\frac{1}{Pe} \iint_S (\boldsymbol{n}\cdot\nabla c) \mathrm{d}S\right]^n}_{\mathbf{D}_{ex}\mathbf{c}^n}
        + \underbrace{\frac{1}{V}\left[\int_V \phi \mathrm{d}V\right]^n}_{\phi^n}
\end{aligned}
$$

According to [Crank-Nicolson](numerical_methods.md#crank-nicolson) and [Adams-Bashforth method](numerical_methods.md#adams-bashforth), we have
$$\begin{align}
    \frac{\partial c}{\partial t} 
    =& D(\boldsymbol{u}, \boldsymbol{x}, c) + F(\boldsymbol{x}, c) + \phi,\\
    \frac{c^{n+1} - c^{n}}{\Delta t}
    \approx & \frac{1}{2}\left(D(\boldsymbol{u}^{n+1}, \boldsymbol{x}^{n+1}, c^{n+1}) + D(\boldsymbol{u}^{n}, \boldsymbol{x}^{n}, c^{n}) \right)\\
    & + \frac{1}{2}\left(3F(\boldsymbol{x}^{n}, c^{n})  - F(\boldsymbol{x}^{n-1}, c^{n-1}) \right)
    + \phi,\\
\end{align}
$$
in which
$$
\begin{equation}
    D(\boldsymbol{u}, \boldsymbol{x}, c) = \frac{1}{Pe} \iint_S (\boldsymbol{n}\cdot\nabla c) \mathrm{d}S,
    \quad
    F(\boldsymbol{x}, c)= \iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S.
\end{equation}
$$

### Advective Term
For the advective term
$$
\begin{equation}
    F(\boldsymbol{x}, c)= \iint_S (\boldsymbol{u}\cdot\boldsymbol{n}c) \mathrm{d}S. 
\end{equation}
$$
we need to determine the velocity, scalar concentration and area of each cell faces.
A third-order upstream central scheme (HOUC3) algorithm is incorporated to interpolate the scalar concentration on the west and east faces using biased stencils [@nourgalievHighfidelityInterfaceTracking2007], 
$$
\begin{aligned}
    c^{w-}_{(i, j, k)} &= \frac{1}{6}(-c_{i-2, j, k} + 5c_{i-1, j, k} + 2c_{i, j, k}),\\
    c^{e+}_{(i, j, k)} &= \frac{1}{6}(-c_{i+2, j, k} + 5c_{i+1, j, k} + 2c_{i, j, k}).\\
\end{aligned}
$$