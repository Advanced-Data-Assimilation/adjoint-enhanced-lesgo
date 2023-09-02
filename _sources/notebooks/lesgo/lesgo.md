# Lesgo Tutorial
- [Lesgo Tutorial](#lesgo-tutorial)
  - [1. Domain Setup](#1-domain-setup)
    - [1.1 Parameters](#11-parameters)
    - [1.1 Boundary Conditions](#11-boundary-conditions)
    - [Initial Conditions](#initial-conditions)
  - [2. Code Structure](#2-code-structure)
    - [2.1 main.f90](#21-mainf90)
    - [2.2 fft.f90](#22-fftf90)
    - [2.3 derivatives.f90](#23-derivativesf90)
      - [subroutine ddx(f,dfdx,lbz)](#subroutine-ddxfdfdxlbz)
    - [2.10 scalar.f90](#210-scalarf90)
      - [subroutine to\_big(a, a\_big)](#subroutine-to_biga-a_big)
- [Appendices: Libraries](#appendices-libraries)
  - [A. FFTW](#a-fftw)
    - [A.1 Reference](#a1-reference)
    - [A.2 Parameters](#a2-parameters)
    - [A.3 Functions](#a3-functions)
      - [dfftw\_plan\_dft\_r2c\_2d](#dfftw_plan_dft_r2c_2d)
      - [dfftw\_execute\_dft\_r2c](#dfftw_execute_dft_r2c)
  - [B. OpenMP](#b-openmp)
    - [B.1 Reference](#b1-reference)
    - [B.2 Parameters](#b2-parameters)


## 1. Domain Setup

### 1.1 Parameters
**lbz** -- overlap level for MPI transfer, equal to 0 when MPI is turn on

### 1.1 Boundary Conditions
* Inflow
* Outflow
* Lower
* Upper

### Initial Conditions
* 

## 2. Code Structure

### 2.1 main.f90

### 2.2 fft.f90
- [Dealiasing FFT](https://math.jhu.edu/~feilu/notes/DealiasingFFT.pdf)

### 2.3 derivatives.f90
#### subroutine ddx(f,dfdx,lbz)
This subroutine computes the partial derivative of f with respect to x using spectral decomposition.


### 2.10 scalar.f90
<span style="color:red"> Check the BCs for theta</span>

```Fortran
! <scalars.f90>
! Bottom of domain
if (coord == 0) then
    RHS_big(:,:,1) = const*(u_big(:,:,1)*dTdx_big(:,:,1)                       &
        + v_big(:,:,1)*dTdy_big(:,:,1)                                         &
        + 0.5_rprec*w_big(:,:,2)*dTdz_big(:,:,2))
end if

! Top of domain
if (coord == nproc-1) then
    RHS_big(:,:,nz-1) = const*(u_big(:,:,nz-1)*dTdx_big(:,:,nz-1)              &
        + v_big(:,:,nz-1)*dTdy_big(:,:,nz-1)                                   &
        + 0.5_rprec*w_big(:,:,nz-1)*dTdz_big(:,:,nz-1))
end if
```

<span style="color:red"> Is this correct? Upper BC and lower BC for temperature. What is RHS? </span>

```Fortran
pi_y(:,:,1) = -0.5_rprec*(Kappa_t(:,:,1) + Kappa_t(:,:,2))*dTdy(:,:,1)
```

<span style="color:red"> What is pi?</span>

#### subroutine to_big(a, a_big)
```Fortran
! Set variables onto big domain for multiplication in physical space and
! dealiasing
const = 1._rprec/(nx*ny)
do jz = lbz, nz
    temp_var(:,:,jz) = const*a(:,:,jz)
    call dfftw_execute_dft_r2c(forw, temp_var(:,:,jz), temp_var(:,:,jz))
    call padd(a_big(:,:,jz), temp_var(:,:,jz))
    call dfftw_execute_dft_c2r(back_big, a_big(:,:,jz), a_big(:,:,jz))
end do
```

[dfftw_execute_dft_r2c](#dfftw_execute_dft_r2c)


<span style="color:red"> Why forward on the origin data and then backward on the bigger grids?</span>



# Appendices: Libraries
## A. FFTW
### A.1 Reference
- [FFTW TOC](https://www.fftw.org/fftw3_doc/index.html#SEC_Contents)
### A.2 Parameters

```Fortran
! <input_util.f90>
! Grid size for dealiasing
nx2 = 3 * nx / 2
ny2 = 3 * ny / 2

! Grid size for FFT's
lh = nx / 2 + 1
ld = 2 * lh
lh_big = nx2 / 2 + 1
ld_big = 2 * lh_big
```


### A.3 Functions
#### [dfftw_plan_dft_r2c_2d](https://www.fftw.org/fftw3_doc/Fortran-Examples.html) 
-- generate DFT plans.
#### [dfftw_execute_dft_r2c](https://www.fftw.org/doc/FFTW-Execution-in-Fortran.html) 
-- execute the DFT plan.


## B. OpenMP
Separate the data into different blocks vertically ( along z direction)
```Fortran
! <input_util.f90>
! Set the processor owned vertical grid spacing
nz = floor ( real( nz_tot, rprec ) / nproc ) + 1
```

### B.1 Reference
### B.2 Parameters
