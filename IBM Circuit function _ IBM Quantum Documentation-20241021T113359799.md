## IBM Circuit function

<!-- image -->

Note

Qiskit Functions are an experimental feature available only to IBM Quantum™ Premium Plan users. They are in preview release status and subject to change.

## Overview

<!-- image -->

The IBM® Circuit function takes abstract PUBs as inputs, and returns mitigated expectation values as outputs. This circuit function includes an automated and customized pipeline to enable researchers to focus on algorithm and application discovery.

## Description

After submitting your PUB, your abstract circuits and observables are automatically transpiled, executed on hardware, and post-processed to return mitigated expectation values. To do so, this combines the following tools:

Qiskit Transpiler Service, including auto-selection of AI-driven and heuristic transpilation passes to translate your abstract circuits to hardware-optimized ISA circuits

Error suppression and mitigation required for utility-scale computation, including measurement and gate twirling, dynamical decoupling, Twirled Readout Error eXtinction (TREX), Zero-Noise Extrapolation (ZNE), and Probabilistic Error Amplification (PEA)

Qiskit Runtime Estimator, to execute ISA PUBs on hardware and return mitigated expectation values

<!-- image -->

<!-- image -->

## Get started

Authenticate using your IBM Quantum Platform API token , and select the Qiskit Function as follows:

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC atal og 2 3 catalog=Q iskitFunctionsC atalog() 4 function=catalog.load("ibm/circuit-function")

## Example

To get started, try this basic example:

1 fromqiskit.circuit.randomimportrandom_circuit 2 fromqiskit_ibm_runtimeimportQ iskitR untimeService 3 4 # Youcanskipthisstepifyouhaveatargetbackend, 5 # backend_name="ibm_sherbrooke" 6 # You'llneedtospecifythecredentialswheninitiali 7 service=Q iskitR untimeService() 8 backend=service.least_busy(operational=True, simulat 9 1 0 circuit=random_circuit(num_qubits=2, depth=2, seed=4 1 1 observable="Z" * circuit.num_qubits 1 2 pubs=[(circuit, observable)] 1 3 1 4 job=function.run( 1 5 # Use'backend_name=backend_name'ifyoudidn'tinit 1 6 backend_name=backend.name, 1 7 pubs=pubs 1 8 )

Check your Qiskit Function workload's status or return results as follows:

Output: 1 print(job.status()) 2 result=job.result() Q UEUED

The results have the same format as an Estimator result :

1 print(f'Theresultofthesubmittedjobhad{len(re su l t 2 print(f'TheassociatedPubR esultofthisjobhasthefo 3 print(f'AndthisDataBinhasattributes: {result[0].dat 4 print(f'TheexpectationvaluesmeasuredfromthisPUBa

Output:

Theresultofthesubmittedjobhad1 PUB

TheassociatedPubR esultofthisjobhasthefollowingData DataBin(evs=np.ndarray(<shape=(), dtype=float64>), stds=np

AndthisDataBinhasattributes: dict_keys(['evs', 'stds', TheexpectationvaluesmeasuredfromthisPUBare: 0.07302231 23732251 5

## Inputs

See the following table for all input parameters the IBM Circuit function accepts. The subsequent Options section goes into more details about the available options .

| Name         | Type                       | Description                           |
|--------------|----------------------------|---------------------------------------|
| backend_name | str                        | Name of the backend t                 |
| pubs         | Iterable[EstimatorPubLike] | (circuit, observab (circuit, observab |
| options      | dict                       | Input options. See the  details.      |
| instance     | str                        | The hub/group/project                 |

## Options

## Structure

Similar to Qiskit Runtime primitives, options for IBM Circuit function can be specified as a nested dictionary. Commonly used options, such as optimization_level and mitigation_level , are at the first level. Other, more advanced options are grouped into different categories, such as resilience .

## Defaults

If you do not specify a value for an option, the default value specified by the service is used.

## Mitigation level

IBM Circuit function also supports mitigation_level . The mitigation level specifies how much error suppression and mitigation to apply to the job. Higher levels generate more accurate results, at the expense of longer processing times. The degree of error reduction depends on the method applied. The mitigation level abstracts the detailed choice of error mitigation and suppression methods to allow users to reason about the cost/accuracy trade-off appropriate to their application. The following table shows the corresponding methods for each level.

<!-- image -->

## Note

While the names are similar, mitigation_level applies different techniques than the ones used by Estimator's resilience_level .

Similar to resilience_level in Qiskit Runtime Estimator, mitigation_level specifies a base set of curated options. Any options you manually specify in addition to the mitigation level are applied on top of the base set of options defined by the mitigation level. Therefore, in principle, you could set the mitigation level to 1, but then turn off measurement mitigation, although this is not advised.

| Mitigation Level   | Technique                                          |
|--------------------|----------------------------------------------------|
| 1 [Default]        | Dynamical decoupling + measurement twirling + TREX |
| 2                  | Level 1 + gate twirling + ZNE via gate folding     |
| 3                  | Level 1 + gate twirling + ZNE via PEA              |

The following example demonstrates setting the mitigation level:

1 options={"mitigation_level": 2} 2 3 job=function.run( 4 backend_name=backend.name, 5 pubs=pubs, 6 options=options 7 )

## All available options

In addition to mitigation_level , the IBM Circuit function also provides a select number of advanced options that allow you to fine-tune the cost/accuracy trade-off. The following table shows all the available options:

| Option               | Sub-option         | Sub-sub-option   | Desc         |
|----------------------|--------------------|------------------|--------------|
|                      |                    |                  | The  call  t |
| default_precision    | default_precision  |                  | Eac h        |
|                      |                    |                  | give n       |
|                      |                    |                  | call  t      |
|                      |                    |                  | Max on  Q    |
| max_execution_time   | max_execution_time |                  | amo          |
|                      |                    |                  | job  e       |
|                      |                    |                  | How          |
| mitigation_level     |                    |                  | Mit ig       |
|                      |                    |                  | at e a       |
|                      |                    |                  | How          |
| optimization_level   |                    |                  | gen e        |
|                      |                    |                  | tran s       |
| dynamical_decoupling | enable             |                  | Whe          |
|                      |                    |                  | and          |
|                      |                    |                  | Wh ic *  XX  |
|                      | sequence_type      |                  | X            |
|                      |                    |                  | *  p         |
|                      |                    |                  | *  XY        |
|                      |                    |                  | ta u         |
| twirling             | enable_gates       |                  | Whe          |
|                      | enable_measure     |                  | Whe          |
| resilience           | measure_mitigation |                  | to E r       |
|                      |                    |                  | the  m       |
|                      |                    |                  | Whe          |
|                      | zne_mitigation     |                  | Ref e        |
|                      |                    |                  | expl         |

Whi

|                | Wh ic IBM Circuit function | IBM Quantum Documentation   |
|----------------|----------------------------------------------------------|
|                | -  g a                                                   |
|                | If th thes                                               |
|                | -  g a                                                   |
|                | the  n                                                   |
|                | thes                                                     |
| zne            | amplifier DAG                                            |
|                | Nois doe s                                               |
| extrapolator   |                                                          |
|                | the  n                                                   |
|                | thes DAG                                                 |
|                | -  p e                                                   |
| pec_mitigation | to a m                                                   |

In the following example, setting the mitigation level to 1 initially turns off ZNE mitigation, but setting zne_mitigation to True overrides the relevent setup from mitigation_level .

1 options={ 2 "mitigation_level": 1 , 3 "resilience": {"zne_mitigation": True} 4 }

## Outputs

The output of a Circuit function is a PrimitiveResult, which contains tw ofie lds:

One or more PubResult objects. These can be indexed directly from the PrimitiveR esult .

Job-level metadata.

Each PubR esult contains a data and a metadata field.

The data field contains at least an array of expectation values ( PubR esult.data.evs ) and an array of standard errors ( PubR esult.data.stds ). It can also contain more data, depending on the options used.

The metadata field contains PUB-level metadata ( PubR esult.metadata ).

The following code snippet describes the PrimitiveR esult (and associated PubR esult ) format.

Output: 1 print(f'Theresultofthesubmittedjobhad{len(re su l t 2 print(f'TheexpectationvaluesmeasuredfromthisPUBa 3 print(f'Andtheassociatedmetadatais:  {result[0].me Theresultofthesubmittedjobhad1 PUB TheexpectationvaluesmeasuredfromthisPUBare: 0.07302231 23732251 5 Andtheassociatedmetadatais: {'shots': 4096, 'target_precision': 0.01 5625, 'circuit_meta

## Fetching error messages

If your workload status is ER R O R , use job.result( ) to fetch the error message to help debug as follows:

Output: 1 job=function.run( 2 backend_name="bad_backend_name", 3 pubs=pubs, 4 options=options 5 ) 6 7 print( job.result( )) Traceback( mostrecentcalllast): File"/ runner/ runner.py", line1 0, inrun func=C ircuitFunction( **arguments) ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ File"/ runner/ circuit_function/ circuit_function.py", line self._backend=self._service.backend( ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ File"/ usr/ local/ lib/ python3.1 1 / site-packages/ qiskit_ibm_ backends=self.backends( name, instance=instance, use_f ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ File"/ usr/ local/ lib/ python3.1 1 / site-packages/ qiskit_ibm_ raiseQ iskitBackendNotFoundError( "Nobackendmatchesth qiskit.providers.exceptions.Q iskitBackendNotFoundError: 'No

## Get support

Reach out to IBM Quantum support , and include the following information:

Qiskit Function Job ID ( qiskit-ibm-catalog ), job.job_id

A detailed description of the issue

Any relevant error messages or codes

Steps to reproduce the issue

## Next steps

## Recommendations

Try the Building workflows with the IBM Circuit function tutorial.

## Was this page helpful?

Report a bug or request content on GitHub .


<!-- image -->