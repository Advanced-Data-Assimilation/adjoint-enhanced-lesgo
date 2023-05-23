

# Scalar.f90
This file is created to record the mathematical background inside scalar.f90 subroutine.

## Governing Equation

$$
\underbrace{\left(\frac{\partial }{\partial t} + \boldsymbol{u}\cdot\nabla - \frac{1}{Pe} \nabla^2\right)}_{\mathbf{L}}c = \phi(\boldsymbol{x}, t),
$$

$$
\frac{\partial c}{\partial t}  = f = \frac{1}{Pe}\nabla^2c - \boldsymbol{u}\cdot \nabla c + \phi(\boldsymbol{x}, t)
$$


Take $\phi(\boldsymbol{x}, t)$ as single impulse at initial condition,

$$
\phi(\boldsymbol{x}, t) = \delta\boldsymbol{x}\delta{t}
$$

## Implicit in time 

Crank-Nicolson Method, detail in [here](Crank-Nicolson.md)

$$
    \frac{c_i^{n+1} - c_i^n}{\Delta t} = \frac{1}{2} \left[ f_i^{n+1} + f_i^n\right] \\
$$

Expand the right-hand side and substitute $\kappa = \frac{1}{Pe}$ we have

$$
\begin{aligned}
    f_i^n = &  \kappa_i^n \left(\nabla^2_x + \nabla^2_y + \nabla^2_z \right)c_i^n - \boldsymbol{u}_i^n\cdot \nabla c_i^n + \phi_i^n \\
    = & \mathcal{F}^{-1}\left(\right) + \kappa_i^n \nabla^2_z c_i^n + \phi_i^n \\
    = & F_i^n + \kappa_i^n \nabla^2_z c_i^n + \phi_i^n 
\end{aligned}
$$

<span style='color:red'> How spectral method deal with fourier transform in different direction with different discretization? </span>
<span style='color:red'> What about more general case? Kappa is function of space and time. </span>

### cn_diff_stag_array_theta
```fortran
! Set constants
const1 = dt / (2._rprec*dz)
const2 = 1._rprec / dz
```

$$
C_1 = \frac{\Delta t}{2\Delta z},\quad
C_2 = \frac{1}{\Delta z}
$$

```fortran
! Initialize with the explicit terms
Rtheta(:,:,1:nz-1) = theta(:,:,1:nz-1) + dt * ( tadv1 * RHS_T(:,:,1:nz-1) + tadv2 * RHS_Tf(:,:,1:nz-1) )
```
use R to represent Rtheta

$$
R_i^{n} = c_i^n + \Delta t (\alpha F_i^{n} + (1-\alpha)F_i^{n-1})
$$

<span style="color:red"> How to determine the value of $\alpha$ ? - related to CFL</span> <tag question>

```fortran
! Add explicit portion of Crank-Nicolson
call ddz_w(pi_z, temp_var, lbz) ! ddz_w -> This subroutine computes the partial derivative of f with respect to z using 2nd order finite differencing.
temp_var(ld-1:ld, :, 1:nz-1) = 0._rprec
#ifdef PPSAFETYMODE
#ifdef PPMPI
temp_var(:,:,0) = BOGUS
#endif
temp_var(:,:,nz) = BOGUS
#endif
Rtheta(:,:,1:nz-1) = Rtheta(:,:,1:nz-1) -                           &
    dt * 0.5_rprec * temp_var(:,:,1:nz-1)
```

use $\Gamma$ to represent temp_var, done by 2nd order finite difference.

$$
\Gamma_i^n = \frac{\partial}{\partial z}\left(\kappa\frac{\partial c_i^n}{\partial z}\right)
$$ 

and set no flux on the boundary on x direction. <span style="color:red"> why ld-1:ld? Is ld set to be equal to 1?</span>
<span style='color:red'> Shouldn't the sign of diffusion term be positive?</span>

And then $R$ is updated again,

$$
\begin{aligned}
R_i^{n+1} &= R_i^{n+1} - \frac{1}{2}\Delta t \Gamma_i^n\\
&= c_i^n + \Delta t (\alpha F_i^{n+1} + (1-\alpha)F_i^{n})- \frac{1}{2}\Delta t \frac{\partial}{\partial z}\left(\kappa\frac{\partial c_i^n}{\partial z}\right)
\end{aligned}
$$


```fortran
! Compute coefficients in domain
do jz = jz_min, jz_max
do jy = 1, ny
do jx = 1, nx
    ! molec = true and sgs = false only
    kappa_a = Kappa_t(jx,jy,jz)
    kappa_b = (Kappa_t(jx,jy,jz+1)/jaco_w(jz+1)) + (Kappa_t(jx,jy,jz)/jaco_w(jz))
    kappa_c = Kappa_t(jx,jy,jz+1)

    a(jx, jy, jz) = -const1*(1._rprec/jaco_uv(jz))*const2*(1._rprec/jaco_w(jz))*kappa_a
    b(jx, jy, jz) = 1._rprec + const1*(1._rprec/jaco_uv(jz))*const2*kappa_b
    c(jx, jy, jz) = -const1*(1._rprec/jaco_uv(jz))*const2*(1._rprec/jaco_w(jz+1))*kappa_c
end do
end do
end do
```
**Bottom and Top is abbreviated.**
Use $r$ to represent jaco_uv and $l$ to represent jaco_w, detail of jacobian matrix for stretched grids can be found [here](). <span style="color:red"> link to domain setup note</span>
i, j, k represent index in x, y, z direction.

$$
\kappa_a = \kappa_k\\
\kappa_b = \kappa_{k+1}/l_{k+1} + \kappa_k/l_k\\
\kappa_c = \kappa_{k+1}
$$

And components for the tridiagonal matrix,

$$
\begin{aligned}
a_k &= -C_1\frac{1}{r_{k+1}}C_2\frac{1}{l_{k}}\kappa_a\\
& = - \frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k+1}l_{k}}\kappa_k\\
b_k &= 1 + C_1\frac{1}{r_k}C_2\kappa_b\\
& = 1+ \frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k}}\left(\kappa_{k+1}/l_{k+1} + \kappa_k/l_k\right)\\\\
c_k & = - C_1\frac{1}{r_k}C_2\frac{1}{l_{k+1}}\kappa_c\\
& = - \frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k}l_{k+1}}\kappa_{k+1}\\
\end{aligned}
$$

```fortran
! Using uv tridag routine since theta is on uv, same discretization
call cn_tridag_array_uv ( a, b, c, Rtheta, theta_sol )
```
**Tridiagonal Matrix**

$$
\mathbf{A} = 
\begin{bmatrix}
    b_0 &  c_0 & & &  \\
    a_1 & b_1 & c_1 & &\\
    & a_2 & b_2 & c_2 &\\\\
    & & \ddots & \ddots & \ddots \\
    & & &a_{k-1}& b_{k-1} & c_{k-1}\\\\
    & & & &  a_k & b_k\\
\end{bmatrix}
$$

0 --> bottom, k--> top.

And we have $A_{kk}^{n+1}c_k^{n+1} = R_k^{n+1}$ <span style="color:blue"> why? what exact formula is that</span> 

> $$
a_{k-1} T^{n+1}_{k-2} + b_{k-1} T^{n+1}_{k-1} + c_{k-1}T^{n+1}_{k} = R_{k-1}^{n+1}
$$
>
> Then, we could get
> 
> $$
 \begin{aligned}
    LHS = &
    -\frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k}l_{k-1}}\kappa_{k-1}T^{n+1}_{k-2}
+\left(1+ \frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k-1}}\left(\kappa_k/l_k 
+\kappa_{k-1}/l_{k-1} \right)\right)T^{n+1}_{k-1}
-\frac{\Delta t}{2\Delta z^2}\frac{1}{r_{k-1}l_{k}}\kappa_{k}T^{n+1}_{k} \\
    = &\frac{\Delta t}{2\Delta z} \left[ 
    \kappa_{k-1}\frac{1}{\Delta z} \left(\frac{1}{r_{k-1}l_{k-1}}T_{k-1}^{n+1} - \frac{1}{r_kl_{k-1}} T^{n+1}_{k-1}\right)\right.
     - \left.
    \kappa_k\frac{1}{\Delta z} \left(\frac{1}{r_{k-1}l_k}T_k^{n+1} - \frac{1}{r_{k-1}l_k}T_{k-1}^{n+1}\right) 
    \right] 
    + T^{n+1}_{k-1}
\end{aligned}
$$
>
> and 
> 
>$$
\begin{aligned}
    RHS = & T_{k-1}^n + \Delta t (\alpha F_{k-1}^{n} + (1-\alpha)F_{k-1}^{n-1})- \frac{1}{2}\Delta t \frac{\partial}{\partial z}\left(\kappa\frac{\partial T_{k-1}^n}{\partial z}\right)\\
\end{aligned}
$$
>
>Thus, the updated theta could be updated solving the inverse matrix $\mathbf{A}$.

## Original code
### scalars_transport

Simplify derivative in one direction and the interested variable is written as $c_j^i$, in which i represent time step and j represent spatial location.

Input variable: $u_*^{i-1}$, $v_*^{i-1}$, $w_*^{i-1}$, $c_*^{i-1}$

**u_big** : detail of to_big function is elaborated in [here](FFT.md)


**const** : $\frac{1}{Nx_2\times Ny_2}$ Definition find in this FFT [markdown note](FFT.md)


**RHS_big** : $\frac{1}{Nx_2\times Ny_2}$

**RHS_T** : <span style='color:navy'> Might be the advection term</span> <tag question>

**pi_x** : $\kappa\frac{\partial T}{\partial x}$

**div_pi** : $\frac{\partial }{\partial x}\left(\kappa\frac{\partial T}{\partial x}\right) + \frac{\partial }{\partial y }\left(\kappa\frac{\partial T}{\partial y}\right) + \frac{\partial }{\partial z}\left(\kappa\frac{\partial T}{\partial z}\right)$

$$
c^i_* = c^{i-1}_* + dt \left(\alpha_1  \left[\right]+ \alpha_2 \right)
$$