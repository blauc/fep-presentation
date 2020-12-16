---
title: Free energy pertubation methods overview
tags: Talk, FEP
description: 
slideOptions:
  theme: white
---

<style>
.reveal { font-size: 30px; }
.reveal section img { background:none; border:none; box-shadow:none; }
</style>

# Free energy pertubation methods

Christian Blau - BioExcel

blau@kth.se

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/Bioexcel_symbol.svg)

---

# Content

- Setting the scope
- Crash-course / refresh statistical physics
- 10 free energy pertubation methods and flavours
- Simulation setup considerations

---

# The challenge


----

## Protein mutations / ligands

$$
\begin{eqnarray}
\mathsf{protein + ligand }&\rightleftharpoons&\mathsf{ protein \cdot ligand} \\
\mathsf{protein }&\rightleftharpoons& \mathsf{protein_{mut}}
\end{eqnarray}
$$

----

## Concrete system

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/lush-propanol-ethanol.svg)



----

## Relative binding free energies

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/free-energy-of-binding.svg =600x)

 - ABFE hard, but possible

----

## Relative binding free energies

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/thermodynamic-cycle.svg =600x)


---

# Basic framework

----

## Boltzmann free energies

$$
G=-k_BT\log P
$$

----

## Calculating free energies is counting 
$$
  \Delta G_{12} = G_1 - G_2 = -k_BT\log \frac{P_1}{P_2}
$$

----

## Calculating free energies is counting 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepiscounting0.svg)

----

## Calculating free energies is counting 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepiscounting.svg)

----

## Calculating free energies is counting 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepiscounting2.svg)

----

## Calculating free energies is counting 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepiscounting3.svg)

----


## Calculating free energies is counting 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepiscounting4.svg)

----

## How to "count" chemistry

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-states.svg)

----

## Lambda dynamics

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-lambda-dynamics.svg)


----

## Dynamic protonation / constant pH simulation

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/original.jpg =400x)

###### (c) Max Planck Insitute for Biophysical Chemistry / Plamen Dobrev / Carsten Kutzner 

----

## Moving between lambas - morphing states

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/morph-ethanol-propanol.svg)

----

## Moving between lambas - morphing states

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/morph-ethanol-propanol1.svg)

----

## Moving between lambas - morphing states

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/morph-ethanol-propanol1.svg)


 - a tedious job, use `pmx` in the `GROMACS` universe

----

## The issue with lambda dynamics


![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-lambda-dynamics1.svg)

---

# Energy/Hamiltonian based methods 

----

## Main concept

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-lambda-dynamics1.svg)

 - free energy calculation methods = enhancing sampling

----

## Simple FEP

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-simplefep.svg)

###### Zwanzig, R. W. J. Chem. Phys. 1954, 22, 1420-1426


----

## Simple FEP

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-simplefep.svg)

$$
\Delta G_{AB} = -\frac{1}{\beta}\log\left\langle\exp(-\beta (H_B(\vec{x})-H_A(\vec{x}))\right\rangle_A
$$

###### Zwanzig, R. W. J. Chem. Phys. 1954, 22, 1420-1426

----

## Simple FEP - main issue

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-simple-issues.svg)

$$
\Delta G_{AB} = -\frac{1}{\beta}\log\left\langle\exp(-\beta (H_B(\vec{x})-H_A(\vec{x}))\right\rangle_A
$$

----

## Simple FEP

**Pros**
 - technically the simplest form
 - need to run just one simulation
 - only needs energy calculation
 - no need to generate a starting structure for the B state
 
**Cons**
 - very bad performance
 - no sampling in B state will mean that reasonable configurations will be missed

----

## Stratified FEP



![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-stratification.svg)

----

## Stratified FEP

![states](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-stratifiedfep.svg)

----

## Stratified FEP

 - **Pros**
   - technically still easy to set up
   - only needs energy calculation
 - **Cons**
   - still very bad performance
   - need to generate a starting structure for the intermediate states


----


## Simple BAR

![states](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-simplebar.svg)

 - Bennets Acceptance Ratio
 

----


## Simple BAR


![states](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-simplebar.svg)

$$
\Delta G_{AB} = \frac1\beta \log \cfrac{\sum_{x\in A} f\left[H_A(\vec{x})- H_B(\vec{x})+C\right] / n_A} {\sum_{x\in B} f\left[H_B(\vec{x})- H_A(\vec{x})-C\right] / n_B } +C 
$$

with

$$
C = \Delta G_{AB} - \frac1\beta \log \frac{n_A}{n_B} ; f[x]= \cfrac{1}{1+\exp(\beta x)}
$$


----


## Simple BAR

 - use `gmx bar` or simlar tools
 
 - **Pros**
   - better performance than simple FEP
 - **Cons**
   - Still largely dependent on ensemble overlap


----


## Stratified BAR

![fepmethods](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-stratifiedbar.svg)


----


## Stratified BAR

 - **Pros**
   - combines the benifits of BAR and stratification
 - **Cons**
   - can do even better with same simulation effort (see mBAR)


----


## Multistate BAR

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-mbar.svg)


----


## Multistate BAR

$$
G_{i} = -\beta \log \sum_{j\in\mathrm{simulations}} \sum_{n\in\mathrm{configurations}_j} \cfrac{\exp(- \beta H_k(x^j_n))} {\sum_{k\in\mathrm{simulations}} n_k \exp(G_k - \beta H_k(x^j_n)) }
$$

- use [alchemlyb](https://github.com/alchemistry/alchemlyb) to solve that type of equation


----

## Hamiltonian replica exchange

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-rex.svg)

----

## AWH 

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-awh.svg)


---


# Energy/Hamiltonian-derivative based methods


----


# Thermodynamic integration

![TI](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-ti.svg)


----


## Thermodynamic integration

![TI](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-ti.svg)

$$
\Delta G = \int_{\lambda=0}^1 \left\langle\cfrac{\mathrm{d}H}{\mathrm{d}\lambda}(x)\right\rangle_\lambda \,\mathrm{d}\lambda 
$$


----


## Theromdynamic integration

 - **Cons**
   - computationally expensive
   - has a systematic error for non-infinite sampling times due to the steadily moving $\lambda$ value


----


## Discrete thermodynamic integration

![dti](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-dti.svg)


----


## Discrete thermodynamic integration

 - **Pros**
   - better approximation than slow-growth TI
 - **Cons**
   - need to prepare system at different starting points
   - need some overlap between states between $\lambda$ (tight $\lambda$ spacing) to reasonably approximate the integral that yields the free energy.
   - still bad performance compared to other methods


----


## Jarzynski equation

![jarzinsky](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-jarzynski.svg)


----


## Jarzynski equation


![jarzinsky](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-jarzynski.svg)

$$
W=\int_{\lambda=0}^1 \cfrac{\mathrm{d}H}{\mathrm{d}\lambda}(x)\,\mathrm{d}\lambda
$$

$$
\Delta G = -\frac1\beta \log \left( \frac1N \sum_i \exp(-\beta W_i)\right)
$$

----

## Jarzynski

 - **Pros**
   - good when you don't know what the B-state looks like 
   - some systematic bias for finite number of samples (long-tail distribution)
 - **Cons**
   - Crooks is better in most other cases

----


## Crooks fluctuation theorem

![CFT](https://raw.githubusercontent.com/blauc/fep-presentation/master/fepmethods-cft.svg)


----


## Crooks fluctuation theorem

$$
\log \cfrac{P_f(W)}{P_r(W)} = \beta(W-\Delta G)
$$

---

# Absolute binding free energies

----

# Absolute binding free energies

![](https://pubs.rsc.org/image/article/2016/sc/c5sc02678d/c5sc02678d-f2_hi-res.gif =400x)

###### (C) Matteo Aldeghi "Accurate calculation of the absolute free energy of binding for drug molecules"

---

# Simulation system setup


----

## X-ray to GROMACS

 - `gmx pdb2gmx`
 - Watch out for 
   - terminal residues
   - titration sites

----


## Periodic boundary conditions

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/pbctric.png)

----

## Adding simulation box

- `1.0 nm` distance to periodic image is usually save

----

## Adding simulation box

- rhombic dodecahedron for most efficiency (approx. 15% speed gain)

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/truncoct.png)

----

## Adding simulation box

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Rhombic_dodecahedra.png/720px-Rhombic_dodecahedra.png =400x)

----

## Adding water

 - copy-paste block of water

----

## Adding water

![](https://raw.githubusercontent.com/blauc/fep-presentation/master/cell_water.png =1000x)

----

## Adding ions

- `150mM NaCl`
- randomly swaps water with ions
- ensure ions far enough from ligand / protein
- use `GROMACS2020` or higher
- make sure to not use outdated force-fields 

----

## Setting force-field

 - most popular choices amber, charmm
 - for FEP chose all the force-fields if you can

----

## Setting force-field and simulation parameters

 - match force-field and parameters for 
   - short-range electrostatics/vdW cutoff
   - vdw interaction switch
   - dispersion correction
   - coulomb interaction type (PME, etc)
   - PME approximation: fourier-spacing * rcoulomb 

----

## Energy minimisation

 - steepest descend, usually around 10,000 steps

----

## Enery minimisation

 - typical output
![](https://raw.githubusercontent.com/blauc/fep-presentation/master/epotemin.png =500x) 

----

## Temperature coupling

- keep heavy atoms fixed springs
- use `v-rescale` overall
![](https://raw.githubusercontent.com/blauc/fep-presentation/master/temperature.svg =500x) 


----

## Pressure coupling

- use `Berendsen` first, then `Parinello-Rahman` 
- from `GROMACS2021` on use `stochastic cell rescaling`
![](https://raw.githubusercontent.com/blauc/fep-presentation/master/pressure.svg =500x) 

----

## Setting up efficient equilibrium simulation

- GPUs drastically enhance performance
- Focus on sampling, rather than individual simulation length
- `-multidir` option
- GROMACS performance bottlenecks:
  - clock-rate
  - communication (MD step in `ms`)
  - **not** memory bound

---

# Questions


![](https://raw.githubusercontent.com/blauc/fep-presentation/master/Bioexcel_symbol.svg)
