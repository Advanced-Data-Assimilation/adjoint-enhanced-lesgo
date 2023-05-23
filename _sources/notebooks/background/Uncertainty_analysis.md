# Uncertainty analysis
This file is created to record how the uncertainty analysis works.

## Mathematical Background
$$
\log{c^\dag_i(\boldsymbol{x}_s)} \sim GP(\overline{\log{c^\dag_i}}, \mathbf{K}_i(\boldsymbol{x}_{s1}, \boldsymbol{x}_{s2})), \qquad i = 1, 2, \dots, N.
$$


## Principal Components Analysis

$$
\log{c^\dag_i} \approx \overline{\log{c^\dag_i}} + \sum_{l}\alpha_l \phi_l(\boldsymbol{x})
$$