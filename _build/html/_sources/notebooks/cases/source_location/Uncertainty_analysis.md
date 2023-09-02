# Source Reconstruction
This file is created to describe methods that used to reconstruct the spatial location of steady passive scalar source.

- [Source Reconstruction](#source-reconstruction)
  - [Background -- Duality relation](#background----duality-relation)
  - [Time integral adjoint field](#time-integral-adjoint-field)
  - [Principal Components Analysis](#principal-components-analysis)
    - [Mathematical Background](#mathematical-background)

## Background -- Duality relation
According to the derived adjoint equation of scalar transport equation in [here](../../background/Adjoint_equation.md), we could obtain the duality relationship between the forward scalar field $c(\boldsymbol{x}_m, t_m)$ and adjoint field $c^\dag(\boldsymbol{x}_s, t_s)$,
$$
\begin{aligned}
    c^\dag(\boldsymbol{x}_s, t_s) &= \langle \delta(\boldsymbol{x} - \boldsymbol{x}_s)\delta(t - t^2), c^\dag\rangle\\
    & = \langle \mathbf{L}c, c^\dag\rangle = \langle c, \mathbf{L}^\dag c^\dag\rangle = \langle c, \delta(\boldsymbol{x} - \boldsymbol{x}_m\delta(t-t_m) )\rangle = c(\boldsymbol{x}_m ,t_m),
\end{aligned}
$$
which demonstrates that the sensor measurements at $(\boldsymbol{x}_m, t_m)$ due to a scalar impulse at $(\boldsymbol{x}_s, t_s)$ are identical to the adjoint signal at $(\boldsymbol{x}_s, t_s)$ due to an adjoint impulse at $(\boldsymbol{x}_m, t_m)$.

## Time integral adjoint field
Adjoint fields of previous method {cite}`wangSpatialReconstructionSteady2019a` depend on time when the source is released. However, we could integrate the adjoint field in time to obtain an averaged adjoint field,
$$
c(\boldsymbol{x}_m, t_m; \boldsymbol{x}_s) = \boldsymbol{I}_s \int_\tau c^\dag(\boldsymbol{x}_s, \tau) \mathrm{d}\tau = \boldsymbol{I}_s C^\dag(\boldsymbol{x}_s),
$$
which could be used to reconstruct the spatial location of steady passive scalar source.


<span style='color:red; font-weight:bold'> Qeustion: how to determine the span of time integration? </span>



## Principal Components Analysis

### Mathematical Background
$$
\log{c^\dag_i(\boldsymbol{x}_s)} \sim GP(\overline{\log{c^\dag_i}}, \mathbf{K}_i(\boldsymbol{x}_{s1}, \boldsymbol{x}_{s2})), \qquad i = 1, 2, \dots, N.
$$

$$
\log{c^\dag_i} \approx \overline{\log{c^\dag_i}} + \sum_{l}\alpha_l \phi_l(\boldsymbol{x})
$$