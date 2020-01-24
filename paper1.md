
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

In this paper, authors mainly focus on rendering a method that will incur the least error. So the GAUSS-2 in vacuo EM structure is the primary focus. They compared its outputs with that of the traditional method and elaborate on their
differences.


1. `trying to find what factors really have the influence`. 

 <p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image3.png" width="700" height="550">
  </p>
 
- Differences in `Population of Salt Bridges (SBs)` Directly Affect Differences in `ensemble` using Traditional Method Across Different EM Structures of a Protein.
  * The comparison of the population of salt bridges after minimization shows that the in `vacuo` protein EM structures have a higher number of these than the EM structures from the other two environments(Figure 3a). 
  * This has been further demonstrated by computing the relative number of SBs using the number in the corresponding EM structure from explicit water environment for normalization (Figure 3b).

- examination on how other structural features such as `the structural backbone RMSD` and `intraprotein hydrogen bond network` vary after minimization in different environments.
  * Energy minimization protocol is `not` expected to cause a significant change in a protein’s conformation, especially the backbone conformation, but it may result in different hydrogen positions. To verify, they calculated `the structural RMSD` of the backbone and `number of intraprotein hydrogen bonds` (hydrogen bonds within the atoms of a protein) after minimization, with respect to the corresponding crystal structure.
  * The comparison for structural RMSD is shown in Figure 3c. They noticed that large backbone changes did `not` occur post minimization regardless of the environment. Essentially, the backbone atomic positions were preserved.
  * In the same way, Figure 3d illustrates the comparison for the number of intraprotein hydrogen bonds. It is evident that all of the three environments yielded `similar` numbers after minimization.
  * The above analysis indicates that EM in different environments results is very similar backbone structures and intraprotein hydrogen bonds and therefore cannot be the reason for the differences in the polar solvation energies.
  
  
2. `Seeking to find a quantitative association of the change in polar solvation energy ΔG and the number of SBs formed or lost upon solvation`.
  
  <p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image4.png" width="700" height="300">
  </p>
  
- one can infer from the reasonably high r2 values (0.525 and 0.384 for GBIS and explicit solvent, respectively) that a linear relation is evident. This is a clear indicator of how the solvent can affect the number of SBs and subsequently alter the polar solvation free energy. Moreover, since the ordinate in the plots is the true difference (ΔΔG = ΔG(in vacuo) −
ΔG(in solvent)), a greater loss of the SBs yields a more favorable solvation (ΔG is more negative).

3. `demonstrate how the breaking and forming of SBs in MD simulations is well mimicked by the Gaussian model but not the traditional one`.

- Gaussian-Based Smooth Dielectric Model Reproduces Ensemble ⟨ΔG⟩ as It Can Mimic the Fluctuations of SBs.

<p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image5.png" width="700" height="300">
  </p>

  *  The plot indicates that the error incurred by a dielectric distribution model `deteriorates` as more of the SBs present in the EM structure `break` during the MD. One can notice, from the linear trend in Figure 5a, that this is indeed the case with the traditional model. At the same time, from Figure 5b, the error of the GAUSS-2 method is not only smaller than that of the TRAD-1 method but is independent of the occupancy of the salt bridges.
  * the Gaussian-based dielectric model (GAUSS-2) is able to capture SB fluctuations resulting in smaller errors (than the
TRAD method), and the computed polar solvation energy has `no` dependence on the occupancy of the SBs (r2 = 0.011).
  
  
  
- The `ε_ref` of Gaussian-Based Dielectric Distribution That Best Reproduces the Ensemble Average from a Structure Depends on the Strength of Salt Bridge Interactions in It.

<p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image7.png" width="700" height="600">
  </p>

* Figure 7 shows an example of a salt bridge that fluctuates between open and closed forms in MD simulations; it is
closed/formed (O−N distance = 2.72 Å implying `stronger interaction`, Figure 7a), in the in vacuo-minimized structure but
open/broken (O−N distance = 6.13 Å implying a `weaker interaction`, Figure 7b) in the GBIS-minimized structure of its
host protein.

<p align="center">
  <img src="https://github.com/wendydoog/pbe_project/blob/master/paper1_image8.png" width="400" height="700">
  </p>

* there is a significant difference in how the Gaussian-based dielectric function treats closed versus open salt bridges.
A schematic is shown in Figure 8.


### Conclusion(Quote From the Paper):


> The primary objective was to ascertain if the `Gaussian-based smooth dielectric distribution` (as implemented in DelPhi) for
modeling the dielectric distribution can mimic the natural dynamics of a protein and therefore yield its `ensemble average
polar solvation energy` using a `single` structure alone. The Gaussian-based model, in parallel with the `traditional 2-
dielectric model`, was paired with structures minimized in different environments (in `vacuo`, `GBIS` and `explicit water`) and `crystal structure` of 74 proteins to study its ability to approximate the ensemble ⟨ΔG⟩. Our study shows that
he `traditional dielectric model` is able to reproduce a protein’s ⟨ΔG⟩ only with its `crystal structure` or a `structure minimized in solvent`. However, for most of the proteins, one would have to decrease the dielectric internal dielectric (ε_in) to `below 1`, in order to achieve better approximations. This unreasonable modification can be circumvented by the use of Gaussian-based dielectric model. Not only does it yield a better agreement with the ensemble ⟨ΔG⟩ for physically valid internal dielectric values (known as ε_ref), its performance is appreciable regardless of the minimization environment. In fact, for most of the cases, `Gaussian-based dielectric model performs better than the traditional model`, even if subtly. Upon comparing the overall results, we show and therefore suggest that the use of Gaussian-based dielectric model with `ε_ref = 2`, paired with a protein’s in `vacuo-minimized structure`, is best suited for reproducing its ensemble average polar solvation energy.

> A detailed analysis revealed the reasons for the aforementioned differences in performance and other solvation energy
trends. We found that the `conformational states of SBs` (open/ closed) in a protein’s minimized structure play an important
role in offering one dielectric model an advantage over the other in terms of reproducing its ensemble average polar
solvation energy. This means that a dielectric model that best mimics the flexibility of the SB forming residues from their
configuration in the EM structure is better at reproducing the ensemble average polar solvation free energy. The Gaussian-based dielectric model is shown to accomplish this and therefore is capable of generating ensemble average polar solvation energy of a protein from its in vacuo energy minimized structure. Our findings can henceforth serve as a starting point for developing time-inexpensive single structure MM/PBSA method.





  
  
  


  
  
 
 



  
  
