## Execute on target hardware

<!-- image -->

The "execute on hardware" step of a Qiskit pattern involves running your circuits on hardware and produces the outputs of the quantum computation. The ISA circuits produced in the previous step can be executed using either a Sampler or Estimator primitive from Qiskit Runtime, initialized locally on your computer or from a cluster or other heterogeneous compute environment. These can be executed in a Batch, which allows parallel transpilation for classical computational efficiency or a Session, which allows iterative tasks to be implemented efficiently without queuing delays.

During this step, there is also the option to configure certain error suppression and mitigation techniques provided by Qiskit Runtime.

Depending on whether you are using the Sampler or Estimator primitive, the outcome of this step will be different. If using the Sampler, the output will be per-shot measurements in the form of bitstrings. If using the Estimator, the output will be expectation values of observables corresponding to physical quantities or cost functions.

## Guides for executing on hardware

## Run with primitives

Introduction to primitives

Get started with primitives

PUBs and primitive results

Primitives examples

Primitives with REST API

Noise learning helper

## Configure runtime options

Overview

Specify options

Configure error suppression

Configure error mitigation

Error mitigation and suppression techniques

## Execution modes

Introduction to execution modes

Introduction to sessions

Run jobs in a session

Run jobs in a batch

Fixed and dynamic repetition rate execution

Execution modes using REST API

Execution modes FAQs

## Manage jobs

Monitor or cancel a job

Estimate job run time

Minimize job run time

Maximum execution time

Job limits

## QPU and platform information

Processor types

QPU information

Get QPU information with Qiskit

Native gates and operations

Retired QPUs

Hardware considerations and limitations for classical feedforward and control flow

Instances

Fair-share scheduler

Manage cost

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .