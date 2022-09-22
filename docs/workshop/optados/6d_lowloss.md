(:title JDOS and OPTICS:)

!!JDOS
See  @@examples/Si2_JDOS@@. This is a simple example of using optados for calculating joint electronic density of states.  We choose to recalculate the Fermi level using the calculated DOS, rather than use the Fermi level suggested by castep, and so @@EFERMI: OPTADOS@@ is included in the @@Si2.odi@@ file.  

#Execute castep and optados using the example files.  The JDOS is written to @@Si2.jadaptive.dat@@. A file suitable for plotting using @@xmgrace@@ is written to @@Si2.jadaptive.agr@@.
#Check the effect of changing the sampling by increasing and decreasing the value of @@JDOS_SPACING@@ in the @@Si2.odi@@ file. 
 
!!OPTICS
Two sets of example files are provided for calculations of optical properties.  For each example, the castep files containing all the cell and simulation parameters are included, along with an optados input file. We assume that the reader is familiar with the previous sections on DOS and JDOS.  


!!!Silicon
@@examples/Si2_OPTICS/@@  This is a simple example of using optados to calculate the optical properties of crystalline silicon, which is an insulator. 

INSTRUCTIONS:
* Execute optados to calculate the optical properties.  Several @@*.dat@@ files are produced:
** @@Si2_OPTICS_absorption.dat@@ : This file contains the absorption coefficient (second column) as function of energy (first column).
** @@Si2_OPTICS_conductivity.dat@@ : This file contains the conductivity outputted in SI units (Siemens per metre).  The columns are the energy, real part  and imaginary part of the conductivity respectively.  
** @@Si2_OPTICS_epsilon.dat@@ : This file contains the dielectric function.  The columns are the energy and real and imaginary parts of the dielectric function respectively. The file header also includes the result of the sum rule \\
Attach:sum3.png\\
N'_eff_' is the effective number of electrons contributing to the absorption process, and is a function of energy.    
** @@Si2_OPTICS_loss_fn.dat@@ : This file contains the loss function (second column) as a function of energy (first column).  The header of the file shows the results of the two sum rules associated with the loss function \\
Attach:sum1.png\\
and\\
Attach:sum2.png

** @@Si2_OPTICS_reflection.dat@@ : This file contains the reflection coefficient (second column) as a function of energy (first column).
** @@Si2_OPTICS_refractive_index.dat@@ : This file contains the refractive index.  The columns are the energy and real and imaginary parts of the refractive index respectively.

Corresponding @@*.agr@@ files are also generated which can be plotted easily using xmgrace.

* Change parameters @@JDOS_SPACING@@ and @@JDOS_MAX@@ and check the effect on the optical properties.  Note: all of the other optical properties are derived from the dielectric function.  

*  The optados input file has been set up to calculate the optical properties in the polycrystalline geometry (@@optics_geom = polycrystalline@@).  It is possible to calculate either polarised or unpolarised geometries, or to calculate the full dielectric tensor.  To calculate the full dielectric tensor set @@optics_geom = tensor@@.  This time only the file @@Si2_OPTICS_epsilon.dat@@ is generated.  The format of this file is the same as before (the columns are the energy and the real and imaginary parts of the dielectric function respectively), but this time the six different components of the tensor are listed sequentially in the order &epsilon;'_xx_', &epsilon;'_yy_', &epsilon;'_zz_', &epsilon;'_xy_', &epsilon;'_xz_' and &epsilon;'_yz_'.

* Additional broadening can be included in the calculation of the loss function.  This is done by including the keyword @@optics_lossfn_broadening@@ in the optados input file.  If you include this keyword and re-run optados, you will find that the file @@Si2_OPTICS_loss_fn.dat@@ now has three columns.  These are the energy, unbroadened spectrum and broadened spectrum respectively.  

!!Aluminium
 
*Aluminium is a metal so we need to include both the interband and intraband contributions to the dielectric function.  To include the intraband contribution @@optics_intraband = true@@ must be included in the optados input file.  When you run optados, the same files are generated as when only the interband term is included.  

*The @@Al_OPTICS_epsilon.dat@@ file has the same format as before, but it now contains sequentially the interband contribution, the intraband contribution and the total dielectric function.  The file @@Al_OPTICS_epsilon.agr@@ only contains the interband term.  In the same way, @@Al_OPTICS_loss_fn.dat@@ contains the interband contribution, intraband contribution and total loss function.  All other optical properties are calculated from the total dielectric function and the format of the output files remains the same.

* In the case where the dielectric tensor is calculated and the intraband term is included, only the @@Al_OPTICS_epsilon.dat@@  file is generated.  As before it contains each component, but this time it lists sequentially the interband contribution, intraband contribution and total dielectric function for each component.   

* This time, if additional broadening for the loss function is included by using the key word @@optics_lossfn_broadening@@,  @@AL_OPTICS_loss_fn.dat@@ will contains four sequential data sets.  These are the interband contribution, the intraband contribution, the total loss function without the additional broadening and the broadened total loss function.  



