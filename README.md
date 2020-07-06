

![logo](https://github.com/Long-group/FullQuantumEigensolver/blob/master/logo.PNG)
> FQE is a quantum algorithm for calculating the ground state energy of a system Hamiltonian, and can be applied to a wide range of fields, such as determining the electronic structure of  fermionic systems, finding the ground energy of  molecules  in quantum chemistry.  For more information, see our release paper.

## Getting started
Performing FQE requires installing OpenFermion firstly. And, installing OpenFermion requires pip. Make sure that you are using an up-to-date version of it. You can click the [link](https://github.com/quantumlib/OpenFermion) to learn how to install OpenFermion. We use ProjectQ framework to program this quantum algorithm. You need to install ProjectQ with this [link](http://projectq.ch/code-and-docs/).

## An introduction of FQE
Full quantum eigensolver (FQE) is an algorithm  to  estimate the ground state of a given Hamiltonian using  quantum gradient descent. We adopt it to calculate the molecular ground energies and electronic structures. We embed this method in Python code and provide a full stack quantum simulation procedure. One feeds a molecule into this program and will obtain an  explicit quantum circuit, and calculating results of the molecular energy of this molecule, and corresponding electronic structures. It can be performed both by  a quantum simulator or a real quantum processor. Compared to existing classical-quantum hybrid methods such as variational quantum eigensolver (VQE), this method removes the classical optimizer and performs all the calculations on a quantum computer.  
A molecular system, containing a collection of nuclear charges  and  electrons, can be described by a  molecular Hamiltonian. By Jordan-Wigner or Bravyi-Kitaev transformations, the molecular Hamiltonian can be mapped into a qubit Hamiltonian form

![](http://latex.codecogs.com/gif.latex?H=\sum_{i,\alpha}h_{\alpha}^i\sigma_{\alpha}^i+\sum_{i,j,\alpha,\beta}h_{\alpha\beta}^{ij}\sigma_{\alpha}^{i}\sigma_{\beta}^j+\dots)

where Roman indices *i*, *j* denote the qubit on which the operator acts, and Greek indices ![](http://latex.codecogs.com/gif.latex?\\alpha), ![](http://latex.codecogs.com/gif.latex?\\beta) refer to  the type of Pauli operators, i.e.,  ![](http://latex.codecogs.com/gif.latex?\sigma^i_{x}) means Pauli matrix ![](http://latex.codecogs.com/gif.latex?\sigma_{x}) acting on a  qubit at site *i*.
We can calculate the ground state energy by minimizing the expect value of Hamiltonian

![](http://latex.codecogs.com/gif.latex?f(X)=X^THX)

The gradient descent of *𝑓(X)* can be deviated as

![](http://latex.codecogs.com/gif.latex? f(X, \\epsilon \\delta X)=(X+\\epsilon \\delta X)^TH(X+ \\epsilon \\delta X))

=X^⊤HX+ε(δX)^⊤HX+εX^⊤𝑅(δX)+ε^2(δX)^⊤H(δX) )

![](http://latex.codecogs.com/gif.latex? \frac{d}{dε}𝑓(X,εδX)=(δX)^⊤HX+X^⊤H(δX)+2ε(δX)^⊤H(δX) )
At the limit when ε->0 :
![](http://latex.codecogs.com/gif.latex?\frac{d}{d \epsilon}f(X,\epsilon \delta X)=(\delta X)^THX+X^TH(\delta X))
![](http://latex.codecogs.com/gif.latex? \frac{d}{d\epsilon}f(X,\epsilon \delta X)=2(\delta X)^THX )
![](http://latex.codecogs.com/gif.latex? ∇𝑓(X)=2HX )
Then, the classical gradient descent iteration  can be mapped to a quantum version by  being regarded as an evolution of *X* under operator *H*,
![](http://latex.codecogs.com/gif.latex?|X^{(t+1)}\rangle= \left ( |X^{(t)}\rangle -\gamma H |X^{(t)}\rangle \right))
where ![](http://latex.codecogs.com/gif.latex?\gamma) is learning rate.

Denoting  ![](http://latex.codecogs.com/gif.latex?H^{g}=I-\gamma H) and it can be expressed as 

![](http://latex.codecogs.com/gif.latex?H^{g}=\sum_{i=1}^{M}\beta_{i}H^{g}_{i})

where  *M* is the number of Pauli product terms  in  ![](http://latex.codecogs.com/gif.latex?H^{g} ). 
 Then the gradient descent process can be rewritten as 
 
![](http://latex.codecogs.com/gif.latex? |X^{(t+1)}\\rangle=H^{g} |X^{(t)}\\rangle =\sum_{i=1}^{M} \beta_iH^{g}_{i}|X^{(t)} \\rangle)

The above equation can be evaluated by the following quantum circuit
![circuit](https://github.com/Long-group/FullQuantumEigensolver/blob/master/circuit.PNG)

After enough iterative times, ![](http://latex.codecogs.com/gif.latex?|X\\rangle) will converge to the ground state and ![](https://latex.codecogs.com/gif.latex?\langle%20X|H|X%20\rangle) is the corresponding ground state energy.
 Moreover, by variating the interatomic distances of a molecule, this method can calculate the lowest energy in potential-energy surfaces corresponds to the most stable structure of the molecules.
 
## Example

We write a demo with **ProjectQ** in form of jupyter notebook. We solve the ground state and ground state energy of Hydrogen molecule in this demo. You can also change to solve other molecules. Some other molecular information is contained in **Molecules_Data.json**, which was copied from [NIST](https://cccbdb.nist.gov/justgeom.asp).

The program can be divided in three parts,

* Data input and parameter setting
* Compute the hamiltonian of the system
* Simulation with FullQuantumEigensolver(FQE)

Use the class **FullQuantumEigensovler** to bulid the object **FQE**, and run it with **K** iterations.

```
FQE = FullQuantumEigensolver(Gatelist, active_space_stop, active_space_start, initial_state)
FQE.start(K)   
```
In every iteration, the information about the quantum state remained will be output. After several iterations, the state will converge to the ground state. 
>**Note:** We use the **get_amplitude** to extract the state, so there will be no measurement in the program. Because of that, the program may output warning sign.


 
## Authors
ShiJie Wei, Zeng-Rong Zhou

Contact: long_group@163.com
## How to cite
When using FQE for research projects, please cite:
ShiJie Wei, Hang Li, GuiLu Long. A Full Quantum Eigensolver for Quantum Chemistry Simulations.  Research 2020, 1 (2020).




