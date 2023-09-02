# Scalar

This part is to describe the scalar module in LESGO.

- [Scalar](#scalar)
  - [Governing Equation](#governing-equation)
  - [Available Solver](#available-solver)
  - [Monitoring Parameters](#monitoring-parameters)


## Governing Equation



## Available Solver
So far, we have multiple version of scalar solvers and there description and details are elaborated in following documents.

1. [Spectral Method + Explicit Time Advancement](./scalar_spectrum_explicit.md)
1. [Finite Volume Method + Explicit Time Advancement](./scalar_fvm.md)
1. [Finite Volume Method + Implicit Time Advancement](./scalar_fvm_implicit.md)

## Monitoring Parameters

In order to observe the behavior of solver, I write a log file at each output time step to record some significant variables in the scalar field. Detail can be found [here](./scalar_log.md)