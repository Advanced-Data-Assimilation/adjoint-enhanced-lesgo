
## Implicit Method
The discretization equation of advection-diffusion equation based on finite volume method in Eq. {eq}`eq:wang2019_fvm` {cite}`wangSpatialReconstructionSteady2019a` is not very easy to satisfy the boundary condition in fourier space, so we include only $T^{n+1}$ on the LHS instead of $\delta c$, 

$$
\begin{align}
    \frac{T^{n+1}}{\Delta t} 
    - \frac{1}{2}\underbrace{\frac{1}{V}\left[\frac{1}{Pe}\iint_S (\boldsymbol{n} \cdot \nabla T^{n+1}) \mathrm{d}S\right]}_{{D}_{1}\mathbf{T}^{n+1}}
    = & \frac{1}{\Delta t}c^n - \frac{1}{2} \left( 
        3\underbrace{\frac{1}{V}\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}T) \mathrm{d}S \right]^n}_{\mathbf{F}^n\mathbf{T}^n} 
        - \underbrace{\frac{1}{V}\left[\iint_S (\boldsymbol{u}\cdot\boldsymbol{n}T) \mathrm{d}S \right]^{n-1} }_{\mathbf{F}^{n-1}\mathbf{T}^{n-1}}
        \right) \\
        & + \frac{1}{2}\underbrace{\frac{1}{V}\left[\frac{1}{Pe} \iint_S (\boldsymbol{n}\cdot\nabla T) \mathrm{d}S\right]^n}_{\mathbf{D}_{2}\mathbf{T}^n}
        + \underbrace{\frac{1}{V}\left[\int_V \phi \mathrm{d}V\right]^n}_{\phi^n}
\end{align}
$$(eq:scalar_implicit_fvm_governing)

in which ${D}_{1}$ is the diffusion operator based on Fourier method with modified wave number of finite volume method in x and y direction and finite volume in z direction, ${D}_{2}$ is the diffusion operator based on finite volume method. Take Eq.{eq}`eq:scalar_implicit_fvm_governing` into Fourier space, we have

$$
\begin{equation}
a_{(i,j,k)}\widehat{T}^{n+1}_{(i, j, k-1)}+ b_{(i,j,k)}\delta\widehat{T}^{n+1}_{(i, j, k)} + c_{(i,j,k)}\widehat{T}_{(i, j, k+1)} = \widehat{R}_{(i,j,k)},
\end{equation}
$$(eq:scalar_fvm_implicit_fourier)

in which,

$$
\begin{align}
a_{(i, j, k)} &= \frac{1}{2 Pe} \frac{1}{\mathrm{d}\xi_{k}\mathrm{d}\xi_{kk}},\\
b_{(i, j, k)} &=\frac{1}{\Delta t} - \frac{1}{2Pe}\left(-\kappa_1^2x_i^2 - \kappa_2^2y_j^2 + \frac{1}{\mathrm{d}\xi_{k}(\mathrm{d}\xi_{kk}\mathrm{d}\xi_{kk+1})}\right),\\
c_{(i, j, k)} & = \frac{1}{\mathrm{d}\xi_{k}\mathrm{d}\xi_{kk+1}}.
\end{align}
$$(eq:scalar_fvm_implicit_matrix_component)

Don't need to worry about the aliasing error since the advection term is discretized in explicit way and there is no non-linear term on the LHS.


```{admonition} **Modified Wavenumber**
:label: definition:modified_wavenumber_fvm

For the finite volume method, we have the corresponding modified wave number as

$$
\kappa^* = 2 \frac{|\sin{\frac{1}{2}\kappa \Delta x}|}{\Delta x}
$$

in which $\kappa$ is the wave number for continuous Fourier transform (Details in [here](wavenumber_dft)).
```

```{admonition} Boundaries
Eq.{eq}`eq:scalar_fvm_implicit_fourier` need special care on the lower boundary and top boundary. 

**Lower boundary condition**

At the lower boundary ($k=0$), we have

$$
\begin{align}
\text{Dirichlet BC:} & \quad T_{kk=1} = T_{lbc} \quad \Rightarrow \quad \widehat{T_0} + \widehat{T_1} = \mathcal{F}\left(2T_{lbc}\right),\\
\text{Neumann BC:} & \quad T_0 = T_1 - (\frac{\partial T}{\partial z})_{lbc}\dd \xi_{kk=1}\quad \Rightarrow \quad  -\widehat{T_0} + \widehat{T_1} = \mathcal{F}\left(\left(\frac{\partial T}{\partial z}\right)_{lbc} \dd \xi_{kk=1}\right)
\end{align}
$$

**Upper boundary condition**
At the upper boundary ($k=N_z$), we have


$$
\begin{align}
\text{Dirichlet BC:} & \quad T_{kk=N_z} = T_{ubc} \quad \Rightarrow \quad \widehat{T_{N_z-1}} + \widehat{T_{N_z}} = \mathcal{F}\left(2T_{ubc}\right),\\
\text{Neumann BC:} & \quad T_{N_z} = T_{N_z-1} + (\frac{\partial T}{\partial z})_{ubc}\dd \xi_{kk=N_z}\quad \Rightarrow \quad -\widehat{T_{N_z-1}} + \widehat{T_{N_z}} = \mathcal{F}\left(\left(\frac{\partial T}{\partial z}\right)_{ubc} \dd \xi_{kk=N_z}\right)
\end{align}
$$


```

For the zero wave number, we have

$$
\hat{T}_{(k_i=0, k_j=0, k)} = \frac{1}{N_x N_y} \sum_{i = 0}^{N_x-1} \sum_{j=0}^{N_y - 1} T_{(i, j, k)}  = \overline{T_{k}}.
$$

Thus, the governing equation of zero waver number is

$$
\frac{\overline{T_k}}{\Delta t} - \frac{1}{2 Pe} \frac{1}{\dd \xi_k}\left[ \frac{\overline{T_{k+1}}  - \overline{T_k}}{\dd \xi_{kk+1}} - \frac{\overline{T_k} - \overline{T_{k-1}}}{\dd \xi_{kk}}\right] = 
$$




### Validation