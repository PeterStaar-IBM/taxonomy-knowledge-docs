## QESEM: A Qiskit Function by Qedma

## Note

Qiskit Functions are an experimental feature available only to IBM Quantum™ Premium Plan users. They are in preview release status and subject to change.

## Overview

While quantum processing units have vastly improved in recent years, errors due to noise and imperfections in existing hardware remain a central challenge for quantum algorithm developers. As the field approaches utility-scale quantum computations that cannot be verified classically, solutions for canceling noise with guaranteed accuracy are becoming increasingly important. To overcome this challenge, Qedma has developed Quantum Error Suppression and Error Mitigation (QESEM), seamlessly integrated on IBM Quantum Platform as a Qiskit Function.

With QESEM, users can run their quantum circuits on noisy QPUs to obtain highly accurate error-free results with highly efficient QPU-time overheads, close to fundamental bounds. To achieve this, QESEM leverages a suite of propriety methods developed by Qedma, for the characterization and reduction of errors. Error reduction techniques include gate optimization, noise-aware transpilation, error suppression (ES), and unbiased error mitigation (EM). With this combination of these characterization-based methods, users can achieve reliable, error-free results for generic large-volume quantum circuits, unlocking applications that cannot be accomplished otherwise.

## Description

You can use the QESEM function by Qedma to easily estimate and execute your circuits with error suppression and mitigation, achieving larger circuit volumes and higher accuracies. To use QESEM, you provide a quantum circuit, a set of observables to measure, a target statistical accuracy for each observable, and a chosen QPU. Before you run the circuit to the target accuracy, you can estimate the required QPU time based on an analytical calculation that does not require circuit execution. Once you are satisfied with the QPU time estimation, you can execute the circuit with QESEM.

When you execute a circuit, QESEM runs a device characterization protocol tailored to your circuit, yielding a reliable noise model for the errors occurring in the circuit. Based on the characterization, QESEM first implements noise-aware transpilation to map the input circuit onto a set of physical qubits and gates, which minimizes the noise affecting the target observable. These include the natively available gates (CX/CZ on IBM® devices), as well as additional gates optimized by QESEM, forming QESEM's extended gate set. QESEM then runs a set of characterizationbased ES and EM circuits on the QPU and collects their measurement outcomes. These are then classically post-processed to provide an unbiased expectation value and an error bar for each observable, corresponding to the requested accuracy.

<!-- image -->

## Unique error mitigation features

QESEM has been demonstrated to provide high-accuracy results for a variety of quantum applications and on the largest circuit volumes achievable today. QESEM offers the following user-facing features, demonstrated in the benchmarks section below:

1. Guaranteed accuracy: QESEM outputs unbiased estimations for expectation values of observables. Its EM method is equipped with theoretical guarantees, which - together with Qedma's cutting-edge characterization - ensure the mitigation converges to the noiseless circuit output up to the user-specified accuracy. In contrast to many heuristic EM methods that are prone to systematic errors or biases, QESEM's guaranteed accuracy is essential for ensuring reliable results in generic quantum circuits and observables.

2. Scalability to large QPUs: QESEM's QPU time depends on circuit volumes, but is otherwise independent of the number of qubits. Qedma has demonstrated QESEM on the largest quantum devices available today, including the IBM Quantum 127-qubit Eagle and 133-qubit Heron devices.

3. Application-agnostic: QESEM has been demonstrated on a variety of applications, including Hamiltonian simulation, VQE, QAOA, and amplitude estimation. Users can input any quantum circuit and observable to be measured, and obtain accurate error-free results. The only limitations are dictated by the hardware specifications and allocated QPU time, which determine the accessible circuit volumes and output accuracies. In contrast, many error reduction solutions are application-specific or involve uncontrolled heuristics, rendering them inapplicable for generic quantum circuits and applications.

4. Extended gate set: QESEM supports fractional-angle gates, and provides Qedma-optimized fractional-angle gates on IBM Quantum Eagle devices. This extended gate set enables more efficient compilation and unlocks circuit volumes larger by a factor of up to 2 compared to default CX/CZ compilation. Rzz ( θ )

5. Multibase observables: QESEM supports input observables composed of many non-commuting Pauli strings, such as generic Hamiltonians. The choice of measurement bases and the optimization of QPU resource allocation (shots and circuits) is then performed automatically by QESEM to minimize the required QPU time for the requested accuracy. This optimization, which takes into account hardware fidelities and execution rates, enables you to run deeper circuits and obtain higher accuracies.

## Function parameters

| Name           | Type                       | Description                                                         |
|----------------|----------------------------|---------------------------------------------------------------------|
| instance       | str                        | The hub/group/proj to use in that forma                             |
| action         | str                        | The required action "estimate_qpu_time "execute"                    |
| pubs           | Iterable[EstimatorPubLike] | A pub-like object in form of (circuit, observables)                 |
| precision      | float                      | The target precision expectation value estimates of each observable |
| run_options    | dict                       | Includes the name  the backend to run                               |
| custom_options | dict                       | Advanced features: transpilation_l e and  max_qpu_tim e seconds)    |

## Get started

Authenticate using your IBM Quantum Platform API token , and select the Qiskit Function as follows:

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC atal og 2 3 catalog=Q iskitFunctionsC atalog(token="<YO UR _IQ P_API_T 4 5 qesem_function =catalog.load('qedma/qesem')

## Example

The QESEM function must get the following parameters to be executed:

action

pubs (circuit, observables)

precision

run_options

To get started, try the basic example of estimating QPU time:

1 job=qesem_function.run( 2 action="estimate_qpu_time", 3 instance="hub/group/project", 4 pubs=[ 5 ( 6 circ, 7 [obs1 ,obs2,obs3] 8 ) 9 ], 1 0 precision=0.03, 1 1 run_options={ 1 2 "backend_name": "ibm_brisbane", 1 3 } 1 4 )

The following example executes a QESEM job:

1 job=qesem_function.run( 2 action="execute", 3 instance="hub/group/project", 4 pubs=[ 5 ( 6 circ, 7 [obs1 ,obs2,obs3] 8 ) 9 ], 1 0 precision=0.03, 1 1 run_options={ 1 2 "backend_name": "ibm_brisbane", 1 3 } 1 4 )

## Note

The precision parameter signifies the acceptable error on the expectation values of the observables in absolute value. Namely, the QPU runtime for mitigation will be determined to provide output values for all the observables of interest that fall within a 1 σ confidence interval of the target precision. If multiple observables are provided, the mitigation will run until the target precision is reached for each of the input observables.

Currently QESEM supports a single PU B .

You can use the familiar Qiskit Serverless APIs to check your Qiskit Function workload's status or return results:

1 print(job.status()) 2 result=job.result()

## Custom options

Provide the custom_options parameter to set additional advanced features for the QESEM function:

1 # exampleexecuteQ ESEM job 2 job=qesem_function.run( 3 action="execute", 4 instance="hub/group/project", 5 pubs=[ 6 ( 7 bell, 8 [obs1 ,obs2,obs3] 9 ) 1 0 ], 1 1 precision=0.03, 1 2 run_options={ 1 3 "backend_name": "ibm_brisbane", 1 4 }, 1 5 custom_options={ 1 6 "max_qpu_time": 1 4400, 1 7 "transpilation_level": 0 1 8 } 1 9 )

max_qpu_time : Allows you to limit the QPU time, specified in seconds, to be used for the entire QESEM process. Since the final QPU time required to reach the target accuracy is determined dynamically during the QESEM job, this parameter enables you to limit the cost of the experiment. If the dynamically-determined QPU time is shorter than the time allocated by the user, this parameter will not affect the experiment. The max_qpu_time parameter is particularly useful in cases where the analytical time estimate provided by QESEM before the job starts is too pessimistic and the user wants to initiate a mitigation job anyway. After the time limit it reached, QESEM stops sending new circuits. Circuits that have already been sent continue running (so the total time may surpass the limit by up to 30 minutes), and the user receives the processed results from the circuits that ran up to that point. If you want to apply a QPU time limit shorter than the analytical time estimate, consult with Qedma to obtain an estimate for the accuracy achievable within the time limit.

transpilation_level : After a circuit is submitted to QESEM, it automatically prepares several alternative circuit transpilations and chooses the one that minimizes QPU time. For instance, alternative transpilations might utilize Qedma-optimized fractional RZZ gates to reduce the circuit depth. Of course, all transpilations are equivalent to the input circuit, in terms of their ideal output.

To exert more control over the circuit transpilation, set the transpilation level in the Q esemO ptions . While 'level 1' corresponds to the default behavior described above, 'level 0' includes only minimal modifications

required for high-accuracy output; for example, 'layerification' - the organization of circuit operations into 'layers' of simultaneous two-qubit gates. Note that automatic hardware-mapping onto high-fidelity qubits is applied in any case.

|   transpilation_level | description                                                                                                                                                                                                                                                                                                      |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     1 | Default QESEM transpilation. Prepares several alternative transpilations and chooses the one that minimizes QPU time. Barriers may be modified in the layerification step. Minimal transpilation: the mitigated                                                                                                  |
|                     0 | circuit will closely resemble the input circuit structurally. Circuits provided in level 0 should match the device connectivity and should be specified in terms of the following gates: CX, Rzz(α), and standard single-qubit gates (U, x, sx, rz, etc). Barriers will be respected in the layerification step. |

<!-- image -->

Note

Qiskit barriers are typically used to specify the layers of two-qubit gates in quantum circuits. In level 0, QESEM preserves the layers specified by the barriers. In level 1, the layers specified by the barriers are considered as one transpilation alternative when minimizing QPU time.

## Benchmarks

QESEM has been tested on a wide variety of use cases and applications. The following examples can assist you with assessing which types of workloads you can run with QESEM.

A key figure of merit for quantifying the hardness of both error mitigation and classical simulation for a given circuit and observable is active

volume : the number of CNOT gates affecting the observable in the circuit. The active volume depends on the circuit depth and width, on the observable weight, and on the circuit structure, which determines the lightcone of the observable. For further details, see the talk from the 2024 IBM Quantum Summit . QESEM provides particularly large value in the high-volume regime, giving reliable results for generic circuits and observables.

<!-- image -->

| Application                        |   Number of qubits | Device     | Circuit description                                      | Accuracy   | Total time   |
|------------------------------------|--------------------|------------|----------------------------------------------------------|------------|--------------|
| VQE circuit                        |                  8 | Eagle (r3) | 21 total layers, 9 measurement bases, 1D chain           | 98%        | 35 min       |
| Kicked Ising                       |                 28 | Eagle (r3) | 3 unique layers x 3 steps, 2D heavy-hex topology         | 97%        | 22 min       |
| Kicked Ising                       |                 28 | Eagle (r3) | 3 unique layers x 8 steps, 2D heavy-hex                  | 97%        | 116 min      |
| Trotterized Hamiltonian simulation |                 40 | Eagle (r3) | 2 unique layers x 10 Trotter steps, 1D chain             | 97%        | 3 hours      |
| Trotterized Hamiltonian simulation |                119 | Eagle (r3) | 3 unique layers x 9 Trotter steps, 2D heavy-hex topology | 95%        | 6.5 hours    |
| Kicked Ising                       |                136 | Heron (r2) | 3 unique layers x 15 steps, 2D heavy-hex topology        | 99%        | 52 min       |

Accuracy is measured here relative to the ideal value of the observable: , where ' ' is the absolute precision of the mitigation (set by the ⟨ O ⟩ ideal ⟨ O ⟩ - ϵ ideal ϵ

user input), and is the observable at the noiseless circuit. 'Runtime usage' measures the usage of the benchmark in batch mode (sum over usage of individual jobs), whereas 'total time' measures usage in session mode (experiment wall time), which includes additional classical and communication times. QESEM is available for execution in both modes, so that users can make the best use of their available resources. ⟨ O ⟩ ideal

The 28-qubit Kicked Ising circuits simulate the Discrete Time Quasicrystal studied by Shinjo et al. (see arXiv 2403.16718 and Q2B24 Tokyo ) on three connected loops of ibm_kawasaki. The circuit parameters taken here are , with a ferromagnetic initial state . The measured observable is the absolute value of the magnetization . The utility-scale Kicked Ising experiment was run on the 136 best qubits of ibm_fez; this particular benchmark was run at the Clifford angle , at which the active volume grows slowly with circuit depth, which - together with the high device fidelities - enables high accuracy at a short runtime. ( θ , θ ) = x z (0.9 π , 0) ∣ ψ ⟩ = 0 ∣0⟩ ⊗ n M = ∣ ⟨ Z ⟩∣ 28 1 ∑$_{i}$$_{=0}$ 27 i ( θ , θ ) = x z ( π , 0)

Trotterized Hamiltonian simulation circuits are for a Transverse-Field Ising model at fractional angles: and correspondingly (see Q2B24 Tokyo ). The utility-scale circuit was run on the 119 best qubits of ibm_brisbane, whereas the 40-qubit experiment was run on the best available chain. The accuracy is reported for the magnetization; high-accuracy results were obtained for higher-weight observables as well. ( θ , θ ) = zz x ( π /4, π /8) ( θ , θ ) = zz x ( π /6, π /8)

The VQE circuit was developed together with researchers from the Center for Quantum Technology and Applications at the Deutsches ElektronenSynchrotron (DESY). The target observable here was a Hamiltonian consisting of a large number of non-commuting Pauli strings, emphasizing QESEM's optimized performance for multi-basis observables. Mitigation was applied to a classically-optimized ansatz; although these results are still unpublished, results of the same quality will be obtained for different circuits with similar structural properties.

## Get support

The Qedma support team is here to help! If you encounter any issues or have questions about using the QESEM Qiskit Function, please don't

hesitate to reach out. Our knowledgeable and friendly support staff are ready to assist you with any technical concerns or inquiries you may have.

You can email us at support@qedma.com for assistance. Please include as much detail as possible about the issue you're experiencing to help us provide a swift and accurate response. You can also contact your dedicated Qedma POC representative via email or phone.

To help us assist you more efficiently, please provide the following information when you contact us:

A detailed description of the issue

Any relevant error messages or codes

Steps to reproduce the issue

Your contact information

We are committed to providing you with prompt and effective support to ensure you have the best possible experience with our API.

We are always looking to improve our product and we welcome your suggestions! If you have ideas on how we can enhance our services or features you'd like to see, please send us your thoughts to support@qedma.com

## Next steps

<!-- image -->

Was this page helpful?

Report a bug or request content on GitHub .


<!-- image -->