# Fast Fourier Transform 
## Reference 
1. [De-aliasing in Fast Fourier Transform](https://math.jhu.edu/~feilu/notes/DealiasingFFT.pdf)
2. 

## Discrete Fourier Transform (DFT) and FFT


### Fourier Transform (FT)

### Discrete Fourier Transform (DFT)
Given sequential data $\{X_j\}_{j=0}^{N-1}$ of length $N$ at time index $j$, we could convert the time-domain discrete sequence into frequency-domain discrete spectrum:


### Modified Wavenumber
**Reference**
1. [Compact Scheme](https://en.wikipedia.org/wiki/Compact_finite_difference)
1. [Modified Wave Number](https://www.nas.nasa.gov/assets/pdf/ams/2018/introtocfd/Intro2CFD_Lecture2_Lecture3_Pulliam_Chap3_Modk.pdf)



## FFTW
##### to_big(a, a_big)

```fortran {hide}
!*******************************************************************************
subroutine to_big(a, a_big)
!*******************************************************************************
use fft
use param, only : lbz, nx, ny, nz

real(rprec), dimension(ld, ny, lbz:nz), intent(inout) ::  a
real(rprec), dimension(ld_big, ny2, lbz:nz), intent(inout) :: a_big

integer :: jz
real(rprec) :: const

! Set variables onto big domain for multiplication in physical space and
! dealiasing
    const = 1._rprec/(nx*ny)

do jz = lbz, nz
    temp_var(:,:,jz) = const*a(:,:,jz)
        call dfftw_execute_dft_r2c(forw, temp_var(:,:,jz), temp_var(:,:,jz))
        call padd(a_big(:,:,jz), temp_var(:,:,jz))
        call dfftw_execute_dft_c2r(back_big, a_big(:,:,jz), a_big(:,:,jz))
end do

end subroutine to_big
```

#### [dfftw_plan_dft_r2c_2d](https://www.fftw.org/fftw3_doc/Fortran-Examples.html) 
-- generate DFT plans.
#### [dfftw_execute_dft_r2c](https://www.fftw.org/doc/FFTW-Execution-in-Fortran.html) 
-- execute the DFT plan. Fast Fourier Transform. Detail in [document](https://www.fftw.org/doc/Real_002ddata-DFTs.html)

##### *padd* 
```fortran {hide}
!*******************************************************************************
subroutine padd (u_big,u)
!*******************************************************************************
! puts arrays into larger, zero-padded arrays
! automatically zeroes the oddballs
use types, only : rprec
use param, only : ld,ld_big,nx,ny,ny2
implicit none

!  u and u_big are interleaved as complex arrays
real(rprec), dimension(ld,ny), intent(in) :: u
real(rprec), dimension(ld_big,ny2), intent(out) :: u_big

integer :: ny_h, j_s, j_big_s

ny_h = ny/2

! make sure the big array is zeroed!
u_big(:,:) = 0._rprec

! note: split access in an attempt to maintain locality
u_big(:nx,:ny_h) = u(:nx,:ny_h)

! Compute starting j locations for second transfer
j_s = ny_h + 2
j_big_s = ny2 - ny_h + 2

u_big(:nx,j_big_s:ny2) = u(:nx,j_s:ny)

end subroutine padd
```




## Parameteres


```fortran
! Grid size for dealiasing
nx2 = 3 * nx / 2
ny2 = 3 * ny / 2
```

```fortran
! Grid size for FFT's
lh = nx / 2 + 1
ld = 2 * lh
lh_big = nx2 / 2 + 1
ld_big = 2 * lh_big
```

