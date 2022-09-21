# Geometry Optimisation

## Exercise 1 - H'_2_' dimer

In this tutorial we will model the bond length of a H2 molecule. This will give you the opportunity to get used to 
geometry optimisations and gain familiarity 

Remember you can use `castep --help` to assist you with finding the appropriate input parameters.

  
  
               * Set your initial H-H distance to 1 Angstrom.
               * Choose an appropriate plane-wave energy cut-off (cut_off_energy in the param file).
  * What k-point sampling do you need for this computation?
               * This is a geometry optimisation, choose an appropriate task (in the param file).

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
  A: You may wish to test this by changing the size of the box!
  
  Q:

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
  %block 
  ```

Variable size cell

So far you've used the local density approximation (LDA) for the exchange-correlation functional in this exercise. Repeat your calculation with the PBE exchange-correlation functional (a popular GGA):

* ``xc_functional : PBE``(in the param file)
* As CASTEP builds its own pseudopotential on-the-fly you do not need to change the pseudopotential in the cell file.



How does this change your results?

The experimental H2 bond length is about 0.74 Angstroms and the vibrational frequency is about 4395 1/cm.

## Exercise 2

Run a geometry optimisation on silicon.

You can use a silicon input file from one of the previous tutorials as a starting point for your input files.

Set CASTEP's parameters to perform a geometry optimisation using a 160 eV plane-wave cut-off energy and an 8x8x8 Monkhorst-Pack k-point grid:

```
* task : geometry optimisation
* cut_off_energy : 160 eV (in the param file; you will need to remove any existing @@basis_precision@@ setting)
* kpoints_MP_grid 8 8 8 (in the cell file)
```

Because you're going to change the lattice vectors, CASTEP will do a finite basis-set correction (FBSC); this will calculate and print out dEtotal/dlog(Ecut) – anything more than 0.1 eV/atom is big and a sign of incomplete convergence.
What is the final lattice parameter?

Do convergence tests (cut-off-energy, kpoints etc). The experimental lattice constant is 3.84 Angstrom - how does your value compare? What is the difference in your results between calculations using an LDA and a PBE exchange-correlation functional?

## Exercise 3 - Graphite

Perform geometry optimisations on graphite using LDA and PBE.  Again, you can use a previous input file as a starting point.

* What is going wrong?
* How can you use cell constraints to fix it?

## Exercise 4 - Extensions
Experiment with the above structures, and see how you can improve the convergence rate by tweaking the various parameters discussed in lectures. You might also want to experiment with ionic constraints and/or alternative minimisers to BFGS and/or external pressure.

Try some systems you are interested in! Play with the functionality and see what you can compute.

