!! Computational Details

!! Exercise 1 - H'_2_' dimer

Build H2.cell and H2.param files to compute the bond length of an H'_2_' molecule.

Remember you can use @@castep --help@@ to assist you with finding the appropriate input parameters.

* How big does your box need to be?
* Do you want the size of your box to vary?
* Set your initial H-H distance to 1 Angstrom.
* Set your basis set to FINE (basis_precision in the param file) or choose an appropriate plane-wave energy cut-off (cut_off_energy in the param file).
* What k-point sampling do you need for this computation?
* This is a geometry optimisation, choose an appropriate task (in the param file).

So far you've used the local density approximation (LDA) for the exchange-correlation functional in this exercise. Repeat your calculation with the PBE exchange-correlation functional (a popular GGA):
* @@xc_functional : PBE@@ (in the param file)
* As CASTEP builds its own pseudopotential on-the-fly you do not need to change the pseudopotential in the cell file.



How does this change your results?

The experimental H2 bond length is about 0.74 Angstroms and the vibrational frequency is about 4395 1/cm.

!! Exercise 2

Run a geometry optimisation on silicon.

You can use a silicon input file from one of the previous tutorials as a starting point for your input files.

Set CASTEP's parameters to perform a geometry optimisation using a 160 eV plane-wave cut-off energy and an 8x8x8 Monkhorst-Pack k-point grid:
* @@task : geometry optimisation@@
* @@cut_off_energy : 160 eV@@ (in the param file; you will need to remove any existing @@basis_precision@@ setting)
* @@kpoints_MP_grid 8 8 8@@ (in the cell file)

Because you're going to change the lattice vectors, CASTEP will do a finite basis-set correction (FBSC); this will calculate and print out dEtotal/dlog(Ecut) – anything more than 0.1 eV/atom is big and a sign of incomplete convergence.
What is the final lattice parameter?

Do convergence tests (cut-off-energy, kpoints etc). The experimental lattice constant is 3.84 Angstrom - how does your value compare? What is the difference in your results between calculations using an LDA and a PBE exchange-correlation functional?

!! Exercise 3 - Graphite

Perform geometry optimisations on graphite using LDA and PBE.  Again, you can use a previous input file as a starting point.

* What is going wrong?
* How can you use cell constraints to fix it?

!! Exercise 4 - Extensions
Experiment with the above structures, and see how you can improve the convergence rate by tweaking the various parameters discussed in lectures. You might also want to experiment with ionic constraints and/or alternative minimisers to BFGS and/or external pressure.

Try some systems you are interested in! Play with the functionality and see what you can compute.

