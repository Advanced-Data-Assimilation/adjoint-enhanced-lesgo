# Numerical Methods for CFD
This file is to record different numerical methods that have been deployed in lesgo.

## Crank-Nicolson

> In numerical analysis, the **Crank-Nicolson method** is a finite difference method used for numerically sovling the heat equation and similar partial differential equations. It is a second-order method in time. It is implicit in time, can be written as an implicit Runge-Kutta method, and it is numerically stable.
> 
> For diffusion equations (and many other equations), it can be shown the Crank-Nicolson method is unconditionally stable. <span style="color:red">However, the approximate solutions can still contain (decaying) spurious oscillations if the ratio of time step $\Delta t$ times the thermal diffusivity to the square of space step, $\Delta x^2$, is large(typically, larger than 1/2 per [Von Neumann stability analysis]). </span> For this reason, whenever large time steps or high spatial resolution is necessary, the less accurate backward Euler method is often used, which is both stable and immune to oscillations.

$$
\frac{u_i^{n+1} - u_i^n}{\Delta t} = \frac{1}{2} \left[ F_i^{n+1} + F_i^n\right]
$$


## Adams-Bashforth
IVP
$$
\begin{equation}
    \frac{\partial y}{\partial t} = f(t, y)
\end{equation}
$$
Then we have
$$
\begin{equation}
    y^{n+1} \approx y^n + \frac{3}{2}\Delta t f(t^n, y^n) - \frac{1}{2} \Delta t f(t^{n-1}, y^{n-1})
\end{equation}
$$


## Jacobian
Reference
https://www.stephanosterburg.com/math_jacobian

Should put into domain setup note.