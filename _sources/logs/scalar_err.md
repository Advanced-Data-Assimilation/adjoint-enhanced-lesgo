# Scalar 
This file is created to record the problem we encounter about the scalar simulation.

- [Scalar](#scalar)
- [LESGO of LATEST VERSION](#lesgo-of-latest-version)
  - [Parabolic Velocity - Forward](#parabolic-velocity---forward)
    - [Slurm ID - 12824516](#slurm-id---12824516)
    - [Slurm ID - 12825125](#slurm-id---12825125)
  - [Turbulence Velocity - Forward](#turbulence-velocity---forward)
    - [Slurm ID - 12825250](#slurm-id---12825250)
    - [Slurm ID - 12825267](#slurm-id---12825267)
- [LESGO of ORIGINAL VERSION](#lesgo-of-original-version)
  - [Parabolic Velocity](#parabolic-velocity)
    - [Slurm ID - 12824989](#slurm-id---12824989)
    - [Slurm ID - 12825161](#slurm-id---12825161)





# LESGO of LATEST VERSION
GIT SHA: 8399a1b29608032b7cdc5bcd098b50ed4cfec87b

## Parabolic Velocity - Forward
### Slurm ID - 12824516
* Forward simulation (velocity forward is commented)
* Code : channel-flow-scalar 
* Theta IC: Gaussian
* Velocity Field: Parabolic

![Velocity Profile](imgs/12824516.png)
![Theta Profile](imgs/theta.gif)
* Time Step : 0 : 100 : 5
### Slurm ID - 12825125
* Forward simulation (velocity forward is commented)
* Code : channel-flow-scalar 
* Theta IC: 8 point source
* Velocity Field: Parabolic


![Theta Profile](imgs/12825125.gif)
* Time Step : 0 : 20 : 1

## Turbulence Velocity - Forward
### Slurm ID - 12825250
* Forward simulation
* Code : channel-flow-scalar 
* Theta IC: 8 point source
* Velocity Field: Turbulence

![Theta Profile](imgs/12825250.gif)
* Time Step : 1 : 100 : 5
* Scalar: [0.0001, 1]

![Theta Profile](imgs/12825250_1_1000.gif)
* Time Step : 1 : 1000 : 50
* Scalar: [0.001, 1]

### Slurm ID - 12825267
* Forward simulation
* Code : channel-flow-scalar 
* Theta IC: 1 point source
* Velocity Field: Turbulence


![Theta Profile](imgs/12825267.gif)
* Time Step : 1 : 100 : 5
* Plot: [0.0001, 1]
* Range: [0.00009999680332839489, 0.23749186098575592]

![Theta Profile](imgs/12825267_1_1000.gif)
* Time Step : 1 : 1000 : 50
* Plot: [0.001, 1]
* Range: 



# LESGO of ORIGINAL VERSION
GIT SHA: d4aeab156b100ea99c614eac0853e3c7d02357a4
## Parabolic Velocity
### Slurm ID - 12824989
* Forward simulation
* Code : channel-flow-scalar 
* Theta IC: 8 point source
* Velocity Field: Parabolic

![Theta Profile](imgs/12824989.gif)
* Time Step : 1 : 20 : 1
* Scalar: [-Infinity, 0]


### Slurm ID - 12825161
* Forward simulation
* Code : channel-flow-scalar 
* Theta IC: 1 point source
* Velocity Field: Parabolic

![Theta Profile](imgs/12825161.gif)
* Time Step : 10 : 500 : 20
* Scalar: [-Infinity, 9.676083890913122e+30]