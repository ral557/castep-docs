!! Learning Objectives
* Gain awareness and familiarity with the tools available to produce band structure and density of states plots with CASTEP.
* Investigate how band structures differ for metallic and semiconductor systems differ.
* Running spin polarised calculations in CASTEP.

!! Note
This tutorial we will be running on Arcus, you should be able to complete this tutorial without copying files backwards and forwards; however if you wish to visualise your system (e.g. using VESTA) then you will need to copy files to your local machine to do so.

If you don't remember how to do this, remind yourself by referring to [[ http://www.castep.org/Tutorials/00-unix |Tutorial 0 ]] and [[ http://www.castep.org/Tutorials/01-BasicsAndBonding |Tutorial 1 ]].

!! Xterminals, Plotting and other computational hassle
To save some effort transferring files backwards and forwards to and from Arcus we are going to use Arcus to plot the band structure and DOS and send the image to your monitor. 


!! Introduction
The aim of this tutorial is to enable you to compute band structures using CASTEP and introduce you to a few of the tools which allow you to visualise the band structure and density of electronic states computed using CASTEP.  The band structure of metals and semiconductors will be plotted so that you can look at the differences between different types of systems.

First you will look at the CASTEP input files (.cell and .param) used to produce band structure information. 

Next you will use the perl script dispersion.pl which can be used to take CASTEP output and plot band structure diagrams.

Then you will use orbitals2bands. A tool which provides more information about the orbitals that contribute to different bands in your band structure diagram.

Finally you will look at the band structure of iron, to show how magnetic systems can be studied using CASTEP, you will also plot the density of states of iron using dos.pl.

!! You will need:
In addition to CASTEP and the suite of tools it comes with you will need:
* [[ http://plasma-gate.weizmann.ac.il/Grace/ | grace ]] - A 2D plotting program.
* [[http://www.perl.org | perl ]] - a scripting language.

These are available on the Arcus cluster.

!! Example files:
Download the input files\\
@@ wget http://www.castep.org/files/bandstructure.tgz@@\\


Then untar and unzip it using:\\
@@ gunzip bandstructure.tgz @@\\
@@ tar -xvf bandstructure.tar @@

!! Example 1 - Graphite.
Move into the graphite directory, look at the CASTEP .cell and .param files and notice the differences from the previous single point energy runs.

To the .param file the task (which lets CASTEP know what you want it to do) needs to be changed to:\\
@@task : bandstructure@@\\
The .cell file requires a path through the Brillouin Zone along which you want the bandstructure to be plotted:\\
@@ %BLOCK BS_KPOINT_PATH\\
    0.0000  0.00000 0.00000  ! G\\
    0.0000  0.00000 0.50000  ! A\\
   -0.3333  0.66667 0.50000  ! H\\
   -0.3333  0.66667 0.00000  ! K\\
    0.0000  0.00000 0.00000  ! G\\
    0.0000  0.50000 0.00000  ! M\\
    0.0000  0.50000 0.50000  ! L\\
   -0.3333  0.66667 0.50000  ! H\\
%ENDBLOCK BS_KPOINT_PATH @@

Run CASTEP using:\\
@@ $ castepsub -n 16 graphite @@

Once the CASTEP calculation has finished a @@graphite.bands@@ file will be present in the directory.  A band structure plot can be viewed by using the dispersion.pl tool.
@@ $ dispersion.pl -xg -bs -symmetry hexagonal graphite.bands@@\\
The -xg option tells dispersion.pl that you are using grace to plot the band structure, the -bs option tells the script that you want to plot using CASTEP output files, the -symmetry hexagonal option labels the high symmetry points on the bands structure plot.

When you view this band structure plot you will notice the bands are coloured from lowest to highest energy. Using information about the wavefunction CASTEP can improve this band structure plot, so bands are coloured due to the orbitals that contribute. The orbitals2bands tool can be used to alter the Si.bands file represent the orbitals that contribute to the bands.  This tool can be run in the same directory that you ran CASTEP in.\\
The program orbitals2bands overwrites your Si.bands file, so it's best to copy it to another file to preserve it\\
@@ $ cp graphite.bands graphite.bands.orig @@\\
@@ $ orbitals2bandssub -n 16 graphite @@\\

orbitals2bandssub is a wrapper, similar to castepsub to run orbitals2bands on the compute nodes on arcus-b.


!! Example 2 - Silicon and Aluminium.
Very similar to example 1, but this time comparing a semiconductor and a metal, both with FCC crystal structures.

Go into the silicon and aluminium directories and compute the band structures as above.

The dispersion symmetry option needs to be told that these are FCC materials, not hexagonal.

Compare the band structure of the Silicon and Aluminium crystals.  
* What are the main similarities and differences?
* Can you explain this using your knowledge of the bonding in these materials?

!! Example 3 - Iron

In the iron directory there is a set of input files for iron. As iron is a magnetic system you need to instruct CASTEP that it is spin polarised.  You also need to set up the calculation with the total spin set to a non-zero value, in order to find the magnetic ground state. (If you're interested you could try removing the spin : 1 line from the Fe.param file and see what happens).  You set the spin in the .param file:

@@ spin : 1\\
spin_polarised : true@@

The path through the Brillouin Zone is found in the Fe.cell file.

Run the iron computation using CASTEP and plot the band structure for iron using dispersion.pl.  You might find the -mono option to dispersion.pl to be useful - it colours the bands by spin channel.

!! Iron's Density of States

To plot the density of states of iron, we need to run CASTEP again, instead of computing the band structure along a high symmetry line, we compute it on a grid.  To your Fe.cell file, remove the bs_kpoint_path block and replace it with:

@@ BS_KPOINT_MP_GRID 12 12 12 @@

Run CASTEP using these new input files then use the dos.pl plotting script \\
@@ dos.pl -xg -bs -w 0.2 Fe.bands @@

Can you relate the features in the DOS to those in the Bandstructure?

You might find the @@-mirror@@ option useful. The @@-w@@ option sets the Gaussian broadening in eV. Try smaller (0.05) and larger (0.5) values - explain what you see.

DOS for the other crystals in this tutorial can be computed in a similar way.


!! Further work

a) Compute the band-structure of hexagonal monolayer boron nitride. What is the key difference to graphene?

b) MoS2 has attracted much recent interest. What is its structure? Can you make a cell file for this (if not ask). What about its band structure - metal or insulator, direct vs indirect gap.


