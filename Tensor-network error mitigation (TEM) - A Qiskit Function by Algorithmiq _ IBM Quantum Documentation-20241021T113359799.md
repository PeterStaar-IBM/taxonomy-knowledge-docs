## Tensor-network error mitigation (TEM): A Qiskit Function by Algorithmiq

## Note

Qiskit Functions are an experimental feature available only to IBM Quantum™ Premium Plan users. They are in preview release status and subject to change.

## Overview

Algorithmiq's Tensor-network Error Mitigation (TEM) method is a hybrid quantum-classical algorithm designed for performing noise mitigation entirely at the classical post-processing stage. With TEM, the user can compute the expectation values of observables mitigating the inevitable noise-induced errors that occur on quantum hardware with increased accuracy and cost efficiency, making it a highly attractive option for quantum researchers and industry practitioners alike.

The method consists of constructing a tensor network representing the inverse of the global noise channel affecting the state of the quantum processor and then applying the map to informationally complete measurement outcomes acquired from the noisy state to obtain unbiased estimators for the observables.

As an advantage, TEM leverages informationally complete measurements to give access to a vast set of mitigated expectation values of observables and has optimal sampling overhead on the quantum hardware, as described in Filippov at al. (2023), arXiv:2307.11740 , and Filippov at al. (2024), arXiv:2403.13542 . The measurement overhead refers to the number of additional measurements required to perform efficient error mitigation, a critical factor in the feasibility of quantum

computations. Therefore, TEM has the potential to enable quantum advantage in complex scenarios, such as applications in the fields of quantum chaos, many-body physics, Hubbard dynamics, and small molecule chemistry simulations.

The main features and benefits of TEM can be summarized as:

1. Optimal measurement overhead : TEM is optimal with respect to theoretical bounds , meaning that no method can achieve a smaller measurement overhead. In other words, TEM requires the minimum number of additional measurements to perform error mitigation. This in turns means that TEM uses minimal quantum runtime.

2. Cost-effectiveness : Since TEM handles noise mitigation entirely in the post-processing stage, there is no need to add extra circuits to the quantum computer, which not only makes the computation cheaper but also diminishes the risk of introducing additional errors due to the imperfections of quantum devices.

3. Estimation of multiple observables : Thanks to informationallycomplete measurements, TEM efficiently estimates multiple observables with the same measurement data from the quantum computer.

4. Measurement error mitigation : The TEM Qiskit Function also includes a proprietary measurement error mitigation method able to significantly reduce readout errors after a short calibration run.

5. Accuracy : TEM significantly improves the accuracy and reliability of digital quantum simulations, making quantum algorithms more precise and dependable.

## Description

The TEM function allows you to obtain error-mitigated expectation values for multiple observables on a quantum circuit with minimal sampling overhead. The circuit is measured with an informationally complete positive operator-valued measure (IC-POVM), and the collected measurement outcomes are processed on a classical computer. This measurement is used to perform the tensor network methods and build a noise-inversion map. The function applies a map that fully inverts the whole noisy circuit using tensor networks to represent the noisy layers.

<!-- image -->

Error-mitigated estimation of an observable O via post-processing measurement outcomes of the noisy quantum processor. U and N denote an ideal quantum operation and the associated noise map, which can be generally non-local (and extended to grey boxes). D stands for a tensor of operators that are dual to the effects in the IC measurement. The noise mitigation module M is a tensor network that is efficiently contracted from the middle out. The first iteration of the contraction is represented by the dotted purple line, the second one by the dashed line, and the third one by the solid line.

Once the circuits are submitted to the function, they are transpiled and optimized to minimize the number of layers with two-qubit gates (the noisier gates on quantum devices). The noise affecting the layers is learned through Qiskit Runtime using a sparse Pauli-Lindblad noise model as described in E. van den Berg, Z. Minev, A. Kandala, K. Temme, Nat. Phys. (2023). arXiv:2201.09866 .

The noise model is an accurate description of the noise on the device able to capture subtle features, including qubit cross-talk. However, noise on the devices can fluctuate and drift and the learned noise might not be accurate at the point at which the estimation is done. This might result in inaccurate results.

## Get started

Authenticate using your IBM Quantum Platform API token , and select the TEM function as follows:

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC atal og 2 3 tem_function_name="algorithmiq/tem" 4 5 catalog=Q iskitFunctionsC atalog(token="<YO UR _IQ P_API_T 6 7 # Loadyourfunction 8 tem=catalog.load(tem_function_name)

## Example

The following snippet shows an example where TEM is used to compute the expectation values of an observable given a simple quantum circuit.

Use the Qiskit Serverless APIs to check your Qiskit Function workload's status:

You can return the results as: print( job.status( )) 1 result=job.result( ) 2 evs=result[0].data.evs

Use the Qiskit Serverless APIs to check your Qiskit Function workload's status: You can return the results as: 1 fromqiskitimportQ uantumC ircuit 2 fromqiskit.quantum_infoimportSparseP auliO p 3 fromqiskit_ibm_runtimeimportQ iskitR untimeService 4 5 # C reateaquantumcircuit 6 qc=Q uantumC ircuit( 3) 7 qc.u( 0.4, 0.9, -0.3, 0) 8 qc.u( -0.4, 0.2, 1 .3, 1 ) 9 qc.u( -1 .2, -1 .2, 0.3, 2) 1 0 for_ inrange( 2): 1 1 qc.barrier( ) 1 2 qc.cx( 0, 1 ) 1 3 qc.cx( 2, 1 ) 1 4 qc.barrier( ) 1 5 qc.u( 0.4, 0.9, -0.3, 0) 1 6 qc.u( -0.4, 0.2, 1 .3, 1 ) 1 7 qc.u( -1 .2, -1 .2, 0.3, 2) 1 8 1 9 # D efinetheobservables 20 observable=SparseP auliO p( "IYX", 1 .0) 21 22 # D efinetheexecutionoptions 23 service=Q iskitR untimeService( ) 24 backend_name=service.least_busy( operational=True).na 25 26 instance="<IQ P _HUB/ IQ P _GR O UP / IQ P _P R O J EC T>" 27 28 pub=( qc, [observable]) 29 options={ 30 "max_execution_time": 60, 31 } 32 job=tem.run( pubs=[pub], instance=instance, backend_n print( job.status( )) 1 result=job.result( ) 2 evs=result[0].data.evs

<!-- image -->

## Info

The expected value for the noiseless circuit for the given operator should be around 0.1 8409094298943401 .

## Inputs

## Parameters

| Name         | Type                       | Description                                                     |
|--------------|----------------------------|-----------------------------------------------------------------|
| pubs         | Iterable[EstimatorPubLike] | such as tuples  (cir c (circuit, observa b See Overview of PUBs |
| backend_name | str                        | Name of the backend                                             |
| instance     | str                        | The hub/group/projec                                            |
| options      | dict                       | Input options. See  O p                                         |

<!-- image -->

## Caution

TEM does not currently support parametrized circuits. The parameters argument should be set to None if precision is specified.

<!-- image -->

Caution

TEM currently supports circuits with linear connectivity. This restriction will be removed in future versions.

## Options

A dictionary containing the advanced options for the TEM. The dictionary may contain the keys in the following table. If any of the options are not provided, the default value listed in the table will be used. The default values are good for typical use of TEM.

| Name                   | Choices     | Description                                                                                                                                            | D e   |
|------------------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| max_bond_dimension     | int         | bond dimension to be used for the tensor networks.                                                                                                     | 5 0   |
| tem_compression_cutoff | float       | The cutoff value to be used for the tensor networks.                                                                                                   | 1 e   |
| max_execution_time     | int or None | The maximum execution time on the QPU in seconds. If the runtime exceeds this value, the job will be canceled. If None , a default limit set by Qiskit | N o   |
| num_randomizations     | int         | The number of randomizations to be used for gate twirling.                                                                                             | 3 2   |
| mitigate_readout_error | bool        | indicating whether to perform readout error mitigation or not.                                                                                         | Tr    |

| num_readout_calibration_shots   | int   | shots to be used for readout error mitigation.                              | 1 0   |
|---------------------------------|-------|-----------------------------------------------------------------------------|-------|
| default_precision               | float | precision to be used for the PUBs for which the precision is not specified. | 0. 0  |

## Outputs

A Qiskit PrimitiveResults containing the TEM-mitigated result. The result for each PUB is returned as a PubResult containing the following fields:

| Name       | Type    | Description                                                                                                                                                                                                                                                                                                                                           |
|------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| data       | DataBin | observable and its standard error. The DataBi following fields: ev s : The TEM-mitigated observable value. stds : The standard error of the TEM-mitigat observable.                                                                                                                                                                                   |
| metadadata | dict    | "stds_non_mitigated" : The standard erro result without error mitigation. "evs_mitigated_no_readout_mitigation observable value with error mitigation but wit readout error mitigation. "stds_mitigated_no_readout_mitigation standard error of the result with error mitigat without readout error mitigation. "evs_non_mitigated_with_readout_mitig |

## Fetching error messages

If your workload status is ERROR, use job.result() to fetch the error message as follows:

print(job.result())

## Get support

## Reach out to qiskit_ibm@algorithmiq .fi B

e sure to include the following information:

Qiskit Function Job ID ( qiskit-ibm-catalog ), job.job_id

A detailed description of the issue

Any relevant error messages or codes

Steps to reproduce the issue

## Next steps

<!-- image -->

Was this page helpful?

Report a bug or request content on GitHub .


<!-- image -->