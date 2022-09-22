# Phonons and Spectroscopy Tutorial

  The aims of this tutorial session is to introduce you to running larger-scale CASTEP jobs on supercomputer clusters, editing of input files for and setup of CASTEP     jobs, and analysis of the results using standalone tools.  To achieve results without waiting too long for jobs to complete the initial runs will be small ones, but    the aim by the end of this afternoon should be to set up larger and more significant runs.   Today's session will entirely comprise insulators or semiconductors.
  You may need to consult the CASTEP phonon users guide, which may be accessed [here](http://www.tcm.phy.cam.ac.uk/castep/Phonons_Guide/Castep_Phonons.html)
  The practical will be conducted using the arc service. Make a directory called (e.g.) Phonons and copy the files `/home/jryates/WORKSHOP/h-BN.cell` and   `/home/jryates/WORKSHOP/h-BN.param` into the new folder.

## A. G-Point phonon in h-BN

The first exercise will take you through the construction of a G point phonon calculation and the generation of a simple model infrared spectrum of a semiconductor.    It will also introduce you to the use of some additional analysis and visualisation tools.

BN is one of a family of Nitride semiconductors, which occurs in cubic zincblende (c-BN), hexagonal wurtzite (h-BN) and graphite-like hexagonal (h-BN) polymorphs.  We will calculate the phonons, infrared spectrum and Raman spectrum of h-BN.

1. Phonon Calculation Set-up

  Your starting point will be the structure which is provided in the .cell file.  The [Phonons Manual](http://www.tcm.phy.cam.ac.uk/castep/Phonons_Guide/Castep_Phonons.html) contains a very similar example for the Wurtzite-structure polymorph of BN.
  Edit the .cell file using one of the text editors installed on the system (e.g. nano).  Uncomment the species_pot block and change it to read

  ```
  %block species_pot
  NCP
  %endblock species_pot
  ```

  to select norm-conserving pseudopotentials.

  Choose the k-point set to use – **remove** any `block KPOINT_LIST` and replace with the line
  ```
  kpoint_mp_grid 7 7 4
  ```
  and one to specify a gamma-point phonon wavevector
  ```
  phonon_kpoint_mp_grid 1 1 1
  ```

  Have a look in the .param file, look up the meaning of any keywords you don’t know (you can use `castep.serial -h <keyword>`). Test your configuration using
  ```
  castep.serial –dryrun h-BN
  ```

  This tells Castep to read the intput files and summaries the calculation in the .castep file, but does not run the electronic structure calculation. It is very   useful to check syntax before submitting to a batch queue (and finding out later you’d made a spelling mistake!).
  Check to see if any .err files are produced and fix issues, if any.
If / when no errors are returned, congratulations.  You have set up the input files, and you can delete the dryrun .castep file.
Once you have in place the `<seed>.cell` and `<seed>.param` files you are ready to submit the CASTEP job.  This is done using our general script:

```
castepsub  -n 16 h-BN
```

which requests a 16-core parallel run. Use the `squeue -u $USER` command to monitor the progress of your calculation. When it has finished, you can examine the output file `h-BN.castep` and find the frequencies.  What you see is explained further in the Phonons user guide linked above. There is also a machine-readable file `h-BN.phonon` which contains the frequencies and also the eigenvectors which we will analyse.

2. Analysis of h-BN phonon output.

  We will use Castep’s tools to visualise the modes using the free Jmol visualiser.  
  Use secure file transfer to copy the .phonon file back to the PC where Jmol is installed.  Start jmol and bring up a console window from the right-mouse menu. To use   the full power of the command-line and find the files it is most convenient to set Jmol's current directory to the location of your working directory containing your   output files. (It is possible and simpler to drag and drop the .phonon file onto Jmol window, but you need the power of the command line to display additional
  periodic repeats.)
  ```
  cd
  --- report the current directory
  cd .. or cd <subdir> -- navigate up/down the directory tree.
  ```
  Then type the Jmol command to load your .castep file
 ```
 load h-BN.phonon {3 3 2} PACKED
 ```
  Note that while you can read in the .phonon file from Jmol's File menu, you need the additional opitions of the command line to  display additional periodic repeats.
  You can then use the `Tools-Vibrate` menu to turn on mode animation, and navigate the modes.  Can you see from the mode eigenvectors which modes are IR-active and  which are Raman active? Do you agree with the IR and Raman activity printed in the .castep file?

### Generation of IR spectrum
  The easiest way to generate a simple model IR spectrum is to use Castep’s `dos.pl` tool.  To run this in the most effective way and automatically display a plot, you   will need to be running an X windows server on the PC.  In that case the command
  ```
  dos.pl -ir -xg h-BN.phonon
  ```
  will generate a plot script and use the `xmgrace` plotting program to display it.  You can create a `GNUPLOT` script instead of xmgrace by changing the `-xg` flag to `-gp`.  Alternatively you can generate a GNUPLOT script without plotting by
  ```
  dos.pl -ir -gp -np h-BN.phonon > h-BN-phonon.plt
  ```
  You can then copy the `h-BN-phonon.plt` back to the PC and read this into GNUPLOT.

### Generation of Raman spectrum
  The calculation of a raman intensities is fairly expensive compared to infrared matrix elements and it is therefore not turned on by default however the `.param` file turned this on:
```
CALCULATE_RAMAN : TRUE
RAMAN_METHOD    : DFPT
```
You can use the `-raman` flag of `dos.pl` to generate and plot a Raman spectrum.

## B. Molecular modes in benzene

  The next part of this practical is to compute the modes and spectrum of a molecule and compare the result with a calculation of a molecular crystal.  Our example is  benzene. You are supplied (in the workshop direxctory on arc, `/home/jryates/WORKSHOP`) with pdb files describing a benzene molecule and a high-pressure crystalline polymorph, phase III.

  First run the isolated molecule calculation.  Using the supplied PDB format file and using the `pdb2cell` utility, generate a .cell file for a single molecule calculation.  There are a few other considerations to take into account for an isolated molecule calculation.

  1. The size of the simulation cell governs the interactions between periodic copies of the molecule and should be large enough that these are negligible.

  2. The shape of the simulation cell governs the crystallographic point groups allowed in the handling of the symmetry.  It should be chosen to be commensurate (as far as possible) with the molecular point group to maximise the use of symmetry.  In the case of benzene it should obviously be hexagonal, here we use a box 8A by 8A by 4A.
  3. Recall that there is no electronic dispersion for a molecule, so only a single electronic k-point is needed.  The general rule is that all “molecule in a box” calculations  should use the G point only as Castep uses special performance optimisations in this case.
  4. Use the norm-conserving pseudopotentials (`NCP` in the `species_pot` block)

For simplicity use the local density approximation, `LDA`.

In benzene not all atoms are on special symmetry sites so you should first perform a geometry optimisation, keeping the unit cell fixed (see `fix_all_cell` keyword in Castep’s help). Then set up a follow-on calculation to compute the Gamma point phonons using the `continuation` keyword in the `.param` file.

The Phonons User Guide linked above should help you fill in the details, but please ask if you are stuck.

Check the phonon frequencies. How many zero frequency modes are there? What frequencies do you get? Can you explain?

Benzene Phase III: The next stage is to compute the gamma point phonon modes of the molecular crystal of benzene in the high pressure polymorph, Phase III. You are supplied with a .pdb file.   You should use a 2x2x2 grid of electronic k-points, as dispersion is non-zero in this molecular crystal. Make sure that symmetry is detected and enabled using `symmetry_generate` in the `.cell` file. Also check that the unit cell parameters are fixed to optimise the geometry and NCP pseudopotentials are used. Use the same .param file as for the molecular case to ensure the settings are the same. Similar to the molecular case run a geometry optimisation followed by a phonon calculation.

Once these calculations have completed you should generate a phonon DOS and IR spectra as in the previous practical and compare the molecule with the molecular crystal. You can also use Jmol to identify the modes.

Are all your frequencies positive?  If not, can you suggest why not?

## C. Phonon dispersion using interpolation in NaH

From G point only calculations we now explore the whole of the Brillouin zone of phonon wavevectors.  Our example is the rocksalt-structured hydride, NaH which should run quickly enough to return results in a few minutes.  Based on previous exercises, lectures and the user manual, you should be able to set up and run a DFPT phonon dispersion calculation and display a well converged set of dispersion curves.
A starting point for your `.cell` file is:

```
%block lattice_cart
2.0 2.0 0.0
2.0 0.0 2.0
0.0 2.0 2.0
%endblock lattice_cart

%block positions_frac
Li 0.0 0.0 0.0
H  0.5 0.5 0.5
%endblock positions_frac

Symmetry_generate
```

This contains the primitive fcc unit cell of the B1 rocksalt structure with the Na ion at (0,0,0) and the H ion at (1/2,1/2,1/2). This is a high-symmetry structure so it is important to instruct CASTEP to generate and use the full symmetry set.
Also use norm-conserving pseudopotentials.

A suitable list of points for the fine q-point path for an FCC crystal is:

```
%block phonon_fine_kpoint_path
0.0 0.0 0.0   ! Gamma
0.5 0.5 0.0   ! X (along Delta
1.0 1.0 1.0   ! Gamma (Sigma )
0.5 0.5 0.5   ! L (Delta)
0.5 0.75 0.25 ! W (Q)
0.5 0.5 0.0   ! X (Z)
%endblock phonon_fine_kpoint_path
```

Although you can now start the phonon calculation, knowing the accuracy of the result (error bounds) is good science. You should perform a k-point convergence test.

For a phonon calculation, convergence of the forces is an appropriate test criterion. Since all of the ions are on symmetry positions the forces are zero by symmetry.  Try to think of a way around this obstacle. And adopt a suitable compromise between accuracy and run-time.

## C.II Phonon DOS using interpolation

The task here is to use the NaH example to compute and display not a dispersion curve but a density of states.  This will exploit CASTEPs interpolation functionality, and you will be able to compute a good DOS without the need the repeat the expensive electronic structure calculation.  To do this you will need to set up a calculation neatly identical to your previous one but with two differences.

  1. The calculation should be set up as a continuation.  If your previous run wrote a .check file named “NaH.check” for example, then the param file should contain the line
   ```
   continuation : NaH.check
   ```
  2. Instead of a %block phonon_fine_kpoint_list in the .cell file, you can specify a grid
   ```
   phonon_fine_kpoint_mp_grid 16 16 16
   ```

You can run CASTEP on just 1-4 processors for this
You can try this several times with different fine q-point grids.
This will produce a  `.castep` and `.phonon` file as before.   You may analyse the .phonon file and generate a DOS using the `dos.pl` script
    ```
    dos.pl -xg NaH-dos.phonon
    ```
(again, an X server running on the PC will be needed for grace to display; MobaXterm should have this enabled by default) .
