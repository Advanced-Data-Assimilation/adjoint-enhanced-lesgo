
# Scalar backward check
This file is created to record the backward and forward simulation check.

- [Scalar backward check](#scalar-backward-check)
  - [Turbulence Velocity - Forward](#turbulence-velocity---forward)
    - [Slurm ID - 12842079](#slurm-id---12842079)
    - [Slurm ID - 12842637](#slurm-id---12842637)
  - [Backward](#backward)
    - [Slurm ID - 12842652](#slurm-id---12842652)
    - [Slurm ID - 12855662](#slurm-id---12855662)
- [New LESGO](#new-lesgo)
    - [Slurm ID - 12855023](#slurm-id---12855023)
  - [Turbulence Forward](#turbulence-forward)
    - [Slurm ID - 12854656](#slurm-id---12854656)
  - [Parabolic Velocity](#parabolic-velocity)
      - [Slurm ID - 12866546](#slurm-id---12866546)
      - [Slurm ID - 12867107](#slurm-id---12867107)
      - [Slurm ID - 12867728](#slurm-id---12867728)


## Turbulence Velocity - Forward
SHA: ba76e6592328897dc68dfc646d82947373ec9641
### Slurm ID - 12842079
* Forward simulation
* Case : channel-flow-scalar 
* Theta IC: 1 point source
* Pr = 0.1



### Slurm ID - 12842637
* Forward simulation
* Case : channel-flow-scalar 
* Theta IC: 1 point source
* Pr = 1


## Backward
### Slurm ID - 12842652
* Backward simulation
* Case : channel-flow-backward
* Theta IC: 1 point source
* Velocity Field: Baseflow - 12842652
* Pr = 1

![Theta Profile](imgs/12842652.gif)
* Time Step : 0 : 20 : 1


### Slurm ID - 12855662
bbb68ea3532edecdff7aa45d1b0d5733f00c8d41
* Backward simulation
* Case : channel-flow-backward
* Theta IC: 1 point source
* Velocity Field: Baseflow - 12854656
* Pr = 1

![Theta Profile](imgs/12855662.gif)
* Time Step : 0 : 1000 : 50

# New LESGO



### Slurm ID - 12855023
3e81b5c1a1eb28cce4675bad292b293b7e945ac7
* Forward simulation
* Case : 2a_channel_flow_scalar_parabolic
* Theta IC: 1 point source
* Velocity Field: 
* Pr = 1

![Theta Profile](imgs/12855023.gif)
* Time Step : 0 : 1000 : 50

## Turbulence Forward
### Slurm ID - 12854656
bbb68ea3532edecdff7aa45d1b0d5733f00c8d41
* Forward simulation
* Case : 2_channel_flow_scalar
* Theta IC: 1 point source
* Velocity Field: 
* Pr = 1

![Theta Profile](imgs/12854656.gif)
* Time Step : 0 : 1000 : 50


## Parabolic Velocity
Compare the simulation result with different Prontal Number.
#### Slurm ID - 12866546
* ba4c28b382728410b203efd221224cd0cfe5c01a
* Forward simulation
* Case : 2a_channel_flow_scalar_parabolic
* Theta IC: 1 point source
* Pr = 1


![Theta Profile](imgs/12866546.gif)
* Time Step : 0 : 1000 : 50

#### Slurm ID - 12867107
* 0fd7deb9390312fb055174f53247e3f237c3e036
* Forward simulation
* Case : 2a_channel_flow_scalar_parabolic
* Theta IC: 1 point source
* Pr = 0.1


![Theta Profile](imgs/12867107.gif)
* Time Step : 0 : 1000 : 50


#### Slurm ID - 12867728
* 29251f7e801d939e8a18181e5119c152d3378538
* Forward simulation
* Case : 2a_channel_flow_scalar_parabolic
* Theta IC: 1 point source
* Pr = 10


![Theta Profile](imgs/12867728.gif)
* Time Step : 0 : 1000 : 50