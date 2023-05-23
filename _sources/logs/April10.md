# [Report for April 10](https://github.com/Userzj123/ERI/blob/main/logs/April10.md)
This file is created to present recent work on scalar reconstruction project.
- [Report for April 10](#report-for-april-10)
  - [Literatures review on Source Reverse Problem](#literatures-review-on-source-reverse-problem)
    - [Stochastic sampling algorithms](#stochastic-sampling-algorithms)
  - [Review on Thermal Boundary Layer](#review-on-thermal-boundary-layer)
  - [Simulation results of LESGO for multi-scalar with implicit method and explicit methods](#simulation-results-of-lesgo-for-multi-scalar-with-implicit-method-and-explicit-methods)
  - [Backward simulation](#backward-simulation)
  - [Stochastic Database](#stochastic-database)
  - [Conclusion](#conclusion)


## Literatures review on Source Reverse Problem
1. What method has been used to solve the reverse problem?

2. What is the challenges for each solution?
3. Simple test case setup for our new method.

### Stochastic sampling algorithms 
**Reference:**
[Source Inversion for Contaminant Plume Dispersion in Urban Environments Using Building-Resolving Simulations](http://journals.ametsoc.org/doi/10.1175/2007JAMC1733.1)

*Method:* 
Based on Bayesian inference via stochastic sampling algorithms with a high-resolution computational fluid dynamics model.

*Challenge:*: 
1. Only consider source on the ground. 
2. Only consider steady-state. (because stochastic method convergence require iteration, which reflects an overwhelming amount of computation on CFD)

*Test case:* **Isolated building example**
> “We have developed an event reconstruction prototype for a flow around an isolated building (a cube) with a source located upwind from the building (see Fig. 1). Four sensors are placed in a diamond-shaped array in the lee of the building. Data at the sensor locations are collected using a forward simulation from the true source location. The data are thus “synthetic” and used in this case only to test the inversion algorithm. Artificial measurement error with a standard lognormal distribution is also added to the synthetic data (in this case with a mean of 0 and a standard deviation of rel 0.05).” ([Chow et al., 2008, p. 1557](zotero://select/library/items/MJC2RPP4)) ([pdf](zotero://open-pdf/library/items/F9X88UAX?page=5&annotation=P3M3XY8U))

Further detail can be found [here](../docs/reviews/Source_Reverse_Problem.md)

## Review on Thermal Boundary Layer
**Reference:**
[Boundary Layer Theory: Chapter 9](http://link.springer.com/10.1007/978-3-662-52919-5)
> “In this chapter we will first discuss those particular flows with heat transfer for which the velocity field is decoupled from the temperature field. This is the case if the physical properties $\rho$ and $\mu$ are constant, i.e. can be assumed to be independent of the temperature and pressure. This assumption is certainly justified as long as the temperature and pressure differences within the boundary layer are small.” ([Schlichting and Gersten, 2017, p. 209](zotero://select/library/items/DIAHPUMV))
>
> “The velocity field will now be assumed to be that of a flow at high Reynolds number, i.e. the flow will have boundary–layer character.” ([Schlichting and Gersten, 2017, p. 210](zotero://select/library/items/DIAHPUMV))
>
>$$
\begin{aligned}
x^{\prime}=\frac{x}{l}, & y^{\prime}=\frac{y}{l}, & u^{\prime}=\frac{u}{V}, & v^{\prime}=\frac{v}{V}, & \theta=\frac{T-T_{\infty}}{\Delta T}
\end{aligned}
$$
>
> “the equation for the thermal boundary layer” ([Schlichting and Gersten, 2017, p. 210](zotero://select/library/items/DIAHPUMV)) 
>
>$$
\begin{aligned}
u^{\prime}\frac{\partial \theta}{\partial x^{\prime}} + \overline{v}\frac{\partial \theta}{\partial \overline{y}} = \frac{1}{\text{Pr}} \frac{\partial^2 \theta}{\partial \overline{y}^2} + \text{Ec} \left(\frac{\partial u^{\prime}}{\partial \overline{y}}\right)^2
\end{aligned}
$$
>
> “There are more different kinds of boundary conditions possible for thermal boundary layers than for velocity boundary layers, since these latter are fixed by the no–slip condition and the impermeability of the wall.” ([Schlichting and Gersten, 2017, p. 211](zotero://select/library/items/DIAHPUMV)) 
> 
> “its general solution can be written down as a superposition of the solution without dissipation and the solution which is due to the dissipation:” ([Schlichting and Gersten, 2017, p. 211](zotero://select/library/items/DIAHPUMV))

It looks like the analytical solution to the thermal boundary layer is closely related to the boundary condition on the wall, which requires further investigation.

## Simulation results of LESGO for multi-scalar with implicit method and explicit methods

| Lx     | Ly    | Lz  | nx  | ny  | nz  | Re  | CFL    | Pr  |
| ------ | ----- | --- | --- | --- | --- | :-: | ------ | --- |
| 2$\pi$ | $\pi$ | 1   | 256 | 256 | 128  | 180 | 0.00625 | 1   |

<span style="color:red">Gonna change the value of Prontal number and CFL for futher simulation.</span>

**Explicit method**
![](imgs/14020330.gif)
![](imgs/14020330_theta01.gif)

**Implicit method**
![](imgs/14041032.gif)

Comparison is not very obvious since the implicit method only runs for 1k time step but both simulations are stable. Previous error might because of mistaking the magnitude of IC.

## Backward simulation
Backward simulation is still running but there should be no problem. But rockfish HPC seems to not support jobs that require more than 1 node recently ( it would show up some network communication error). 

## Stochastic Database
I might be able to start working on this part from this week and I am wondering if it is possible to meet on Thursday or later to have a discussion on this part. Also it kind of worries me that we might need to shift to fermi if rockfish still have the network problem.

## Conclusion

That's all I have to update for this week and thank you so much.