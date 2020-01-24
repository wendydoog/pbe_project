
## Summary of Paper 



----
### Paper Informations:

Paper title: [Reproducing the Ensemble Average Polar Solvation Energy of a Protein from a Single Structure: Gaussian-Based Smooth Dielectric Function for Macromolecular Modeling](http://szhao.people.ua.edu/uploads/9/0/8/4/90844682/jctc18.pdf)

Paper Anthors: `Arghya Chakravorty`, `Zhe Jia`, `Lin Li`, `Shan Zhao`, and `Emil Alexov`.

-----

### Description:


This paper mainly discussed the ways to reproducing  `the Ensemble Average Polar Solvation Energy` from `a Single Structure`, which can be a `Energy Minimized Structure` in `Explicit Water`, `Implicit Solvent` and in `Vacuum` environments. Or it can be a `Crystal` structure.

*****

So here is some questions that we might seek answers from this paper:

####  What's the Ensemble Average Polar Solvation Energy? And how we calculate it in a traditional way?

We've already learnt that calculating Solvation Energy by Implicit Solvent Model is more efficient than Explicit Model. And these implicit approaches, such as `Poisson−Boltzmann(PB)` or `Generalized Born(GB) approach`, were shown to deliver almost identical polar solvation energy as the expensive Explicit approaches such as `thermodynamic integration(TI)` or `free energy perturbation (FEP)` when the macromolecule was kept rigid (single solute conformation). 

However, macromolecules are `not rigid objects`, and they undergo small or large conformational changes while being transferred from one medium to another. Disregarding this can cause severe errors in predictions of solvation free energies.

A popular approach to compute the average solvation energy:  

- First, carry out a molecular dynamics (MD)/Monte Carlo(MC) simulation;
- then, obtain a representative ensemble of structures (snapshots); 
- then, perform TI, FEP, or Bennett Acceptance Ratio (BAR)calculations on each of the snapshots (while keeping each of them rigid).

`Drawbacks`: extremely demanding of computational time and resources since a typical ensemble may consist hundreds or thousands of snapshots.

***

####  Any alternative way to reproducing the Ensemble Avarage?
- Traditional two-dielectric PB Model.

  `Drawbacks`: The traditional two-dielectric PB calculations cannot mimic these conformational changes within an ensemble because it uses homogeneous dielectric constant for macromolecule and water phase.
  
- Gaussian-based smooth dielectric Model.
  
  `Advantages`: This model delivers a smooth and heterogeneous dielectric distribution in the solvated system which accounts not only for the conformational flexibility of the solute but also the differential dielectric properties of the solvent phase in the proximity of solute. 
  
***

####  How to conduct experiment to test the ability of the Gaussian-based smooth dielectric function to mimic the conformational flexibility?

In this paper, the authors have carried out an extensive investigation of the ability of the Gaussian-based smooth dielectric function to mimic the conformational flexibility.

 1. `Data preprocessing`: To obtain a data set of reasonable size that can be managed in parallel with extensive MD simulations,  they used a set of protocols to screen out suitable proteins from the `Protein Data Bank` (PDB).

- the resolution of the structures was limited to between 0.8 and 0.99 Å with at most 200 amino acids.
- the structures should be monomeric.
- The proteins retrieved were required to have at most 30% sequence similarity.
- the structures did not contain a ligand or modified residue.

Based on all the requirements, 74 proteins have been selected from the screening.

 2. `Energy Minimization in Explicit Water, Implicit Solvent, and in Vacuo Environments`:
 
 The explicit water solvated systems were subjected to 10,000 steps of steepest descent (SD) energy minimization. Two other minimizations, involving only the protein structures, were performed in in vacuo and Generalized Born Implicit Solvent (GBIS) environments. Only 5000 SD steps were used since the system size was drastically smaller than the explicit water systems.  For GBIS minimization, the external dielectric was set at 80.0 (emulating water environment) and that for in vacuo was 1.0.

 3. `MD Simulations`: Post energy minimization, the explicit water solvated systems were subjected to three independent MD simulations. More details of the setting of MD Simulation, please check the original paper.
 
 4.  `Ensemble Average Polar Solvation Energy via PB-Based Calculations vs Alchemical MD Methods`:

  When keep structures `rigid`, the author compared the polar component of the solvation free energy of 19 proteins using TI-MD and using traditional 2-dielectric PB Model. And the result shows that PB calculations with Delphi (with protein internal dielectric = 1 and solvent dielectric = 80) can deliver (<img src="https://render.githubusercontent.com/render/math?math=\Delta G^{Sol}_{pol}">) that would otherwise require a much longer TI-MD runs. Therefore, by using Delphi to calculate Polar Solvation Energy for each “snapshot”, the ensemble polar solvation energy was calculated in a manageable time.

  
  <p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image1.png" width="512" height="512">
  </p>
  
  
  5.`Polar Solvation Energy of Energy-Minimized Structures`:
  
  In the rest of the manuscript, all the PB calculations performed using the traditional 2-dielectric method will carry a label “TRAD-x” and that for the Gaussian-based smooth dielectric method will carry a label “GAUSS-x”. The “x” in these
labels indicate the protein internal dielectric constant. For instance, “TRAD-1” and “GAUSS-1” will identify as the corresponding methods with protein internal dielectric set at 1.

 6.`Comparisons`:
 
 The Polar Solvation Energy of the protein structures obtained after minimization in three different environments in vacuo, Generalized Born Implicit Solvent (GBIS), and explicit water (TIP3P) were compared with the ensemble average polar solvation energy obtained by using MD simulations. For the sake of completeness, the crystal structure of the proteins was also subjected to this comparison.
 
  <p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image2.png" width="700" height="700">
  </p>
 
 Here is some results we get from above pictures:
 
- A `negative` difference implies ⟨ΔG⟩ < ΔG(EM), depicting that the ensemble average is more negative than the polar
solvation energy of the EM structure. In terms of `magnitudes`, the EM structure ΔG is smaller than the ⟨ΔG⟩ (`underestimation`). And vice versa.
- Gaussian-based (`GAUSS`) and traditional dielectric (`TRAD`) models were used with the optimized crystal and EM structures to compare with ⟨ΔG⟩. For the former, values of 1, 2, 4, and 8 were used as internal reference dielectric constant. For the latter, only a single value (=1) was used because values `larger than 1` resulted in highly `underestimated` ΔG with
respect to the ensemble averaged ⟨ΔG⟩.
- The traditional dielectric model (`TRAD-1`) has a very similar degree of agreement with the ⟨ΔG⟩ when paired with the optimized `crystal` structure (Figure 2a), and structures optimized in `solvent`(Figure 2c, d).
- `In Vacuo Energy-Minimized Structure Paired with Gaussian-Based Smooth Dielectric Distribution Can Best Reproduce the Ensemble`.
- The Gaussian-based dielectric model reveals a better agreement with the ensemble ⟨ΔG⟩.
- Quantitatively, the mean relative unsigned error varies depending on the ε value used for a particular Gaussian based
model, but there is always a case for all of the optimization environments where the mean relative unsigned error `≈5%`.
- using the same value for GAUSS’s `ε_ref` and TRAD’s `ε_in` yields a more `negative` value for the former.

***

####  what structural factors that mostly influence the ability of the two dielectric models to reproduce ensemble from a single structure？


  
  
 
 



  
  
