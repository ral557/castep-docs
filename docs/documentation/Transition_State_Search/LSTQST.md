## Theory
The linear synchronous transit / quadratic synchronous transit (LST/QST) method is a useful method for finding transition states in simple energy landscapes. A 

LST/QST is a close-ended search, meaning that both the initial (reactant) and final (product) states have to be provided. The LST part of the transition state search linearly interpolates  between
the reactant and product, before performing an SCF calculation of the energy of several intermediate strutures. This is used to "bracket" the location of the maximum along the trajectory through means of a bisection search.

For the simplest reactions, such as the linear movement of an ion between two other ions (e.g. H + H_2 -> H_2 + H) the maximum in this straight line will be the transition state and therefore only performing the LST part of the search may be adequate. For many more complex reactions, however, the lowest energy pathway may not occur along the line that directly linearly interpolates between reactant and product states. To identify these transition states, it is possible to relax the LST transition state 
