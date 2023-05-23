#### Feb 06
- *Summary:*
    1. 
* *Path forward:*
    1. [x] Finish the LESGO tutorial.
    2. [x] Derive the adjoint of the three-dimensional advection-diffusion equation (and show in slides).
    3. [ ] Modify the LESGO code in the following ways:
        * Record and read velocity fields for scalar computation.
        * Run multiple scalars at the same time.
        * A flag for forward/adjoint simulation.
    4. [ ] Visualization for the scalar field (a movie from Paraview would be preferred)

#### Feb 20
- *Summary:*
  - 
* *Targets*
  1. [x] Forward Simulation ( Based on Qi 2019, IC )
  2. [x] Forward Scalar Simulation
  3. [x] Adjoint simulation
  4. [x] Visualization

- *Question:*
  1. [ ] what is the website for the April conference and the abstract format?
  2. [ ] How to solve the boundary problem in lesgo.

#### Feb 27, 2023
* *Targets*
  1. [ ] read forcing.f90, inflow_cond
  2. [ ] check inflow
  3. [ ] check sponge/fringe [Sponge Zone Reading](https://web.stanford.edu/group/ctr/ResBriefs/2010/11_mani.pdf)
  4. [ ] prontal number too large -> scalar oscillates

#### March 6, 2023
1. [ ] Run forward and backward simulation with same parabolic velocity field and compare the result