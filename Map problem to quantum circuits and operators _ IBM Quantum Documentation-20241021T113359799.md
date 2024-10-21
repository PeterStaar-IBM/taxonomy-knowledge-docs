## Map problem to quantum circuits and operators

<!-- image -->

The "map problem to quantum circuits and operators" step of a Qiskit pattern describes how a user starts with a classical problem and figures out how to map it to a quantum computer.

For example, in applications such as chemistry and quantum simulation, this step generally involves constructing a quantum circuit representing the Hamiltonian you are attempting to solve. During this step, for certain problems, it might also be desirable to specify the mapping of the problem onto qubits in the heavy-hex (or gross) lattice of IBM® hardware from the outset if the structure of the problem lends itself to optimization earlier.

It is also worth considering at this point what the outcome of the particular algorithm will be in preparation for the later execute step - for example, if the desired outcome involves inferring correlation functions using Hadamard tests, you might prepare to use Sampler, whereas specifying observables would use Estimator and could provide many error mitigation options.

The output of this step in a Qiskit pattern is normally a collection of circuits or quantum operators.

## Guides for mapping problems to quantum circuits and operators

## Build circuits with the Qiskit SDK

Circuit library

Construct circuits

Measure qubits

Visualize circuits

Classical feedforward and control flow

Synthesize unitary operators

Bit-ordering in the Qiskit SDK

Save circuits to disk

## Build operators with the Qiskit SDK

Operators module overview

Specifying observables in the Pauli basis

The Operator class

## Other circuit building tools

Pulse schedules

OpenQASM

Intro to OpenQASM

OpenQASM 2 and the Qiskit SDK

OpenQASM 3 and the Qiskit SDK

OpenQASM 3 feature table

OpenQASM 3.x live specification

Was this page helpful?

<!-- image -->

Yes

Report a bug or request content on GitHub .