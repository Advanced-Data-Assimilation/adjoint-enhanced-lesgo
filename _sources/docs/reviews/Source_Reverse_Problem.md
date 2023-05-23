# Literatures review on Source Reverse Problem
This file is created to answer following questions.

1. What method has been used to solve the reverse problem?
2. What is the challenges for each solution?
3. Simple test case setup for our new method.


## Stochastic sampling algorithms 
1. [Source Inversion for Contaminant Plume Dispersion in Urban Environments Using Building-Resolving Simulations](http://journals.ametsoc.org/doi/10.1175/2007JAMC1733.1)

### Methods
> “Using Bayesian inference via stochastic sampling algorithms with a high-resolution computational fluid dynamics model, an atmospheric release event can be reconstructed to determine the plume source and release rate based on point measurements of concentration” (Chow et al., 2008, p. 1553) 
> 
> “The stochastic methodology here is general and can be used for time-varying release rates and reactive flow conditions.” (Chow et al., 2008, p. 1553) 
>


> “We use an MCMC procedure with the MetropolisHastings algorithm to obtain the posterior distribution of the source term parameters given the concentration measurements at sensor locations (Gelman et al. 2003; Gilks et al. 1996).” ([Chow et al., 2008, p. 1555](zotero://select/library/items/MJC2RPP4)) ([pdf](zotero://open-pdf/library/items/F9X88UAX?page=3&annotation=EK3T22BW))

### Challenges
> “To lower the computational cost, we limit the prior distribution to the ground surface (thus ignoring the possibility of elevated sources).” ([Chow et al., 2008, p. 1555](zotero://select/library/items/MJC2RPP4)) ([pdf](zotero://open-pdf/library/items/F9X88UAX?page=3&annotation=ME855D8Z))
>

> “For the current applications, we have simplified the situation for demonstration purposes by considering only steady-state flow conditions. (The chosen methodology remains completely general and can handle unsteady and reactive flows.)” ([Chow et al., 2008, p. 1556](zotero://select/library/items/MJC2RPP4)) ([pdf](zotero://open-pdf/library/items/F9X88UAX?page=4&annotation=WJGJUKMH))

### Isolated building example

> “We have developed an event reconstruction prototype for a flow around an isolated building (a cube) with a source located upwind from the building (see Fig. 1). Four sensors are placed in a diamond-shaped array in the lee of the building. Data at the sensor locations are collected using a forward simulation from the true source location. The data are thus “synthetic” and used in this case only to test the inversion algorithm. Artificial measurement error with a standard lognormal distribution is also added to the synthetic data (in this case with a mean of 0 and a standard deviation of rel 0.05).” ([Chow et al., 2008, p. 1557](zotero://select/library/items/MJC2RPP4)) ([pdf](zotero://open-pdf/library/items/F9X88UAX?page=5&annotation=P3M3XY8U))