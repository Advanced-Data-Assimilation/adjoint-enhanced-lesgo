```
bibliography: [../../ERI.bib]
```
# Pressure Solver
This file is to introduce how LESGO solve the pressure Poisson equation.

## Governing equation
Based on the fractional step method {cite}`chorinConvergenceDiscreteApproximations1969, kirisNumericalSolutionIncompressible2001a`, we have

$$
\begin{equation}
    \frac{u^{n+1}_{(i, j, k)} - u^*_{(i, j, k)}}{\Delta t} = -\frac{1}{\rho_0}\nabla p',
\end{equation}
$$ (eq:poisson)

in which $p'=p^{n+1}-p^{n}$ and $u^*$ is the auxillary velocity (details in [velocity solver](velocity_solver.md)).

Divergence of Eq.{eq}`eq:poisson` is the pressure Poisson equation, 

$$
\begin{equation}
(\nabla^2_x + \nabla^2_y + \nabla^2_z)p'_{(i, j, k)} = \frac{1}{\Delta t} \nabla \cdot u^*,
\end{equation}
$$

given that velocity of incompressible flow is divergence free $\nabla\cdot u^{n+1}=0$. Discretized in fourier space is

$$
\begin{equation}
a_{(k_i, k_j, k)}\widehat{p}_{(k_i, k_j, k-1)} + b_{(k_i, k_j, k)}\widehat{p}_{(k_i, k_j, k)} c_{(k_i, k_j, k)} + c_{(k_i, k_j, k)}\widehat{p}_{(k_i, k_j, k+1)}= \frac{1}{\Delta t} \left(\right) \widehat{u}^*,
\end{equation}
$$

in which,

$$
\begin{align}
a_{(k_i, k_j, k)} & = \frac{1}{\mathrm{d}\zeta_{k}\mathrm{d}\zeta_{kk}},\\
b_{(k_i, k_j, k)} &=  -(\kappa_x)_{(k_i, k_j)}^2 - (\kappa_y)_{(k_i, k_j)}^2 + \frac{1}{\mathrm{d}\zeta_{k}(\mathrm{d}\zeta_{kk} + \mathrm{d}\zeta_{kk+1})},\\
c_{(k_i, k_j, k)} & = \frac{1}{\mathrm{d}\zeta_{k}\mathrm{d}\zeta_{kk+1}}.
\end{align}
$$


```{note}
velocity in fourier space has same magnitude in real and imaginary part but in different sign, why?

[Solved] Multiply the imagenary $i$, complex -> -real and real-> complex.
```


```{admonition} Dealiasing
[ONGOING] only necessary for nonlinear term.
```

```{admonition} Boundary Condition
On the top and bottom boundary, the pressure poisson solver is actually solving

$$
\frac{\widehat{p}'_{(i, j, 1)} - \widehat{p}'_{(i, j,0)}}{\Delta \zeta_{0}} = -\widehat{(\nabla\cdot \tau)}_{(i, j, 0)}
$$

**WHY?**
Probably related to the Eq.(7.9) in Pope's book {cite}`popeTurbulentFlows2000a`,

$$
\frac{\dd p}{\dd x} = \frac{\dd \tau}{\dd y}
$$

but there is some difference between them. In the poisson solver, we are actually solving the difference of pressure $p'$.

Need further investigation. Also, this might also related to how the Reynolds number is set up for the simulation.
```





````{margin}
```{admonition} Question
<span style='color:navy;font-weight:bold'> Why calculate the solution of zero wave number independently?</span>

**Answer**

Coefficient of zero wave number is the same as the average on xy plane,

$$
\hat{f}(0) = \frac{1}{N}\sum_{j=0}^{N-1} X_j.
$$

Thus, $\hat{f}(0)$ is not a function of $x$ and $y$, so that its derivatives in x and y direction should all be zero, which makes the solution of zero wave number different from others. 

```
````

And the solver solve the zero wave number ($k_x = k_y = 0$) separately, based on

$$
\widehat{P'}_{(k_x=0, k_y=0, k)} - \widehat{P'}_{(k_x=0, k_y=0, k-1)}= -\frac{\Delta z}{\Delta t} \hat{w}_{(k_x=0, k_y=0, k)}.
$$





```{bibliography}
```
