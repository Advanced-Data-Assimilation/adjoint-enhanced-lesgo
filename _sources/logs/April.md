# Log for April
- [Log for April](#log-for-april)
- [Summary](#summary)
  - [Log](#log)
  - [LESGO - Stability *March 25 -*](#lesgo---stability-march-25--)
  - [LESGO - Multi-scalar *March 25 -*](#lesgo---multi-scalar-march-25--)
  - [Post-Process - Paraview automation *March 25 -*](#post-process---paraview-automation-march-25--)
  - [Post-Process - integral](#post-process---integral)
  - [System March 27 -](#system-march-27--)
  - [Simulations - Probablistic Framework](#simulations---probablistic-framework)
  - [LESGO - Sponge Layer](#lesgo---sponge-layer)
  - [Reading - Source reverse problem](#reading---source-reverse-problem)
  - [Launch Script](#launch-script)
  - [python\_util](#python_util)
- [Daily log](#daily-log)
      - [Previous log](#previous-log)
      - [April 3, 2023](#april-3-2023)
      - [April 5, 2023](#april-5-2023)
      - [April 6, 2023](#april-6-2023)
      - [April 8, 2023](#april-8-2023)
      - [April 10, 2023](#april-10-2023)
      - [April 11, 2023](#april-11-2023)
      - [April 12, 2023](#april-12-2023)
      - [April 13, 2023](#april-13-2023)
      - [April 17, 2023](#april-17-2023)
      - [April 29, 2023](#april-29-2023)


# Summary
##  Log
- [ ] Write a tutorial of Paraview on cluster
- [ ] Finish the log of FFT
- [ ] Finish log of Spectral method
- [ ] Finish log of Crank-Nicolson
  - [ ] Find out what is the jacobian parameter
  - [ ] Find out how the tridiagonal matrix is inverse 
  - [ ] What is Von Neumann stability analysis?
- [ ] Figure out how lesgo discretized in z direction.

## LESGO - Stability *March 25 -*
- [ ] Check theta Instability Problem
    * [x] Probably due to the stability criterion CFL. -> turns out that all criterion is within reasonable range.
      * **Slurm ID - 13684899**      -> Looks like explode
          Source Version : a85397092eb2026d82b5fcf22fddf4df0360421d
          CFL : 0.0625
          Pr : 1

  - [ ] Check what is the criterion for algorithm that lesgo used (Spectrum Method).

Dr. Zhu provides a new version of LESGO code which calculate the theta in an implicit way, but it is missing the scalars.f90 file.
- [x] Run the implicit method LESGO.
  - scalars.f90 is missing.
  - Looks like if we want to use implicit method, the spectral method is no longer included anymore.
  - [x] Figure out how should we apply the Crank-Nicolson method. Detail in [here](../notes/Crank-Nicolson.md)
  - [x] Run with Kappa homogeneous

  - [ ] Figure out the meaning of jacobian parameters
- [ ] Plot the contour of yz cross section for scalar
- [ ] Set the Prontal Number Pr = 0.71
- [ ] 


## LESGO - Multi-scalar *March 25 -*
- [x] Check multi-scalar
    * **Slurm ID - 13684870**
        Multi-scalar, theta.IC.01, --> Check if one scalar, the scalar field will be wrong
        -> No problem. Try 2 scalar to check if there is something wrong with multi-scalar.
    * **Slurm ID - 13794376**        -> Discontinuous
        2 scalar

    *scalars.f90* read RHS_T at the beginning, check if there are some terms left.
    * **Slurm ID - 13798626**        -> Still discontinuous
        New version : 4d1fd21cf2d232f598ee184b158561d59c69484d
        2 scalar
 
- [ ] Run simulation with two same theta IC and two different theta IC to compare and see if the code is wrong because missing variable recorded.
  - All zero because dimension is not match! -> wrong output order in write_array_to_file. Fixed.
  - Wrong order. Try another one.
  - Another one.
  - The magnitude of original gaussian pdf generation function also depend on the variance. If variance set to be too small. Might have problem.
  - **Slurm ID - 13919285** : One Scalar
  - 
- [ ] Run simulation with single processor to check if there is problem with ghost cell
- [ ] Plot the contour of cross-section to visualize the jumping error.



## Post-Process - Paraview automation *March 25 -*
- [ ] Automatically post-process with pvpython script
  - [ ] Write an output subroutine in lesgo to make the output in VTK format
    - [x] Draft version *March 28*
    - [x] Include correct x, y, z coordinate
    - **Slurm ID - 13908823** Draft code -> `Name in only-list does not exist or is not accessible. ` --> Need to specify the subroutine name as public to call from outside.
    - **Slurm ID - 13911292** Fix -> ffname2 not defined
    - **Slurm ID - 13912019** 
  - Seems like paraview cannot read vtk directly, but I add another subroutine to output the result directly in tecplot format. However, one might need to be attention about the coordinate difference for uv grid and w grid.
  - [ ] Finish io.write_tecplot_file
    - [ ] Check grid location
  - [x] Finish the pvpython script
  - Now that the pvpython script could work, but we still need to post-process the output binary file before use the pvpython script to visualize the result. Maybe output in VTK format for next step.
  - [ ] Setup the pvpython ipykernel
- [ ] Check how fortran control output in MPI
  - <span style='color:red'>read(19001,rec=coord+1)</span> Is this related?

## Post-Process - integral
- [ ] Integral theta in time and then plot the plume.



## System March 27 -
- [ ] VScode seems to be banned on Rockfish. Try to run lesgo and develop on local machine.
- [ ] fortran language server is not working on macbook.


## Simulations - Probablistic Framework
- [ ] Run a simulation with 1 source 1 sensor and 100 different velocity profile. (could be simplied to 1 source and 100 sensor placed in different x location since the turbulence profile is homogeneous in x direction)

## LESGO - Sponge Layer
- [ ] Add a sponge layer at the beginning and the ending of the simulation domain. (BC)

## Reading - Source reverse problem
- [ ] Read paper of Tina chow, UCB [link](https://chow.ce.berkeley.edu/publications/)
- [ ] Read paper of flow over cube in the channel flow
- [ ] Check if reverse problem has been studied in smooth wall configuration



## Launch Script
- [x] Update new launch script for rockfish. Download source code from github and compile in the scratch folder since if multiple jobs are submitted. There will be some conflicts.
  - **Slurm ID - 13909469** Test --> Fix typo
  - **SLurm ID - 13909471** Test --> Works

- [ ] Update the launch script to copy initial condition of velocity and theta field to the running folder.
  - **Slurm ID - 13979221**  -> Didn't go to the scratch folder before running the simulation.


## python_util
- [ ] update generate_field



# Daily log


#### Previous log
- [x] Run with Kappa homogeneous
    - **Slurm ID - 13889918** -> magnitude of theta seems to be wrong.
    - **Slurm ID - 13920275**-> Not working. Seems like something is wrong with the new launch script.
    - **Slurm ID - 13921607** Old launch script.-> Works. But there is still something wrong about the IC
  - Generate another IC by matlab. Try
    - **Slurm ID - 13921609** -> Not working. Might be related to the copy process is still not finished while the mpirun has already started.
    - **Slurm ID - 13921610** sleep for 3 seconds before the mpirun starts. -> Not working. Exit for some unknown reason.
    - **Slurm ID - 13960852** New IC generated from python. -> Wrong launch script
    - **Slurm ID - 13960886** Old script to test on the IC. -> Works. Check the visualization. -> IC is perfect.
    - **Slurm ID - 13960892** New launch script. -> Still not working.
    - **Slurm ID - 13960903** 1k timestep with one scalar.
    - **Slurm ID - 13963336** Explicit method result.
  - [ ] update the launch script to build from source code clone from github. Test case 2c.

#### April 3, 2023
1. Wall roughness and block influence to the theta profile
2. How the non-dimensionless theta $\theta^\tau$ define
3. Scalar transport and momentum similarity
4. Tina 2003 Review Adjoint - 08 block - 
5. Particle and scalar
6. Cornell Li Qi
7. Boundary Condition Review

#### April 5, 2023
- [x] New theta IC has been updated to inputs/turbulence_256_256_128 folder. Source locates at x=0.2Lx, 0.5Lx and 0.8Lx.
- [x] Find the error of launch script is that didn't go to the scratch directory before starting the simulation.
- [x] Run 10k time steps simulation with CN method as well as explicit method, and compare the result.
  - **Slurm ID - 13980494** 2c - CN
  - **Slurm ID - ** 2b - explicit
- [ ] Finish the subroutine that output the file to tecplot. Or. Read the data directly from binary file through python script. 
  - Directly read from binary file is possible. Detail in this [link](https://docs.paraview.org/en/latest/ReferenceManual/pythonProgrammableFilter.html). Check chapter 5.
- [ ] Run the simulation of multi-scalar code, with one scalar, two same scalar and two different scalar.
  - **Slurm ID - 13980738** 1 scalar -> looks like something wrong with post-processing
  - **Slurm ID - 13981868** 3 scalar


#### April 6, 2023
- Paraivew doesn't support programable filter together with paraview process. So one might still need to generate tecplot file before put data into paraview.
- Work on post-processing script in the folder of **Slurm ID 13998692**
  - [x] Looks like the output of thetas is problematic. Write new output script for multi-scalar.

#### April 8, 2023
**Slurm ID - 14020330** New multi-scalar, 10k time steps.
- [ ] multi-scalar backward
- [x] multi-scalar CN method


#### April 10, 2023
**Slurm ID - 14020330** -> Stable.

**Slurm ID - 14041032** Implicit, 1k time steps. -> Difference is not very significant with explicit method.


- [ ] Run Backward simulation
  - [ ] Record forward velocity
  - [ ] Run backward simulation
**Slurm ID - 14041175** Backward CN, 1k 

- [ ] Run simulation with CFL=0.0625, Pr=0.71 for 10k timestep forward and backward.
- [ ] Summarize thermal boundary layer.
- [ ] Send report summary.

#### April 11, 2023
**Slurm ID - 14048986** Pr = 0.71, 10 time steps and output baseflow.
-> NPROC not consistence.
**Slurm ID - 14049077** Setup up nproc in source code.
-> Still not working. Looks strange since I have already change the nproc in hard code but there is still error appearing.
**Slurm ID - 14049144** Try nproc=16.
-> Wrong baseflow_flag
**Slurm ID - 14049189** Try nproc=64
-> looks like lesgo.conf is also readed when the file is compile.
**Slurm ID - 14050094** new code.
-> Something wrong with the launch script ( didn't specify the nproc ).
**Slurm ID - 14050571** new launch script.
-> success
**Slurm ID - 14052171** backward.
-> Initial Condition of velocity field is wrong. Also, it looks like the boundary velocity is not correct.

- [ ] Check nproc setting in larger simulation requiring more than one nodes.
  - [ ] check when lesgo.conf is read with cmake compiling


#### April 12, 2023
Fix the BC, and rerun
**Slurm ID - 14079216** -> lbc=1 (wall)


- [ ] Run LESGO on Fermi. Cannot create scratch folder.

#### April 13, 2023
**Slurm ID - 14103340** Backward simulation.
-> u_base instead of u_velocity
**Slurm ID - 14103448** New launch script.
-> wrong output timestep
**Slurm ID - 14104343** Output every 100 steps.

Velocity IC is totally wrong. Regenerate one by interpolation and then run 10k to stabilize.
**Slurm ID - 14107081** New Velocity IC.
-> Network issue
**Slurm ID - 14129029** NPROC=16
-> Post Processing not working
**Slurm ID - 14136248** 10k, 3 scalars




#### April 17, 2023
1. Re=395
2. Sponge layer for scalar, both sides
3. forward, continuous source
4. adjoint, time integral

#### April 29, 2023
Try to launch the simulation on fermi but didn't work. Locate the error to divstress_uv.f90. 
-> Seems like the problem is kind of related to FFTW or memory resource. 