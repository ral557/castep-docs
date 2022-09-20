## Learning Objectives
* Introduction to CASTEP input and output files.
* Running on the Arcus machine at Oxford University.

## Introduction

The aim of this exercise is to familiarise you with CASTEP input and output files and running the code, some associated utilities and conversion programs. You will run some simple and small CASTEP calculations on canonical examples of covalently and ionically bonded materials - silicon and sodium chloride - and use the results to study the bonding from an electronic structure perspective.

While performing the exercises try to think about the reasons for each step, and about how to interpret the results. The point of the exercise is not merely to reach the end but to learn the path. The exercise below contains a number of questions. Please take note when a question is asked of you, and think about the answer. Feel free to discuss the answer with one of the demonstrators after you have thought about it for a while.

The secondary aim of this exercise is to learn to run programs on Oxford University's Arcus cluster. This is a powerful parallel computer. (Although the runs in this first exercise should take only seconds on a desktop.)  For information about how to login to and use this cluster please refer to the instructions provided.
`
## Where To Find Help
If you want more information about a particular CASTEP keyword, or you want to find if CASTEP has particular functionality, there are a few places you can look.

* There is information on this website: [www.castep.org](http://www.castep.org).
* CASTEP has an in built help option to assist with using particular keywords.  Information on using CASTEP can be seen by using:

	`$ castep --help`

	To get more information on a particular input file keyword (e.g. `kpoint_mp_grid`) use:
	
	`$ castep --help kpoint_mp_grid`

	If you don't know the keyword you need to use, then you can search on a particular keyword. This returns a list of keywords that you might be interested in, e.g. to look at all keywords which contain a reference to symmetry.

	`$ castep --search symmetry`

	Finally, to list all keywords, use:

	`$ castep --search all`
	
	Note that the long-form arguments `--help` and `--search` can optionally be replaced with `-h` and `-s`, respectively.


## Example 1 - Silicon
1. Download the files to arcus

	`$ wget http://www.castep.org/files/Si2.tgz`

2. Unzip and untar them, then move into the new directory

	```
	$ gunzip Si2.tgz
	$ tar -xvf Si2.tar
	$ cd Si2
	```

3. Examine the CASTEP input files `Si2.cell` and `Si2.param` using your favourite text editor (e.g. `nano`).
The `Si_00.usp` file is a pseudopotential file, you do not need to understand it at the moment.

	```
	$ nano Si2.cell
	$ nano Si2.param
	```

4. It is useful to view the structure before submitting your calculation using CASTEP. Copy the `Si2.cell` to your local machine using the sftp window (left) in mobaXterm.

5. Cell Structure Visualisation
	* ### Jmol
	To open the `Si2.cell` file using [Jmol](http://www.jmol.org):
	Open Jmol (You will need to copy it from the shared drive to your desktop. Then double click `mol.jar`) 
	then use `File => Open` and navigate to your `Si2.cell` file.
	Alternatively, you can drag and drop the `Si2.cell` file into the Jmol window, and Jmol will open it. 
	It can be helpful to view multiple repeat units of your unit cell.  The easiest way to do this in Jmol is to open a console window,
	click File => Console and type:
	
		`$ load "" { 2 2 2 }`
	
		to show a 2x2x2 supercell.  Check the geometry of the input file is what you expect it to be before moving onto the next step.

	* ### Vesta
	To open the `Si2.cell` file using [VESTA](http://www.jp-minerals.org/vesta/en/):
	Open VESTA (You will need to copy it from the shared drive to your desktop. 
	Then double click `VESTA.exe`) then use File => Open and navigate to your `Si2.cell` file.
	You cannot drag and drop into VESTA.

		If you wish to create a supercell as above, use `Objects => Boundary`. 
	Then edit the maximum and/or minimum values of x, y, and z in order to change your boundaries.
	Setting `x(max)`, `y(max)`, and `z(max)` to 2 will create the 2 by 2 by 2 supercell as above. 

		Check the geometry of the input file is as expected before moving on to the next step.

6. Now run CASTEP on Arcus using the 2-atom input files.

	`$ castepsub -n 1 Si2`
	
	This should only take a few seconds and produce a readable output file `Si2.castep`. Examine this file and try to understand the meaning of the various parts. In particular check the section following the header which lists all of the input parameters, both explicit and default. Note what default values of the major parameters CASTEP chose where you did not specify them explicitly. (There will be some whose meaning has not been explained. Don't worry about these.) 

	* Find the section of the file which monitors the SCF loop and the approach to convergence. How many SCF iterations did it need?

7. Copy the output files `Si2.castep` and `Si2.den_fmt` to the local machine using `sftp`.

8. Visualisation of the charge density
	* ### Jmol
	Jmol can also be used to view the isodensity map, open the `.castep` file by dragging and dropping the `Si2.castep` file into the Jmol window. 

		Open the Jmol console (File => Console) and type the following commands:
		
		```
		$ load "" { 2 2 2 }
		$ isosurface rho cutoff 14 "Si2.den_fmt" lattice { 2 2 2 }
		```
		
		Note: you can use the `cd` command within Jmol to navigate to the folder with your `.castep` files.
Jmol uses forward slash for paths to files on windows and linux based machines.
This `Si2.den_fmt` file is a formatted file produced by CASTEP that contains the value of the electron density on a grid of points.  This isosurface command in Jmol plots an isodensity surface over your atomic positions.

	* ### Vesta
	You will see a file called `Si2.den_fmt` which contains the charge density in a formatted (i.e. a human readable, ASCII file). We need to change this file into a format Vesta can read. Copy it to a file called `Si2.charg_frm`

		```
		cp Si2.den_fmt Si2.charg_frm
		```
	Now edit the file `Si2.charg_frm` with a text editor to remove the first 11 lines. The file should now begin with `1 1 1` and a number. You can now open `Si2.charg_frm` with Vesta. Note that Vesta needs both the `.cell` and `.charge_frm` files to make a plot. If you are working on a remote machine you will need to copy both of these back to your local machine to view with Vesta. You can find a walkthrough video of this process [here](https://youtu.be/_c2Hk4jxmm4).

		![Silicon Charge Density](../img/silicon_charge_density.png)
		An alternative way to plot charge densities (and much more besides) is [c2x](https://www.c2x.org.uk).

	### Answer the following questions:
	1. Can you explain what you see as you vary the isosurface value?
	1. Can you see any features which might be characteristic of a covalently-bonded crystal.
	1. Do you notice anything strange about the electron density close to the Si nucleus? 
	1. Can you explain this as a consequence of the particular kind of electronic structure calculation you have just performed?

9. Repeat steps 1-8 using input files for sodium chloride and aluminium.

```
$ wget http://www.castep.org/files/Al.tgz
$ wget http://www.castep.org/files/NaCl.tgz
```

### Think about the following questions:
* Note what similarities and differences you find compared to silicon? 
* Does this help explain the difference in bond chemistry between silicon, sodium chloride and aluminium?
* Does this help explain why there are many reasonable classical potential functions for NaCl to be found
  in the simulation literature, but that finding good potentials for silicon is a very tough challenge?
* What about aluminium, can you find good potentials for aluminium?
