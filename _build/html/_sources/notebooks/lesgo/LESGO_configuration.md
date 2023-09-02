# LESGO configuration
This file is created to describe how to set up the configuration file properly for specific problem.

## Domain

```
! Domain parameters
DOMAIN {

  ! Specify the number of processors to use (this is mainly to add
  ! compliance with other preprocessing programs)
  nproc = 24

  Nx = 256
  Ny = 256
  ! Total grid size (stored as nz_tot)
  Nz = 120

  ! Dimensional length scale [m]:
  z_i = 1.0

  ! Non-dimensional domain length
  Lx = 12.566370614359172
  Ly = 6.283185307179586
  Lz = 1.0
  ! Specify whether to enforce uniform grid spacing.
  ! If .true. Ly and Lz will be determined to enforce dx=dy=dz based on
  ! Ny and Nz.
  ! NOTE: uniform spacing is required when using level set
  uniform_spacing = .false.

}
```

*nproc* : Number of processors for MPI.
*Nx* : Number of grids in x direction. <span style='color:navy'> Left and right?</span>
*Ny* : Number of grids in y direction.
*Nz* : Number of grids in z direction. <span style='color:navy'> lbz?</span><span style='color:red'> What is the function in post-process to get the z coord</span>
```fortran
 do k = 1, nzbox
    OutArray(:, :, k) = (tanh((dz * real(k - 1, real_p)-real(1, real_p)) * str_factor)/tanh(str_factor) + real(1,real_p))*1.0
 end do
 ```
*z_i* : <span style='color:red'> dimensional length scale </span>
```fortran {hide}
! main.f90 line:158
  if (use_cfl_dt) then

      dt_f = dt
      dt = get_cfl_dt()
      dt_dim = dt * z_i / u_star

      tadv1 = 1._rprec + 0.5_rprec * dt / dt_f
      tadv2 = 1._rprec - tadv1

  end if
```
*Lx, Ly, Lz* : Non-dimensional domain length
*uniform_spacing* : uniform grid spacing. -> Ly, Lz depend on Lx and Nx.

## Model parameters

```
! Model parameters
MODEL {

  ! Model type: 1->Smagorinsky; 2->Dynamic; 3->Scale dependent
  !             4->Lagrangian scale-sim   5-> Lagragian scale-dep
  sgs_model = 1
  ! Wall damping exponent for Mason model (default 2)
  wall_damp_exp = 2

  ! Timesteps between dynamic Cs updates
  cs_count = 5

  ! When to start dynamic Cs calculations
  dyn_init = 100

  ! Co used in the Mason model for Smagorisky coefficient
  Co = 0.16

  ! Test filter type: 1->cut off 2->Gaussian 3->Top-hat
  ifilter = 1

  ! Dimensional velocity scale (friction velocity) [m/s]
  ! u_star is used if coriolis_forcing=.FALSE. and ug is used if
  ! coriolis_forcing=.TRUE.
  u_star = 1.0

  ! von Karman constant
  vonk = 0.4

  ! Coriolis forcing
  ! coriol -> non-dimensional coriolis parameter
  ! ug -> horizontal geostrophic velocity
  ! vg -> transverse geostrophic velocity
  coriolis_forcing = .false.
  coriol = 0.0001
  ug = 1.0
  vg = 0.0

  ! Viscosity models
  sgs = .false.
  molec = .true.

  ! Dimensional molecular viscosity [m^2/s]
  nu_molec = 0.00556
}

```
*sgs_model* : Subgrid Model type. Detial in [here](./subgrid-scale_model.md). 
*wall_damp_exp* : Wall damping exponent for Mason model <span style='color:red'> ???</span>
<span style='color:red'>
*cs_count* : Timesteps between dynamic Cs updates
*dyn_init* : When to start dynamic Cs calculations
*Co* : used in the Mason model for Smagorisky coefficient
*ifilter* : Test filter type: 1->cut off 2->Gaussian 3->Top-hat
</span>
*u_star*: friction velocity $u_\tau =\sqrt{ \frac{\tau_{w}}{\rho}}$. <span style='color:red'> Why is 1?</span>
*vonk* : von Karman constant
*coriolis_forcing* : Coriolis forcing
*coriol* : non-dimensional coriolis parameter<span style='color:red'> ???</span>
*ug* : horizontal geostrophic velocity
*vg* : transverse geostrophic velocity
*sgs* : SGS viscosity models
*molec* : <span style='color:red'> what difference is these two? LES and DNS? Should it be turbulence viscosity?</span>
*nu_molec* : Dimensional molecular viscosity

## Time
```

TIME {

  ! Number of time steps to run simulation
  nsteps = 10000

  ! Specify the allowed runtime in seconds; simulation will exit if exceeded.
  ! This will only account for time loop, not initialization or finalization.
  ! To disable set < 0
  runtime = -1

  ! Specify CFL based dynamic time stepping (.true.)
  use_cfl_dt = .true.
  ! only used if use_cfl_dt=.true.
  cfl = 0.0625

  ! Set static time step
  ! only used if use_cfl_dt=.false.
  dt = 2.0e-4

  ! Use cumulative time across multiple simulations
  cumulative_time = .false.

}
```
*nsteps* : number of time steps
*runtime* : allowed runtime in seconds, diabled when < 0
*use_cfl_dt* : CFL based dynamic time stepping
*cfl* : value of CFL, only used when use_cfl_dt=.true.
*dt* : static time step, only used when use_cfl_dt=.false.
*cumulative_time* : cumulative time across multiple simulations <span style='color:red'> ???</span>


## Solver Parameters
```
! Solver parameters
FLOW_COND {

  ! Lower boundary condition:
  ! 0 - stress free, 1 - DNS wall, 2 - equilibrium wall model, 3 - integral wall model
  ! NOTE: the upper boundary condition is implicitly stress free
  lbc_mom = 1
  ubc_mom = 0

  ! Prescribe bottom and top wall streamwise velocity
  ! Only for DNS (sgs=.false.) and full channel (lbc_mom = ubc_mom = 1)
  ubot = 0.0
  utop = 0.0

  ! Lower boundary condition, roughness length (non-dimensional)
  zo = 0.0001

  ! Use forced inflow
  inflow = .false.
  ! If inflow is true the following should be set:
  ! position of right end of fringe region, as a fraction of L_x
  fringe_region_end = 1.0
  ! length of fringe region as a fraction of L_x
  fringe_region_len = 0.125
  ! Specify uniform inflow velocity (only used if USE_CPS=no in Makefile.in)
  inflow_velocity = 8.0

  ! HIT Inflow
  ! Fluctuation u^prime of the dataset (JHTDB)
  up_in = 0.681

  ! Turbulence intensity desired in the inflow
  TI_out = 0.1

  ! Dimensions of HIT box (non-dimensional using z_i)
  Lx_HIT = 1.
  Ly_HIT = 1.
  Lz_HIT = 1.

  ! Number of grid points in data
  Nx_HIT = 32
  Ny_HIT = 32
  Nz_HIT = 32

  ! Streamwise velocity file
  u_file = './HITData/binary_uFiltered_nx_32_ny_32_nz_32'
  v_file = './HITData/binary_vFiltered_nx_32_ny_32_nz_32'
  w_file = './HITData/binary_wFiltered_nx_32_ny_32_nz_32'

  ! Use mean pressure forcing
  use_mean_p_force = .true.
  ! Evalute mean pressure force. This will compute it as 1/Lz
  ! It may be good idea to put .true. if uniform_spacing = .true.
  ! If .true. the setting for mean_p_force will be overridden.
  eval_mean_p_force = .true.
  ! Specify mean pressure forcing (Typically 1/Lz)
  ! non-dimensional
  mean_p_force_x = 1.0
  mean_p_forec_y = 0.0

  ! Use random forcing
  use_random_force = .false.
  ! if true, specify how many time steps until random forcing stops
  stop_random_force = 20000
  ! if true, specify the rms magnitude of the random forcing
  rms_random_force = 0.4
}
```
*lbc_mom, ubc_mom* : Lower boundary condition and upper boundary condition. 0 - stress free, 1 - DNS wall, 2 - equilibrium wall model, 3 - integral wall model.
*ubot, utop* : bottom and top wall streamwise velocity (u).
*zo* : Lower boundary condition, roughness length <span style='color:red'>???</span>
*inflow* : forced inflow

**SCALAR SPONGE LAYER**

## Output Parameters

```
! Output parameters
OUTPUT {

  ! Specify how often to display simulation update
  wbase = 100

  ! Specify of often to write KE to check_ke.out
  nenergy = 100

  ! Specify how often to display Lagrangian CFL condition of dynamic SGS
  ! models
  lag_cfl_count = 1000

  ! Turn on checkpointing restart data at intermediate time steps
  checkpoint_data = .false.
  ! Number of time steps to skip between checkpoints
  checkpoint_nskip = 10000

  ! Turn on time averaging
  ! records time-averaged data to files ./output/*_avg.dat
  tavg_calc = .false.
  ! Set when to start time averaging (based on global simulation time step)
  tavg_nstart = 50000
  ! Set when to stop time averaging
  tavg_nend = 9000000
  ! Set number of iterations to skip between samples
  tavg_nskip = 100

  ! Turn on instantaneous recording at specified points
  point_calc = .false.
  ! Set when to start recording
  point_nstart = 1
  ! Set when to stop recording
  point_nend = 1000000
  ! Set number of iterations to skip between recordings
  point_nskip = 10
  ! Specify location of points
  point_loc = 0.1, 0.1, 0.1 // 0.5, 0.5, 0.5 // 0.8, 0.8, 0.1

  ! Turn on instantaneous recording in entire domain
  domain_calc = .false.
  ! Set when to start recording
  domain_nstart = 50000
  ! Set when to stop recording
  domain_nend = 1000000
  ! Set number of iterations to skip between recordings
  domain_nskip = 500

  ! Turn on instantaneous recording at specified x-planes
  xplane_calc = .false.
  ! Set when to start recording
  xplane_nstart = 1
  ! Set when to stop recording
  xplane_nend = 1000000
  ! Set number of iterations to skip between recordings
  xplane_nskip = 10
  ! Specify location of planes
  xplane_loc = 0.1, 0.2, 0.3

  ! Turn on instantaneous recording at specified y-planes
  yplane_calc = .false.
  ! Set when to start recording
  yplane_nstart = 1
  ! Set when to stop recording
  yplane_nend = 1000000
  ! Set number of iterations to skip between recordings
  yplane_nskip = 10
  ! Specify location of planes
  yplane_loc = 0.1, 0.2, 0.3

  ! Turn on instantaneous recording at specified z-planes
  zplane_calc = .false.
  ! Set when to start recording
  zplane_nstart = 1
  ! Set when to stop recording
  zplane_nend = 1000000
  ! Set number of iterations to skip between recordings
  zplane_nskip = 10
  ! Specify location of planes
  zplane_loc = 0.1, 0.2, 0.3

}
```
## Level Set
```
LEVEL_SET {

  ! Compute global CA (normalized force time area) based on inflow velocity
  global_CA_calc = .true.
  ! Number of time steps to skip between global CA writes
  global_CA_nskip = 10

  ! Forcing velocity to specified level set BC value
  ! Requires use_log_profile and/or use_enforce_un
  ! (default .false.)
  vel_BC = .false.

  ! Specify handling of level set boundary conditions and treatment.
  ! If unsure please use default values as they are the safest.
  ! (default = .false.)
  use_log_profile = .false.
  ! (default = .false.)
  use_enforce_un = .false.
  ! (default = .true.)
  physBC = .true.
  ! (default = .true.)
  use_smooth_tau = .true.
  ! (default = .false.)
  use_extrap_tau_log = .false.
  ! (default = .true.)
  use_extrap_tau_simple = .true.
  ! Only works w/interp_tau; not MPI compliant
  ! wont work w/extra_tau_log
  ! (default = .false.)
  use_modify_dutdn = .false.

  ! Enables scale dependent Cs evaluations (not dynamic evaluation)
  ! Used only when sgs_model = 4
  lag_dyn_modify_beta = .true.

  ! Configures the mode in which SOR smoothing is applied in the IB
  ! 'xy' may be safely used in most cases (must be used for MPI cases)
  ! '3d' not MPI compliant
  smooth_mode = 'xy'

  ! Surface roughness used for level_set surfaces (non-dimensional)
  zo_level_set = 0.0001

  ! Use the trees_pre_ls functionality
  use_trees = .true.
}
```
## Turbines
```
TURBINES {

  ! Number of turbines in the x- and y-directions
  num_x = 1
  num_y = 1

  ! Placement: (all evenly spaced)
  !  1 = aligned
  !  2 = horizontally staggered
  !  3 = vertically staggered by rows (+/- stag_perc%)
  !  4 = vertically staggered checkerboard (+/- stag_perc%)
  !  5 = horizontally staggered, shifted forward for CPS simulations
  !      note: setting stag_prec to 0 will create aligned array
  orientation = 1
  stag_perc = 50

  ! Turbine dimensions, baseline diameter/height/thickness [meters]
  dia_all = 100
  height_all = 100
  thk_all = 1

  ! Direction turbine is pointing
  !  theta1 is angle CCW (from above) from -x dir [degrees]
  !  theta2 is angle above horizontal
  theta1_all = 0
  theta2_all = 0

  ! Thrust coefficient (Ct')
  Ct_prime = 1.33

  ! Read all turbine parameters above from input_turbines/param.dat
  !   This file is comma separated with each turbine on a line with the
  !   following values for each turbine:
  !     xloc [meters], yloc [meters], height [meters], dia [meters], thk [meters],
  !     theta1 [degrees], theta2 [degrees], Ct_prime [-]
  !   The number of lines must equal num_x*num_y
  read_param = .false.

  ! Specify turbine direction and thrust coefficient dynamically. This will ignore the
  ! values specified above or in input_turbines/param.dat.
  !   If true, then these values are interpolated from the comma separated files:
  !     input_turbines/theta1.dat
  !     input_turbines/theta2.dat
  !     input_turbines/Ct_prime.dat
  !   Each line is a time point (dimensional time) and must have num_x*num_y entries
  !   per line. Dynamic changes are interpolated between each time point.
  dyn_theta1 = .false.
  dyn_theta2 = .false.
  dyn_Ct_prime = .false.

  ! Time scale for one-sided exponential filtering of u_d signal [seconds]
  !   T_avg_dim <= 0 will provide no filtering.
  T_avg_dim = -1

  ! Filtering operation, Gaussian
  !  alpha1 is the filter size as a multiple of the grid spacing in the normal direction
  !  alpha2 is the filter size as a multiple of the grid spacing in the radial direction
  !  filter_cufoff sets the threshold for the unnormalized indicator function.
  !    For a well-resolved turbine, the unnormalized indicator function will be near unity.
  !    Only values above the threshold will used.
  alpha1 = 1.5
  alpha2 = 1.5
  filter_cutoff = 1e-2

  ! Correct ADM for filtered indicator function
  ! For more information see Shapiro et al. (2019) https://arxiv.org/abs/1901.10056
  adm_correction = .true.

  ! The number of timesteps between the output for the turbines
  tbase = 20

}
```