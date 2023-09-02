#### Feb 10, 2023
- Record and read velocity fields for scalar computation
    `output104.f90` write the velocity files.
    `initial.f90` read the ICs.

- Run multiple scalars at the same time.
  * Dimensions change > 

- A flag for forward/adjoint simulation.

- Read velocity data > format: 

#### Feb 20, 2023
Question:
- Purpose of the linearized code, why we need it?
- What buoyancy_force do?
- what is ld & lbz in scalar_init?
  - ld -> Grid size?
  - lbz -> lower boundary z

#### Feb 22, 2023
- [x] hij001.dat file?
      * Wall roughness. Need to take BC into consideration.
- [x] no obs file?
      * automatically create a folder to store the base flow velocity field
- [ ] generate a gif image to show the forward simulation
      * Run the simulation
      * Use the post-process code to generate plt files based on the simulation results
      * plot the flow
- [ ] Write the backforward code
      * Record the velocity field
      * Run the forward Scalar field
      * Backward code
        * Read the base flow velocity
        * Apply the adjoint equation into the CFD
        * Run the simulation
      * plot the backward simulation


#### Feb 24, 2023
- [x] What is the difference between ic_file and ic_les
      * ic_les generate random velocity IC and theta while ic_file read from file. Might need ic_les in future to investigate multiple velocity IC.
- [x] How to replace the velocity u instead of the recorded velocity u_base?
- [ ] Run the scalar forward and record the velocity field or run the scalar forward with the recorded velocity field?
  - [ ] Validates that the rerun velocity is almost the same as the recorded one
- [x] Require dpdx for the input
  - [x] The write_pressure doesn't work properly
  - [x] Problem might be caused by incorrect dims of output
  - [x] Separate the output of pressure gradient from write_obs


#### Feb 25, 2023
- [x] Adjoint
- [ ] Visualization
- [x] Check the recorded pressure gradient (how should the pressure gradient at the boundary be determined?)
  - [x] Why would we need the pressure gradient? For the adjoint equation of velocity?
      * Code for another project.
- [ ] IC of theta only contain nx, ny, nz-1, remove unnecessary data in the IC file
- [x] Should adjoint baseflow obtained from the adjoint_obs? Or directly add a negative sign in the forward_obs code?
      * Negative of the forward velocity.


#### Feb 26, 2023
- [ ] backward simulation blows up, check the error
- [ ] Paraview seperate baseflow and theta
- [ ] backward only has one theta output
- [ ] forward with scalar remove write pressure gradient
- [ ] what is coord?

#### Feb 27, 2023
- [x] output plt file cannot be read
  - [x] output dimension is not correct
      * remove the theta varaible in writeins3d.f90
- [ ] Simulation not turublence
  - [ ] Check the boundary condition
- [ ] one point source theta is too hard to see from the visualization
- [ ] IC of backward should be instantaneous velocity field and scalar field at the last time step
- [ ] variance of the IC should be small -> approximate dirac delta
- [ ] Do continuous source simulation
- [ ] Plot from the IC
- [ ] backward simulation blows up
![](imgs/Feb28.png)