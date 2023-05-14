# Linear Synchronous Transit / Quadratic Synchronous Transit (LST/QST)

## Introduction to LST/QST
The linear synchronous transit / quadratic synchronous transit (LST/QST) method is a useful method for finding transition states in simple energy landscapes. A transition state here is typically defined as a saddle point between two local minima in the potential energy surface. 

Within transition state theory, the rate of a process is given by the Arrhenius equation:

```
k = k_0 e^{-E_barrier / k_BT}
```
where `k` is the rate of reaction, `k_0` some arbitrary constant (which Arrhenius originally defined as a "characteristic frequency", although it may be calculated from the thermodynamic properties of the system), `E_barrier` is the difference in relative energies between the initial state (reactant) and the transition state and `k_BT` is the available thermal energy. Since the thermal barrier is part of a negative exponential, it is found that at low temperatures  (k_BT << E_Barrier) the lowest energy pathway dominates the reaction kinetics. 

## LST/QST Algorithm

LST/QST is a close-ended search, meaning that both the initial (reactant) and final (product) states have to be provided. The LST part of the transition state search linearly interpolates  between
the reactant and product, before performing an SCF calculation of the energy of several intermediate strutures. This is used to "bracket" the location of the maximum along the trajectory through means of a bisection search.

For the simplest reactions, such as the linear movement of an ion between two other ions (e.g. H + H_2 -> H_2 + H) the maximum in this straight line will be the transition state and therefore only performing the LST part of the search may be adequate. For many more complex reactions, however, the lowest energy pathway may not occur along the line that directly linearly interpolates between reactant and product states. To identify these transition states, it is possible to relax the LST transition state along a direction *orthogonal* to the interpolated direction. This is the QST part of the algorithm. By maintaining orthogonality to the original search trajectory, the search avoids relaxing back into either potential well, thereby finding the saddle point that represents the transition state between the initial and final states.


### Advantages of LST/QST

The LST/QST method is relatively cheap compared with other transition state searching methods, making it useful for simple systems -where it can give the same quality of results as more advanced methods at a fraction of the computational cost. 

### Limitations of LST/QST

LST/QST works well for simple systems, however in more complex energy landscapes it is liable to fail. This is guaranteed to be the case if a stable (at a local minimum) intermediate exists between the reactant and the supposed-product that have been provided.

It may also fail if the true transition state is a long way from the linearly interpolated maximum. In both of these cases, it is recommended to consider using the nudged elastic band (NEB) transition state search instead, since that is a more robust (if more expensive) algorithm.

LST/QST is also not capable of providing the shape of the barrier, only its height, and cannot be used in variable-cell calculations.

### General advice for LST/QST calculations

LST/QST depends on having well-optimised initial and final structures. It is therefore advised to perform rigorous geometry optimisation, as if a minimum (better optimised) structure is found on the transition state pathway then the LST/QST algorithm will fail.

It is also recommended that the electronic part of the wavefunction is well-converged, since additional noise due to lack of convergence can cause the algorithm to struggle, especially in cases with small barriers. Trying to perform transition state searches with an under-converged wavefunction is therefore a false economy.  

### Confirming a transition state

For a transition state which is a saddle point (the case in a continuous energy landscape) it is possible to confirm the result of an LST/QST run by performing a phonon calculation on the transition state. Due to the fact a saddle point has 1 direction with negative curvature, a phonon calculation on a valid transition state will find that there is one phonon mode (out of all possible modes of the system) which has a negative eigenvalue - this is a signature that the identified transition state is 


## LST/QST parameters

