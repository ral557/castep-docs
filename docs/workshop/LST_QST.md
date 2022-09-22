# Transition State Search Tutorial
## How Transition State Searches Work
	
There have been many suggestions for ways to identify transition states over the years, of which the  two most popular are `Linear Synchronous Transit / Quadratic Synchronous Transit (LST/QST)` and `Nudged Elastic Band (NEB)`.
	
These two techniques belong to the family of close-ended transition state searches - that is they require both an initial and final structure (reactant and product) and then they attempt to find the lowest barrier between them, the transition state. Open ended searches also exist in the literature, but they find the lowest barrier away from a reactant, which may not be useful for modelling a complex multi-step process.
	
Finally, the gold standard technique (which can be performed in practice but is prohibitively expensive to use for more than verification of transition states) is mode following; the transition state is a stationary state at a saddle point, and accordingly it should have only one imaginary vibrational (phonon) mode. Once a transition structure is predicted, it is possible to perform a phonon calculation in CASTEP to verify this (see phonon tutorials).

### LST/QST

* `LST/QST` is probably the most conceptually simple of the transition state search techniques, and is in itself an extension to the simple LST technique.
* In LST, a linear interpolation between reactant and product states is made (with a variable weighting), and then the total energy of the system is evaluated at various points (choices of weighting)  along this linear interpolation. A bisection search is then performed to find the local maximum along this path.
* Since `LST` performs a linear interpolation it is only guaranteed to find the energy maximum on that line - there is no guarantee that this sill in fact be the saddle point. Accordingly in `QST`, an energy minimisation is performed in directions orthogonal to the LST search direction. This then tends towards the saddle point, improving the estimate of the transition state.
* It is, however, possible that the QST optimisation will *fall off* the saddle point, which can make it a tricky process to converge, or cause the saddle point itself to be missed.
	
## Example: LST/QST

* Linear Synchronous Transit/ Quadratic Synchronous Transit (commonly known as `LST/QST`) is a quick and reasonably accurate method to find a transition state. 
* In this example, you will determine the transition barrier for a simple (and somewhat uninteresting) reaction, `H + H2 -> H2 + H`
* The files for this tutorial may be found at [LST_Tutorial](Attach:LST_Tutorial.tar.gz)

1. Geometry Optimisation

	Good transition state searches are based upon good structural relaxations of the initial and final states. The first step is therefore to perform geometry optimisations for your reactant `H_Initial` and product `H_Final`. 
	
	Note that ionic constraints have been used in these to prevent translations of 2 of the ions and accelerate the geometry optimisation. You may wish to investigate what happens when these constraints are relaxed.
	
2. Once you have checked your end points are relaxed (ask if you are unsure how to confirm this!), you will need to create a new block:

	```
	%block positions_frac_product
	.
	.
	.
	%endblock positions_frac_product
	```
	
	where you need to place the atomic positions from `H_Final-out.cell`.
	
	If doing this manually, you may find the following bash commands useful
	
	```
	cp reactant-out.cell TS.cell
	cat product-out.cell >> TS.cell
	```
	
	This will append the contents of a hypothetical `product-out.cell` to the end of the `reactant-out.cell` in a new file `TS.cell`.
	You will then need to rename the second `positions_frac` block and delete any repeated entries.
	
	**FOR THIS TUTORIAL: An Example TS.cell and TS.param file have been provided!**
	
3. Run the TS Search job
	Using `castepsub` submit the TS job.

4. Results Interpretation
	To watch the path of the transition search, you can drag the `.ts` file into Jmol and watch the animations in the same way outlined in the geometry optimisation tutorial.
	
	* Try and find the LST Transition State  in the `.castep` file. Is this what you would expect?
	
		```
		grep -A 7 "LST Maximum Found" TS.castep
		```
	* Try and find the QST Transition state in the `.castep` file.
		```
		grep -A 7 "Transition State Found" TS.castep
		```
	* Can you explain why the QST result seems odd? What about the initial geometry optimisation could be improved to give a better result?
	
## Extensions

* Think about XC-functional choice; LDA overbinds - how does this affect the transition barrier and initial structures? 
* What happens when the transition is between non-adjacent minima in the potential energy landscape? (This would require a  more complex example than the tutorial)
* What about longer distances? When do we need Van der Waal's interactions (DFT+D) ?
* What effect did the constraints have? Can you improve them?

