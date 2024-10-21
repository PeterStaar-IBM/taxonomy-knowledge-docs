## Performance Management: A Qiskit Function by Q-CTRL Fire Opal

Note

Qiskit Functions are an experimental feature available only to IBM Quantum™ Premium Plan users. They are in preview release status and subject to change.

## Overview

Fire Opal Performance Management makes it simple for anyone to achieve meaningful results from quantum computers at scale without needing to be quantum hardware experts. When running circuits with Fire Opal Performance Management, AI-driven error suppression techniques are automatically applied, enabling the scaling of larger problems with more gates and qubits. This approach reduces the number of shots required to reach the correct answer, with no added overhead - resulting in significant savings in both compute time and cost.

Performance Management suppresses errors and increases the probability of getting the correct answer on noisy hardware. In other words, it increases the signal-to-noise ratio. The following image shows how increased accuracy enabled by Performance Management can reduce the need for additional shots in the case of a 10-qubit Quantum Fourier Transform algorithm. With only 30 shots, Q-CTRL reaches the 99% confidence threshold, whereas the default ( Q iskitR untime Sampler, optimization_level =3 and resilience_level =1, ibm_sherbrooke ) requires 170,000 shots. By getting the right answer faster, you save significant compute runtime.

<!-- image -->

The Performance Management function can be used with any algorithm, and you can easily use it in place of the standard Qiskit Runtime primitives. Behind the scenes, multiple error suppression techniques work together to prevent errors from happening at runtime. All Fire Opal pipeline methods are pre-configured and algorithm-agnostic, meaning you always get the best performance out of the box.

To get access to Performance Management, contact Q-CTRL .

## Description

Fire Opal Performance Management has two options for execution that are similar to the Qiskit Runtime primitives, so you can easily swap in the Q-CTRL Sampler and Estimator. The general workflow for using the Performance Management function is:

1. Define your circuit (and operators in the case of the Estimator).

2. Run the circuit.

3. Retrieve the results.

To reduce hardware noise, Fire Opal employs a range of AI-driven error suppression techniques depicted in the following image. With Fire Opal, the entire pipeline is completely automated with zero need for configuration.

Fire Opal's pipeline eliminates the need for additional overhead, such as increased quantum runtime or extra physical qubits. Note that classical processing time remains a factor (refer to the Benchmarks section for estimates, where "Total time" reflects both classical and quantum processing). In contrast to error mitigation, which requires overhead in the form of sampling, Fire Opal's error suppression works at both the gate and pulse levels to address various sources of noise and to prevent the likelihood of an error occurring. By preventing errors, the need for expensive post-processing is eliminated.

The following image depicts the error suppression methods automated by Fire Opal Performance Management.

<!-- image -->

The function offers two primitives, Sampler and Estimator, and the inputs and outputs of both extend the implemented spec for Qiskit Runtime V2 primitives.

## Benchmarks

Published algorithmic benchmarking results demonstrate significant performance improvement across various algorithms, including Bernstein-Vazirani, quantum Fourier transform, Grover's search, quantum approximate optimization algorithm, and variational quantum eigensolver. The rest of this section provides more details about types of algorithms you can run, as well as the expected performance and runtimes.

The following independent studies demonstrate how Q-CTRL's Performance Management enables algorithmic research at recordbreaking scale:

Parametrized Energy-Efficient Quantum Kernels for Network Service Fault Diagnosis - up to 50-qubit quantum kernel learning

Tensor-based quantum phase difference estimation for large-scale demonstration - up to 33-qubit quantum phase estimation

Hierarchical Learning for Quantum ML: Novel Training Technique for Large-Scale Variational Quantum Circuits - up to 21-qubit quantum data loading

The following table provides a rough guide on accuracy and runtimes from prior benchmarking runs on ibm_fez . Performance on other devices may vary. The usage time is based on an assumption of 10,000 shots per circuit. The "Number of qubits" indicated is not a hard limitation but represents rough thresholds where you can expect extremely consistent solution accuracy. Larger problem sizes have been successfully solved, and testing beyond these limits is encouraged.

| Example                                                   | Number of qubits   | Accuracy   | Measure of accuracy                                                          |
|-----------------------------------------------------------|--------------------|------------|------------------------------------------------------------------------------|
| Bernstein- Vazirani                                       | 50Q                | 100%       | Success Rate (Percentage of run correct answer is the highest cou bitstring) |
| Quantum Fourier Transform                                 | 30Q                | 100%       | Success Rate (Percentage of run correct answer is the highest cou bitstring) |
| Quantum Phase Estimation                                  | 30Q                | 99.9998%   | Accuracy of the angle found: 1 - abs( real_angle- angle_                     |
| Quantum simulation: Ising model (15                       | 20Q                | 99.775%    | (defined below) A                                                            |
| Quantum simulation 2: molecular dynamics (20 time points) | 34Q                | 96.78%     | (defined below) A mean                                                       |

Defining the accuracy of the measurement of an expectation value - the metric is defined as follows: A

A = 1 - , ϵ - ϵ max ideal min ideal ∣ ϵ - ϵ ∣ ideal meas

where = ideal expectation value, = measured expectation value, = ideal maximum value, and = ideal minimum value. is simply the average of the value of across multiple measurements. ϵ ideal ϵ meas ϵ max ideal ϵ min ideal A mean A

This metric is used because it is invariant to global shifts and scaling in the range of attainable values. In other words, regardless of whether you

shift the range of possible expectation values higher or lower or increase the spread, the value of should remain consistent. A

## Get started

Authenticate using your IBM Quantum Platform API token , and select the Qiskit Function as follows:

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC ata lo g 2 3 # C redentials 4 token="<YO UR _IQ P _AP I_TO K EN>" 5 hub="<YO UR _IQ P _HUB>" 6 group="<YO UR _IQ P _GR O UP >" 7 project="<YO UR _IQ P _P R O J EC T>" 8 9 # Authentication 1 0 catalog=Q iskitFunctionsC atalog( token=token) 1 1 1 2 # AccessFunction 1 3 perf_mgmt=catalog.load( "q-ctrl/ performance-managemen

## Estimator primitive

## Estimator example

Use Fire Opal Performance Management's Estimator primitive to determine the expectation value of a single circuit-observable pair.

In addition to the qiskit-ibm-catalog and qiskit packages, you will also use the numpy package to run this example. You can install this package by uncommenting the following cell if you are running this example in a notebook using the IPython kernel.

# % pipinstallnumpy

## 1. Create the circuit

As an example, generate a random Hermitian operator and an observable to input to the Performance Management function.

2. Run the circuit Run the circuit and optionally define the backend and number of shots. You can use the familiar Qiskit Serverless APIs to check your Qiskit Function workload's status: Output: 3. Retrieve the result 1 importnumpyasnp 2 fromqiskit.circuit.libraryimportIQ P 3 fromqiskit.quantum_infoimportrandom_hermitian, Spar 4 5 n_qubits=50 6 7 # Generatearandomcircuit 8 mat=np.real(random_hermitian(n_qubits, seed=1 234)) 9 circuit=IQ P(mat) 1 0 circuit.measure_all() 1 1 1 2 # Defineobservablesasastring 1 3 observable=SparsePauliO p("Z" * n_qubits) 1 # C reatePUBtuple 2 estimator_pubs=[(circuit, observable)] 1 # C hooseabackendorremovethisoptiontodefaul t t o 2 backend_name="<C HO O SE_A_BAC KEND>" 3 4 # R unthecircuitusingthesampler 5 qctrl_estimator_job=perf_mgmt.run( 6 primitive="estimator", 7 pubs=estimator_pubs, 8 instance=hub+ "/" + group+ "/" + project, 9 backend_name=backend_name, 1 0 ) qctrl_estimator_job.status() 'DO NE' 1 # R etrievethecountsfromtheresultlist 2 result=qctrl_estimator_job.result()

The results have the same format as an Estimator result:

1 print( 2 f"Theresultofthesubmittedjobhad{len(result)} 3 ) 4 print( 5 f"TheassociatedPubR esultofthisjobhasthefoll 6 ) 7 print(f"AndthisDataBinhasattributes: {result[0].dat 8 print(f"TheexpectationvaluesmeasuredfromthisPUBa

Output:

Theresultofthesubmittedjobhad1 PUBandhasavalu e: PrimitiveR esult([PubR esult(data=DataBin(evs=np.ndarray(<sh

TheassociatedPubR esultofthisjobhasthefollowingData DataBin(evs=np.ndarray(<shape=(1 ,), dtype=float64>), stds=

AndthisDataBinhasattributes: dict_keys(['evs', 'stds']) TheexpectationvaluesmeasuredfromthisPUBare: [-0.01 464844]

## Estimator inputs

<!-- image -->

## Caution

Fire Opal Performance Management accepts abstract circuits, in contrast to the native Qiskit Runtime primitives, which only accept circuits that are written in the target backend's Instruction Set Architecture (ISA). For best results, do not transpile circuits before submitting via the Performance Management function.

| Name         | Type                                                | Description                                      |
|--------------|-----------------------------------------------------|--------------------------------------------------|
| pubs         | Q ctrlEstimatorPubLike list[Q ctrlEstimatorPubLike] | or containing the inputs listed un EstimatorPu b |
| instance     | st r                                                | The hub/group/proj to use in that fo             |
| backend_name | st r                                                | The name of th e backend                         |
| options      | dict                                                | Input options; s Options section more details    |

Q ctrlEstimatorPubLike components (derived from the Qiskit Runtime PUB definition):

A single circuit defined as a Q uantumC ircuit or in OpenQASM 2.0 or 3.0 string format.

One or more observables that specify the expectation values to estimate, in any of the formats denoted in the list "Supported observables formats".

(Optional) A collection of parameter values to bind the circuit against, which follow the same array broadcasting rules as the Q iskitR untime primitives.

(Optional) A target precision for expectation values to estimate.

(Optional) A real number representing the precision, or a dictionary of run options containing the shot count. For example: {"shots": <int>} .

## Supported observables formats:

Any one of the O bservablesArrayL ike formats, such as P auli , SparseP auliO p , P auliL ist , or st r

A Pauli string: "XY"

A dictionary - Pauli strings with coefficients: {"XY": 0.5, "YZ": 0.3}

A list of Pauli strings: ["XY", "YZ", "ZX"]

A list of Pauli strings with coefficients: [( "XY", 0.5), ( "YZ", 0.3)]

A nested list of Pauli strings: [["XY", "YZ"], ["ZX", "XX"]]

A nested list of Pauli strings with coefficients: [[( "XY", 0.1 ), ( "YZ", 0.2)], [( "ZX", 0.3), ( "XX", 0.4)]]

Supported backends: The following list of backends are currently supported. If your device is not listed, reach out to Q-CTRL to add support.

ibm_brisbane

ibm_brussels

ibm_cleveland

ibm_fez

ibm_kawasaki

ibm_kyiv

ibm_nazca

ibm_quebec

ibm_rensselaer

ibm_sherbrooke

ibm_strasbourg

ibm_torino

## Options:

| Name              | Type   | Description                                                   | Default               |
|-------------------|--------|---------------------------------------------------------------|-----------------------|
| session_id        | st r   | An existing Qiskit Runtime session ID                         | "cw4r3je6f0t01 0870y3 |
| default_shots     | in t   | The number of shots to use for each circuit                   | 2048                  |
| default_precision | float  | precision for expectation values to estimate for each circuit | 0.01 5625             |

## Estimator outputs

| Name   | Type             | Description      | Example          |
|--------|------------------|------------------|------------------|
| N/A    | PrimitiveR esult | corresponding to | PubR esult(dat a |

## Sampler primitive

## Sampler example

Use Fire Opal Performance Management's Sampler primitive to run a Bernstein-Vazirani circuit. This algorithm, used to find a hidden string from the outputs of a black box function, is a common benchmarking algorithm because there is a single correct answer.

## 1. Create the circuit

Define the correct answer to the algorithm, the hidden bitstring, and the Bernstein-Vazirani circuit. You can adjust the width of the circuit by simply changing the circuit_width .

1 importqiskit 2 3 circuit_width=35 4 hidden_bitstring="1 " * circuit_width 5 6 # C reatecircuit, reservingonequbitforBV oracle 7 bv_circuit=qiskit.Q uantumC ircuit(circuit_width+ 1 , 8 bv_circuit.x(circuit_width) 9 bv_circuit.h(range(circuit_width+ 1 )) 1 0 forinput_qubit, bitinenumerate(reversed(hidden_bits 1 1 ifbit=="1 ": 1 2 bv_circuit.cx(input_qubit, circuit_width) 1 3 bv_circuit.barrier() 1 4 bv_circuit.h(range(circuit_width+ 1 )) 1 5 bv_circuit.barrier() 1 6 forinput_qubitinrange(circuit_width): 1 7 bv_circuit.measure(input_qubit, input_qubit) 1 8 1 9 # C reatePUBtuple 20 sampler_pubs=[(bv_circuit,)]

## 2. Run the circuit

Run the circuit and optionally define the backend and number of shots.

t o

1 # C hooseabackendorremovethisoptiontodefaul t 2 backend_name="<C HO O SE_A_BAC KEND>" 3 4 # R unthecircuitusingthesampler 5 qctrl_sampler_job=perf_mgmt.run( 6 primitive="sampler", 7 pubs=sampler_pubs, 8 instance=hub+ "/" + group+ "/" + project, 9 backend_name=backend_name, 1 0 )

Check your Qiskit Function workload's status or return results as follows:

Output: qctrl_sampler_job.status() DO NE

## 3. Retrieve the result

1 # R etrievethejobresults 2 sampler_result=qctrl_sampler_job.result() 1 # Getresultsforthefirst(andonly) PUB 2 pub_result=sampler_result[0] 3 counts=pub_result.data.c.get_counts() 4 5 print(f"C ountsforthemeasoutputregister: {counts}") C ountsforthemeasoutputregister: {'000000000000000000 00

## 3. Plot the top bitstrings

Plot the bitstring with the highest counts to see if the hidden bitstring was the mode.

1 importmatplotlib.pyplotasplt 2 3 defplot_top_bitstrings(counts_dict, hidden_bitstring= 4 # Sortandtakethetop1 00 bitstrings 5 top_1 00 =sorted(counts_dict.items(), key=lambdax 6 ifnottop_1 00: 7 print("Nobitstringsfoundintheinputdictio 8 return 9 1 0 # Unzipthebitstringsandtheircounts 1 1 bitstrings, counts=zip(*top_1 00) 1 2 1 3 # Assigncolors: purpleifthebitstringmatchesh 1 4 colors=['#680C E9' ifbit==hidden_bitstringels 1 5 1 6 # C reatethebarplot 1 7 plt.figure(figsize=(1 5, 8)) 1 8 plt.bar(range(len(bitstrings)), counts, tick_label 1 9 20 # R otatethebitstringsforbetterreadability 21 plt.xticks(rotation=90, fontsize=8) 22 plt.xlabel('Bitstrings') 23 plt.ylabel('C ounts') 24 plt.title('Top1 00 BitstringsbyC ounts') 25 26 # Showtheplot 27 plt.tight_layout() 28 plt.show()

The hidden bitstring is highlighted in purple, and it should be the bitstring with the highest number of counts.

plot_top_bitstrings(counts, hidden_bitstring)

<!-- image -->

## Sampler inputs

<!-- image -->

## Caution

Fire Opal Performance Management accepts abstract circuits, in contrast to the native Qiskit Runtime primitives, which only accept circuits that are written in the target backend's Instruction Set Architecture (ISA). For best results, do not transpile circuits before submitting via the Performance Management function.

| Name         | Type                                                | Description                                         |
|--------------|-----------------------------------------------------|-----------------------------------------------------|
| pubs         | Q ctrlSamplerPubLike  or list[Q ctrlSamplerPubLike] | the inputs listed under                             |
| instance     | st r                                                | The hub/group/project to use in that format         |
| backend_name | st r                                                | The name of the backend                             |
| options      | dict                                                | Input options; see Options section for more details |

Q ctrlSamplerPubLike components (derived from the Qiskit Runtime PUB definition):

A single circuit defined as a Q uantumC ircuit or in OpenQASM 2.0 or 3.0 string format.

(Optional) A collection of parameter values to bind the circuit against.

(Optional) An integer representing the shot count, or a dictionary of runtime options containing the shot count. For example: ( circ, None, 1 23) or ( circ, None, {"shots": 1 23}) .

Supported backends: The following list of backends are currently supported. If your device is not listed, reach out to Q-CTRL to add support.

ibm_brisbane

ibm_brussels

ibm_cleveland

ibm_fez

ibm_kawasaki

ibm_kyiv

ibm_nazca

ibm_quebec

ibm_rensselaer

ibm_sherbrooke

ibm_strasbourg

ibm_torino

## Options:

| Name          | Type   | Description                            | Default   |
|---------------|--------|----------------------------------------|-----------|
| session_id    | st r   | Qiskit Runtime "cw4r3je6f0t01 0870y3g" |           |
| default_shots | in t   | of shots to use for each 2048          |           |

## Sampler outputs

| Name   | Type             | Description            | Example          |
|--------|------------------|------------------------|------------------|
| N/A    | PrimitiveR esult | corresponding to       | PrimitiveR esu l |
| N/A    | PrimitiveR esult | the list of input PUBs | PrimitiveR esu l |

## Get support

For any questions or issues, contact Q-CTRL .

## Next steps

Recommendations

Request access to Q-CTRL Performance Management

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .