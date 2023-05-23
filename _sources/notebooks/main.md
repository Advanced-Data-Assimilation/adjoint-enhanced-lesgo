
# Spatial reconstruction of steady scalar sources from remote measurements in turbulent flow
> “Identifying the source of passive scalar transported in a turbulent environment from remote measurements” (Wang et al., 2019, p. 316)


## Reference
1. [Spatial reconstruction of steady scalar sources from remote measurements in turbulent flow](https://doi.org/10.1017/jfm.2019.241)


## Governing Equation
Governing equation for the base flow is the Navier-Stokes Equation,

$$
\frac{\partial \boldsymbol{u}}{\partial t} + \boldsymbol{u} \cdot\nabla \boldsymbol{u} = -\nabla p + \frac{1}{Re}\nabla^2\boldsymbol{u}\\
\nabla\cdot\boldsymbol{u}=0
$$

We could use these equation to run the forward simulation and obtain the forward velocity field $\boldsymbol{u}_0, \boldsymbol{u}_1, \dots, \boldsymbol{u}_n$  at time $t_0, t_1, \dots, t_n$. We could use $\mathbf{L}_f$ to represent the forward operation on the velocity at each time step, i.e. $\mathbf{L}_f(\boldsymbol{u}_n)=\boldsymbol{u}_{n+1}$. 

<span style='color:red'> ??? might also need to use the adjoint method to determine the velocity field.</span> This is corresponding to the [*nonlinear-forward*](../lesgo_adjoint_tutorial_bundle/clean_version/lesgo_nonlinear_forward_obs)

**Passive Scalar**

Concentration $c$ is governed by the advection-diffusion equation, 
$$
\underbrace{\left(\frac{\partial }{\partial t} + \boldsymbol{u}\cdot\nabla - \frac{1}{Pe} \nabla^2\right)}_{\mathbf{L}}c = \phi(\boldsymbol{x}, t),
$$

in which $\phi$ is the source intensity with respect to time and spatial location, $Pe$ is $\text{P\'eclet}$ number $Pe \equiv Re \, Sc$.

> “Adopting Adams–Bashforth and Crank–Nicolson for temporal discretization of the advection and diffusion terms respectively” (Wang et al., 2019, p. 343)


<span style='color:red'> Question: How linearized? & What is the purpose of linearized code</spans>
<span style='color:navy'> Adams-Bashforth and Crank-Nicolson</span>

And the adjoint advection-diffusion equation is

$$
\underbrace{\left(-\frac{\partial }{\partial t} - \boldsymbol{u}\cdot\nabla - \frac{1}{Pe}\nabla^2\right)}_{\mathbf{L}^\dag}c^\dag = \phi^*(\boldsymbol{x}, t),
$$

in which $\phi^*$ is the observation at the sensor location. Derivation could be found [here](Adjoint_equation.md)


## Problem Setup

![](imgs/setup-1.png)


Parameters of the simulation are listed in following table.

| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- |
| 2$\pi$ | $\pi$ | 1   | 128 | 128 | 64  | 180 | 0.0625 | 1   |


Stability criterion for the mesh griding and time step could be [here](Stability_Criterion.md).

