# QuantumAutoEncoder
A Pennylane/Python script for a model of a quantum auto encoder, described by a paper by Romero et al. dated February 2017

The methodology of the algorithm is as follows:
- initialize a five-qubit wire (four qubits + one reference ancilla)
- initialize a four qubit state to each of the 16 possible combinations of |0> and |1> states and the ancilla to a |0> state
- generate a list of random angles ranging from 0 to 2 pi and a list of random Pauli rotations (RX, RY, RZ)
- implement a unitary transform over the four qubits using the angles and Paulis randomly generated
  - apply a Pauli rotation to each qubit at a random angle
  - apply controlled Pauli rotations from each qubit to every other qubit at random angles (n^2-n gates)
  - apply another Pauli rotation to each qubit at a random angle (final: n^2+n gates)
- implement a SWAP gate between the ancilla and one of the four qubits
- implement the inverse of the unitary over the system with the ancilla substituting for the qubit swapped out to decode
- measure the fidelity of the swapped out qubit (trash state) with the |0> initial ancilla
- after doing so for all 16 iitial states, average the fidelities to get the final fidelity of the model
- run with high shots to obtain a set of rotations and paulis that yields high fidelity
- output is the fidelity, followed by the Pauli and rotation angle of each of the rotations in the unitary to obtain that fidelity
