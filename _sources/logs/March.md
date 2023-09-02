#### March 1, 2023
- [x] Check the error of backward simulation
  - [x] BC
  - [x] Prontal
    - [x] Try a smaller Prontal Number
      ```Fortran
        ! Prontal Number
        Kappa_t = 0.0055! Nu_t/Pr_sgs
      ```
      * Set Kaapa_t = 0.008, Pr=0.7
  - [x] velocity field
- [x] How to output as gif from paraview
- [ ] Include multiple scalar

#### March 3, 2023
**Different Kappa**
Forward: 12750609
Backward:12750630, 12750871
* Kappa_t = 0.008 -> still blows up
* Kappa_t = 0.055 -> Still blows up
- [ ] Multiple scalar
  - [ ] How to read and store? Different length
- [ ] Backward Simulation
  - [x] Regenerate a normal source IC
  - [x] check the inflow and outflow boundary of scalar

#### March 5, 2023
* Check Inflow and outflow BC of theta -> [LESGO_TUTORIAL MARKDOWN](notes/../../notes/LESGO.md)
  * LBC and UBC [Question](../notes/lesgo.md#210-scalarf90)
  * No inflow and outflow ??
  * Size of theta is not nx, ny, nz but ld, ny, lbz:nz

Slurm Job:12781977 -- Backward Simulation with the same theta IC of forward
-> Blows up still


Solution:
1. Develop Scalar backward based on the velocity adjoint code.
   1. Is it required to include the BC for this code?
2. Insert Inflow and Outflow Condition for current code.
   1. Check the concentration conservation


#### March 6, 2023
Slurm Job: 12793176 -- Smaller source var = 1e-6
Slurm Job: 12793403 -- Larger source var = 1e-4

- [x] How to check the conservation in Fourier method solver?
  - Fourier's method doesn't check the conservation.

Pr = 0.01
Forward - Slurm Job: 12794154
Backward - Slurm Job: 12794170

Pr = 1, SHA=d4aeab156b100ea99c614eac0853e3c7d02357a4
Forward - Slurm Job:12794442
Backward - Slurm Job:

Run the simulation with same parobalic velocity at each time step
Forward - Slurm Job:
Backward - Slurm Job:


#### March 8, 2023
channel_flow_scalar_dirac -- old version code
channel_flow_scalar_parabolic -- new version code

- Run parabolic velocity field simulation
  - [x] Write python script to generate velocity field
  - [x] Forward : Slurm ID - 12824516
    * Blows up, try dirac source 
      * [x] 8 point source -> Slurm ID - 12825125
      * [x] 1 point source -> 
    * [ ] Run with turbulence velocity IC
      * [x] 8 point source -> Slurm ID - 12825250
      * [x] 1 point source -> Slurm ID - 12825267
  - [ ] Compare : 
- [x] Rerun the dirac delta IC simulation
  - [x] Write python script to generate such IC
  - [ ] Run with parabolic velocity field 
    - [x] 8 points source -> Slurm ID - 12824989
    - [x] 1 point source -> Slurm ID - 12825161


- [ ] Run the old version with 1 point source and sponge layer
- Impose the dirac on a regular theta field

#### March 10, 2023
* Comparison of simulation results can be found [here](scalar_err.md)

- [x] Fix backward velocity IC problem (negative velocity IC)
  - bbb68ea3532edecdff7aa45d1b0d5733f00c8d41
- [ ] Find out why the scalar disappear
  - [ ] Run parabolic velocity forward and backward


#### March 12, 2023
* check backward problem
  * velocity field looks wrong

#### March 13, 2023
* Develop multi-scalar
  * [x] scalars_transport
  * [ ] scalars_deriv -- dTdx
* [ ] Write script to generate theta.IC for multi-scalar

To-Do :
* [ ] dt -> smaller
* [ ] hyperlolic -> large/smaller Pr
  * [x] 0.1 & 10
  * [ ] 0.7 & 1.3


Error: forrtl: severe (174): SIGSEGV, segmentation fault occurred [Link](https://suncat.stanford.edu/wiki/common-errors-and-possible-solutions)
Memory is not enough


#### March 20, 2023
Slurm ID - 13022389
include MPI_SYNC_DOWNUP
--> Still not working


Slurm ID - 13022600
Remove Allocatable of nk
WORKS!!

Slurm ID - 13024133
Read multi-scalar IC


Slurm ID - 13024255
Pr = 0.1

To-Do : 
1. [x] smaller grid 
   1. [x] Need corresponding turbulence IC
2. [x] Original code + Turbulence + Pr = 0.1



Slurm ID - 13027972    
Original code + Turbulence + Pr = 1
--> Blows up.

Slurm ID - 13028042    
Original code + Turbulence + Pr = 0.1
-->Blows up.


So that might be some problem with respect to the diffusion stability criterion. Detail can be found [here](../notes/Diffusion_criterion.md)

Slurm ID - 13031283
Original code + Turbulence + Pr = 0.1 + 0.1 cfl

Slurm ID - 13031356
Multi-scalar + Turbulence + Pr = 1 + 0.1 cfl
-> Not Explode
-> <span style='color:red'>Looks like there are some problems about the communication of ghost cell</span>

Slurm ID - 13031424
Multi-scalar + Turbulence + Pr = 0.1 + 0.1 cfl

Slurm ID - 13031492
Original code + 10k timesteps

Slurm ID - 13047379
Original code + Pr = 1 + 0.1 cfl


#### March 21, 2023
We can find out that the cell Peclet Number is too large and diffusion is not stable under such situation. In order to make the simulation stable, we double the resolution and run the simulation with Peclet Number = 1. See if the simulation is stable.


- [x] Wirte a script to interpolate the turbulent velocity field

- [ ] Run simulation with double resolution
Slurm ID - 13059103
Double resolution with single source + Original Code
-> wrong IC

SLurm ID - 13060739
1k steps + double resolution
-> Still Wrong IC

- [x] <span style='color:red'> Figure out the current order of dims</span>

- [ ] Check Multi-scalar ghost cell communication problem
Slurm ID - 13059812
remove mpi-sync
--> Discontinuous

Slurm ID - 13060258
read lbz:nz
--> Source jumps.

- [ ] <span style='color:red'> Run 1 theta case with scalar code to check if there is discontinous like multi-scalar</span>

- [ ] <span style='color:red'> Run original code with source near the wall, see if the source also jumps</span>


#### March 22, 2023
Slurm ID - 13078025
Try nproc 16

Slurm ID - 13078111
np.asfortranarray

**Order of read and write data**
By default, Fortran writes multi-dimensional arrays in column-major order, which means that the last index of the array changes fastest in the file. For example, if you have a 3-dimensional array a(i,j,k), Fortran will write the data to the file in the order a(1,1,1), a(2,1,1), ..., a(n,1,1), a(1,2,1), a(2,2,1), ..., a(n,m,1), a(1,1,2), a(2,1,2), ..., a(n,1,p), a(1,2,p), ..., a(n,m,p), where n, m, and p are the sizes of the first, second, and third dimensions, respectively.

By default, when writing a multi-dimensional numpy array to a binary file using numpy.tofile(), the array is written to the file in row-major order, which means that the first index of the array changes fastest in the file. For example, if you have a 3-dimensional array a[i,j,k], the data is written to the file in the order a[0,0,0], a[0,0,1], a[0,0,2], ..., a[0,1,0], a[0,1,1], ..., a[1,0,0], a[1,0,1], ..., a[n-1,m-1,p-1], where n, m, and p are the sizes of the first, second, and third dimensions, respectively.


Ok, it sounds ridiculous, but the problem is the post-processing script read in a wrong dimension information.

Slurm ID - 13078275
Correct post.exe with 1 Gaussian source and 10k timesteps.
Check u, v, w and theta IC all correct.
--> explode
<span style='color:red'> This might not explode but too much noise </span>

Slurm ID - 13079386
Single point source
--> Disk quote limit

Slurm ID - 13079486
output every 500 steps

<span style='color:navy;font-weight:bold'>  Current backward simulation requires velocity field at every time step, which might cause disk quote limit problem </span>

#### March 23, 2023

Slurm ID - 13126220
new Gaussian IC
-->explode

Slurm ID - 13126685
Variance = 1e-3, bigger source. Tstep=1k


#### March 24, 2023
Something is wrong about the numpy.reshape(order='F'). Not working properly.

**Slurm ID - 13201065**
New theta IC
-> Wrong source location

**Slurm ID - 13265212**
New theta IC
-> IC seems not symmetric but in python plot it is symmetric

**Slurm ID - 13342126**
Rerun the simulation
-> Still problematic. Maybe better to write a python script to visualize the flow field.


<span style="color:red;font-weight:bold"> Check function XYZ, might be wrong order to call meshgrid. </span>
- [x] Check. Correct. Output array should has the shape of [nx, ny, nz]


**Slurm ID - 13359817**
10k step to get a turbulence restart velocity IC.
-> Output has been stored in tests/input folder.

#### March 25, 2023
- [ ] Write a tutorial of Paraview on cluster
- [ ] Automatically post-process with pvpython script
  - [ ] Write an output subroutine in lesgo to make the output in VTK format
  - [ ] Finish the pvpython script
  - [ ] Setup the pvpython ipykernel
- [ ] Check how fortran control output in MPI
- [ ] Check theta Instability Problem
Probably due to the stability criterion CFL.
  - [ ] Check what is the criterion for algorithm that lesgo used.
  - [ ] 
**Slurm ID - 13684899**
Source Version : a85397092eb2026d82b5fcf22fddf4df0360421d
CFL : 0.0625
Pr : 1
-> Looks like explode


- [ ] Check multi-scalar
**Slurm ID - 13684870**
Multi-scalar, theta.IC.01, --> Check if one scalar, the scalar field will be wrong
-> No problem. Try 2 scalar to check if there is something wrong with multi-scalar.

**Slurm ID - 13794376**
2 scalar
-> Discontinuous

*scalars.f90* read RHS_T at the beginning, check if there are some terms left.

**Slurm ID - 13798626**
 New version : 4d1fd21cf2d232f598ee184b158561d59c69484d
 2 scalar
 -> Still discontinuous

- [ ] Prepare presentation slide



- [ ] VScode seems to be banned on Rockfish. Try to run lesgo and develop on local machine.

  - [ ] Fortran language server is not working on mac.
