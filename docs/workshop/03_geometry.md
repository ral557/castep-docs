# Geometry Optimisation


## Exercise 1 - Dihydrogen dimer

In this tutorial we will model the bond length of a H2 molecule. This will give you the opportunity to get used to 
geometry optimisations and gain familiarity 

Remember you can use `castep --help` to assist you with finding the appropriate input parameters.

0. Create a new file, `H2.cell` using your favourite text editor, e.g.

  ```
  nano H2.cell
  ```
  
1. Start by creating a lattice block in your cell file.

  ```
  %block lattice_abc
  5 5 5 
  90 90 90
  %endblock lattice_abc
  ```

or
  ```
  %block lattice_cart
  5 0 0
  0 5 0
  0 0 5
  %endblock lattice_cart
  ```
  
Will produce a cube 5 Angstroms long on each side.

**NOTE: ONLY USE ONE OF THESE TWO FORMATS**

  Q: Is this a big enough box to represent a molecule? (Recall Periodic Boundary Conditions)
  A: You may wish to test this by changing the size of the box.

2. Add atoms into our cell
  This can be done either relative to the lattice vectors using

  ```
  %block positions_frac
  H 0.5 0.5 0.5
  H 0.5 0.5 0.2
  %endblock positions_frac
  ``` 
   
  or relative to the origin of the cell file
  
  ```
  %block positions_abs
  H 3 3 3 
  H 3 3 4
  %endblock positions_abs
  ```
  
  Q: If you are varying the size of your unit cell for tests, which one will be more convenient?

3. Add final components to unit cell
  
  ```
  fix_all_cell: true
  kpoint_mp_grid: 1 1 1
  ```
  
  Q: Why do we only want to use a single k-point?
    
  Q: Why do we want to fix the lattice parameters rather than letting them relax?
  
4. Now close `H2.cell` and create a `H2.param` file

In `H2.param` add

```
Task: GeometryOptimisation
XC_Functional: LDA
Cut_Off_Energy: 300
```

If you want to get Castep to automatically print out the final structure, you can also add
  ```
  write_cell_structure: true
  ```

You can now use `castepsub` to submit your geometry optimisation!

5. Results Analysis

  Scroll down through the file. Check to see how the forces and bond-length varies over iterations.
  
  For advanced bash users, try `grep "F|max" H2.castep` to extract this from the file.
  
  You may also want to visualise the `H2.cell` and (if you tolda castep to print it) the `H2-out.cell` files as in previous tutorials
  
  Finally, consider watching the optimisation by drag-and-dropping the `H2.geom` file into `Jmol` (VESTA will not animate it)


  The experimental H2 bond length is about 0.74 Angstroms. How does your result compare? Extension A may give some insight into this! 

### Extensions

You may wish to split these between groups and discuss the results

A. Functional Choice

  So far you've used the local density approximation (LDA) for the exchange-correlation functional in this exercise. 
  Repeat your calculation with the `PBE` exchange-correlation functional (a popular GGA):


  * ``xc_functional : PBE``

  You may also want to consider a hybrid functional such as `HSE06` or the meta-GGA `RSCAN`

B. More Precise Structural Optimisations

  You may wish to have more precise structure for certain  calculations such as `NMR` or `Phonons`
  These may be controlled in the `.param` file with
  
  ```
  geom_force_tol: 0.05 eV/ang
  geom_energy_tol: 2e-5 eV
  geom_stress_tol: 0.1 GPa 
  geom_disp_tol: 0.001 ang
  ```
  
  These are the default values; what happens to your final values when you alter them?
  
  **Hint:** You have fixed the lattice - what will stress do?

C. Wavefunction Convergence

  If you have a bad wavefunction you will get bad forces. To demonstrate this try adding
  
  ```
  elec_energy_tol: 0.1
  ```
  
  This overwrites the default of `0.00001 ev` and will make the SCF convergence very fast. What does this do to the geometry optimisation?
  
  **NOTE:** This is not something you want to do in practice! Hopefully working through this example will demonstrate why.


## Exercise 2

  Run a geometry optimisation on silicon.

  You can use a silicon input file from one of the previous tutorials as a starting point for your input files.

  Set CASTEP's parameters to perform a geometry optimisation using a 160 eV plane-wave cut-off energy and an 8x8x8 Monkhorst-Pack k-point grid:
  
  In `Si2.param`:
  
  ```
  task : geometry optimisation
  cut_off_energy : 160 eV 
  ```
    
  In `Si2.cell`:
  
  ```
  kpoints_MP_grid 8 8 8
  ```

  Because you're going to change the lattice vectors, CASTEP will do a finite basis-set correction (FBSC); this will calculate and print out dEtotal/dlog(Ecut) –         anything more than 0.1 eV/atom is big and a sign of incomplete convergence.


  Q: What is the final lattice parameter?


  Do convergence tests (cut_off_energy, kpoints etc). The experimental lattice constant is 3.84 Angstrom - how does your value compare?
  What is the difference in your results between calculations using an LDA and a PBE exchange-correlation functional?

## Exercise 3 - Graphene

  It can be useful to know how to construct a monolayer material, such as graphene.
  This requires the use of cell constraints so that we force a large distance between periodic images
  (and we don't have the system collapse to form graphite!)
  
  In your `cell` file:
  
  ```
  %block cell_constraints
  1 1 0
  3 4 5
  %endblock cell_constraints
  ```
  
  Will force the `a` and `b` lattice parameters to be the same (though free to vary jointly), fix the `c` lattice vector,
  and let all angles relax independently. This is similar to the `%block lattice_abc` structure you saw earlier.
  
  For the mathematically minded, this is taking the limit that the interlayer spacing, controlled by the out-of-plane lattice vector, goes to infinity!
  
  Practically, we cannot actually set the lattice parameter to infinity - try varying it and seeing how it converges with distance.
  
  

