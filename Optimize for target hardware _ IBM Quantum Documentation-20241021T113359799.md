## Optimize for target hardware

In the "optimize for target hardware" step of a Qiskit pattern, you take the abstract circuits (or operators) produced from the map step and perform a series of optimizations on them. This can include mapping the route and layout of the circuit to physical qubit hardware, converting to basis gates of the hardware, and reducing the number of operations, all designed to optimize the likelihood of success in the later execute step. At this point you might also wish to debug your circuits with a simulator before executing on real hardware in the next step.

During this step, abstract circuits must be transpiled to Instruction Set Architecture (ISA) circuits. An ISA circuit is one that only consists of gates understood by the target hardware (basis gates), and any multiqubit gates needed to obey any connectivity constraints (coupling map). Only ISA circuits can be run on IBM® hardware using IBM Qiskit Runtime.

## Guides for optimizing for target hardware

## Get started with the Qiskit transpiler

Introduction to transpilation

Transpiler stages

Transpile with pass managers

## Configure preset pass managers

Default settings and configuration options

Set optimization level

Commonly used parameters for transpilation

Represent quantum computers

## Advanced transpilation resources

Create a pass manager for dynamical decoupling

Write a custom transpiler pass

Transpile against custom backends

Install and use transpiler plugins

Create a transpiler plugin

Transpile using REST API

## Qiskit Transpiler Service

Transpile circuits remotely with the Qiskit Transpiler Service

AI transpiler passes

## Debugging tools

Introduction to debugging tools

Exact simulation with Qiskit SDK primitives

Exact and noisy simulation with Qiskit Aer primitives

Qiskit Runtime local testing mode

Build noise models

Plot quantum states

Efficient simulation of stabilizer circuits with Qiskit Aer primitives

## Qiskit Functions

IBM Circuit function

Algorithmiq Tensor-network error mitigation

Q-CTRL Performance Management

QEDMA QESEM

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .