# LESGO Tutorial 
This markdown note is a tutorial for LESGO. [Video](https://www.youtube.com/watch?v=LtJrqzgVDD4&list=PLKLcs9yV72LxfFIAZh11Qk5eXIegGgEVB&index=7)

## A. Environment Setup
### A.1. Personal PC
Required packages

> gcc-9.3.0/fftw-3.3.8

Reference
1. [FFTW3 Installation Instruction](http://www.fftw.org/fftw2_doc/fftw_6.html)


### A.2. HPC - Rockfish
First, you need to connect to the HPC rockfish with the given username.

```bash
ssh username@login.rockfish.jhu.edu
```

BTW, you could also mount the folder on HPC on local machine.

```bash
sshfs -p 22 username@login.rockfish.jhu.edu:<remote-dir> -0 <local-dir>
```

Once you login, you could easily set up the compile environment through the modules. 
```bash
source /data/apps/go.sh
ml cmake
ml intel/2020.1
ml openmpi/3.1.6
```
Or, you could create a bash file and run it everytime you want to compile LESGO or put it into the `~/.bashrc` file so that it will be run every time you log in.



## B. Nonlinear Forward Obs 
### B.1. Compile
<span style='color:red;font-weight:bold'> What OBS stand for? </span>

First, we need to go to the code folder
```bash
cd $LESGO_DIR/clean_version/lesgo_nonlinear_forward_obs
```

You could setup the hsotname as well as select different features to turn on or off in `CMakeLists.txt`. Before we compile the code, we need to remove previous built files. Usually we could do it simply through `make clean`. However, this is not yet supported in current version so we simply remove the built folder.
```bash
rm -rf bld/
```

Then, we could build the new executable file with the new configuration.
```bash
./build-lesgo
```

Once we got the built executable file `lesgo-mpi`, we could copy it to the folder where we run the simulations.
```bash
cp lesgo-mpi <run-dir>
```

### Run
First, we need to go the the simulation folder.
```bash
cd <run-dir>
```

Then, we need to specify the domain etc. parameters of the simulation to run the LESGO code in `<run-dir>/lesgo.conf`. Before we run the code, we need to check if the input files are also in the running folder.
> hij001.dat -> wall roughness
> [u/v/w]\_velocity.0000 -> initial condition
> scripts_[fermi/rockfish].sh -> launch script


Run LESGO in without MPI.
```bash
./lesgo-mpi
```

Run LESGO with MPI.
```bash
sbatch scripts_rockfish.sh
```

Check the submitted sbatch job status.
```bash
sacct
```

<span style='color:red'> There are a lot of output file that is not mentioned in the tutorial video.</span>


### Post-process
Mount remote HPC folder on local folder.
```bash
sshfs <remote-address>:<remote-dir> <local-dir>
```

<span style='color:red'>Why running matlab is very slow in the mounted folder?</span>


Use the matlab code in `Post_process` folder to plot the result of the velocity field.

![Initial W Velocity](./imgs/init_w.png)
![](./imgs/end_w.png)
![](./imgs/init_u.png)
![](./imgs/end_u.png)

## Nonlinear Forward Scalar
Difference with obs
> coriolis.f90
> pid.f90
> USE_SCALARS scalars.f90 stability.f90

<span style='color:red'>What scalar field represen t for?<span>

Build the code and copy it to the running folder.
```bash
./build-lesgo
cp lesgo-mpi-scalars <run-dir>
```

Run the LESGO with scalar field in series.
```bash
cd <run-dir>
./lesgo-mpi-scalars
```

The output file is `theta.***`.

### Post-process with Paraview
Go to the folder `Post_processing_TECPOT` and open `parame.ter` to modify the location of data, start/end/step of the timestep. Also check the domain is correct in `PARAM.H`.
```bash
cd <lesgo-dir>/Post_processing_TECPLOT
vim parame.ter
vim PARAM.H
```

Build `./post.exe`.
```bash
make clean
make
```

Or 
```bash
bash Makefile_bash
```

Import files.
> read_rstrt.f90
> writeins3d.f90


Run the post-process.
```bash
./post.exe
```

<span style='color:red'>file not found, unit 18, file /home/ext-zyou6474/LESGO/Post_processing_TECPLOT/../run00/theta.00000000</span>

## Linearized Forward
Linearized forward is runned in `run_F` folder while adjoint is in `run_A` foler. And the source code is located in `clean_version` folder.
> clean_version/lesgo_adjoint
> clean_version/lesgo_linearized_forward

Scripts that could validate the linear forward and adjoint code together are located in `<lesgo-dir>/utils/02_linear_forward_adjoint_validation` folder. And one could check the code in `makefile`.

Check the configuration in `run_A` and `run_F` folder.
> Nx, Ny, Nz
> Lx, Ly, Lz
> nproc
> dt
> nu_molec
> nsteps

The initial velocity should also satisfy the boundary condition, which means that one should not use random number as the initial condition.

<span style='color:red'>what is the difference of $ and ! in the code.</span>


## Adjoint of the Linearized Forward


## Validation of the code
### Validation of Adjoint

$$
$$

### Validation of Linearization

$$
||N(u_0 + \epsilon u_0\prime) - N(u_0)||^2 \approx ||L_u \epsilon u_0\prime||^2 = \epsilon^2||u_n\prime||^2\\
\lim_{\epsilon \rightarrow 0} \frac{||N(u_0 + \epsilon u_0\prime) - N(u_0)||^2 }{\epsilon^2} = ||u_n\prime||^2
$$


<span style="color:red"> Question: Why this this valid? </span>


Start the validation by running
```bash
make validation_linearization_loop
```
 <span style="color:red"> Only lesgo-mpi can generate u_velocity data file. Should we add another procedure before we run the lesgo-mpi-DA?</span>

 ```bash
(cd $(folder_true_forward)  && mpirun -np $(np) ./lesgo-mpi-DA > run_history)
 ```

 ## Validation of Adjoint - non-linear Forward