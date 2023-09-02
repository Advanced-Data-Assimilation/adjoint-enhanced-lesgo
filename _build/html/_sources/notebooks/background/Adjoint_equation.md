# Adjoint Advection-Diffusion Equation
Reference:
1. [The adjoint advection-diffusion equation in stationary and time dependent problems](http://rivista.math.unipr.it/fulltext/2001-4/02.pdf)
2. [Bayesian inference for source determination with applications to a complex urban environment](https://doi.org/10.1016/j.atmosenv.2006.08.044)
3. [PDE-constrained optimization and the adjoint method](https://cs.stanford.edu/~ambrad/adjoint_tutorial.pdf)
4. [Adjoint Equation](https://en.wikipedia.org/wiki/Adjoint_equation)


Define inner product as

$$
\langle f, g\rangle = \int_0^T\int_V fgdVdt
$$

Then, we could obtain the adjoint equation of the advection-diffusion equation by integration by part,

$$
\begin{aligned}
\langle \mathbf{L}_{c+}f, g\rangle 
= & \int_0^T\int_V \left(\frac{\partial f}{\partial t} + \boldsymbol{u}\cdot\nabla f- \frac{1}{Pe} \nabla^2f\right) gdVdt\\
= & \int_0^T\int_V \frac{\partial f g}{\partial t}-f\frac{\partial g}{\partial t}dVdt  \\
& + \left(\int_0^T\int_A \boldsymbol{u}fg \cdot \boldsymbol{n} \,dAdt - \int_0^T\int_V \nabla (\boldsymbol{u}g)  f \, dVdt\right) \\
& - \frac{1}{Pe}\left(\int_0^T\int_A  g\nabla f \cdot \boldsymbol{n} \,dAdt - \underbrace{ \int_0^T\int_V  \nabla g \cdot \nabla f\, dVdt}_{\int_0^T\int_A f\nabla g \cdot \boldsymbol{n}\,dAdt - \int_0^T\int_V f\nabla^2g \, dVdt}\right)\\
= & \int_0^T\int_A \left[\boldsymbol{u}fg- \frac{1}{Pe}(g\nabla f - f\nabla g) \right]\cdot \boldsymbol{n} \,dAdt - \int_0^T\int_V f\left[-\frac{\partial g}{\partial t} - \nabla(\boldsymbol{u}g) - \frac{1}{Pe}\nabla^2g\right] \, dVdt + \int_0^T\int_V \frac{\partial fg}{\partial t} \, dVdt\\
= & \int_0^T\int_A \left[\boldsymbol{u}fg- \frac{1}{Pe}(g\nabla f - f\nabla g) \right]\cdot \boldsymbol{n} \,dAdt + \int_0^T\int_V \frac{\partial fg}{\partial t} \, dVdt - \langle f, \mathbf{L}_{c-}g\rangle 
\end{aligned}
$$


> concomitant
> buffer region
> 