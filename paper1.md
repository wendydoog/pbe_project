
## Summary of Paper 



----
### Paper Informations:

Paper title: [Reproducing the Ensemble Average Polar Solvation Energy of a Protein from a Single Structure: Gaussian-Based Smooth Dielectric Function for Macromolecular Modeling](http://szhao.people.ua.edu/uploads/9/0/8/4/90844682/jctc18.pdf)

Paper Anthors: `Arghya Chakravorty`, `Zhe Jia`, `Lin Li`, `Shan Zhao`, and `Emil Alexov`.

-----

### Description:


This paper mainly discussed the ways to reproducing  `the Ensemble Average Polar Solvation Energy`
(<img src="https://render.githubusercontent.com/render/math?math=\Delta G^{Sol}_{pol}">) from `a Single Structure`, which can be 
a `Energy Minimized Structure` in `Explicit Water`, `Implicit Solvent` and in `Vacuum` environments. Or it can be a `Crystal` structure.

*****

So here is some questions that we might seek answers from this paper:

1. What's the Ensemble Average Polar Solvation Energy? And how we calculate it in a traditional way?

We've already learnt that calculating Solvation Energy by Implicit Solvent Model is more efficient than Explicit Model. And these implicit approaches, such as `Poisson−Boltzmann(PB)` or `Generalized Born(GB) approach`, were shown to deliver almost identical polar solvation energy as the expensive Explicit approaches such as `thermodynamic integration(TI)` or `free energy perturbation (FEP)` when the macromolecule was kept rigid (single solute conformation). 

However, macromolecules are `not rigid objects`, and they undergo small or large conformational changes while being transferred from one medium to another. Disregarding this can cause severe errors in predictions of solvation free energies.

A popular approach to compute the average solvation energy:  
- First, carry out a molecular dynamics (MD)/Monte Carlo(MC) simulation;
- then, obtain a representative ensemble of structures (snapshots); 
- then, perform TI, FEP, or Bennett Acceptance Ratio (BAR)calculations on each of the snapshots (while keeping each of them rigid).

`Drawbacks`: extremely demanding of computational time and resources since a typical ensemble may consist hundreds or thousands of snapshots.

2. Any alternative way to reproducing the Ensemble Avarage?
- Traditional two-dielectric PB Model.

  Drawback:

