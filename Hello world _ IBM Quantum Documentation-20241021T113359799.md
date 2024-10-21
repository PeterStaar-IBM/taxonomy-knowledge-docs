## Hello world

This example contains two parts. You will first create a simple quantum program and run it on a quantum processing unit (QPU). Because actual quantum research requires much more robust programs, in the second section (Scale to large numbers of qubits), you will scale the simple program up to utility level. You can also follow along with the Hello World episode of the Coding with Qiskit 1.0 video series.

Coding with Qiskit 1.x, Episode 3: Hello world


<!-- image -->

<!-- image -->

## Note

This video uses the Q iskitR untimeService.get_backend method, which has since been deprecated. Use

Q iskitR untimeService.backend instead.

## Before you begin

Follow the Install and set up instructions if you haven't already, including the steps to Set up to use IBM Quantum™ Platform.

It is recommended that you use the Jupyter development environment to interact with quantum computers. Be sure to install the recommended extra visualization support ( 'qiskit[visualization]' ). You'll also need the matplotlib package for the second part of this example.

To learn about quantum computing in general, visit the Basics of quantum information course in IBM Quantum Learning.

IBM® is committed to the responsible development of quantum computing. Learn more about responsible quantum at IBM and review our responsible quantum principles in the Responsible quantum computing and inclusive tech topic.

## Create and run a simple quantum program

The four steps to writing a quantum program using Qiskit patterns are:

1. Map the problem to a quantum-native format.

2. Optimize the circuits and operators.

3. Execute using a quantum primitive function.

4. Analyze the results.

## Step 1. Map the problem to a quantum-native format

In a quantum program, quantumcircuitsare the native format in which to represent quantum instructions, and operatorsrepresent the observables to be measured. When creating a circuit, you'll usually create a new Q uantumC ircuit object, then add instructions to it in sequence.

The following code cell creates a circuit that produces a Bellstate,which is a state wherein two qubits are fully entangled with each other.

## Note: bit ordering

The Qiskit SDK uses the LSb 0 bit numbering where the digit has value or . For more details, see the Bit-ordering in the Qiskit SDK topic. n th 1 ≪ n 2 n

1 fromqiskitimportQ uantumC ircuit 2 fromqiskit.quantum_infoimportSparsePauliO p 3 fromqiskit.transpiler.preset_passmanagersimportgene 4 fromqiskit_ibm_runtimeimportEstimatorV2 asEstimato 5 6 # C reateanewcircuitwithtwoqubits 7 qc=Q uantumC ircuit(2) 8 9 # AddaHadamardgatetoqubit0 1 0 qc.h(0) 1 1 1 2 # Performacontrolled-X gateonqubit1 , controlledb 1 3 qc.cx(0, 1 ) 1 4 1 5 # R eturnadrawingofthecircuitusingMatPlotLib("m 1 6 # lastlineofthecell, sothedrawingappearsinthe 1 7 # R emovethe"mpl" argumenttogetatextdrawing. 1 8 qc.draw("mpl")

Output:

<!-- image -->

See Q uantumC ircuit in the documentation for all available operations.

When creating quantum circuits, you must also consider what type of data you want returned after execution. Qiskit provides two ways to return data: you can obtain a probability distribution for a set of qubits you choose to measure, or you can obtain the expectation value of an observable. Prepare your workload to measure your circuit in one of these two ways with Qiskit primitives (explained in detail in Step 3).

This example measures expectation values by using the qiskit.quantum_info submodule, which is specified by using operators (mathematical objects used to represent an action or process that changes a quantum state). The following code cell creates six twoqubit Pauli operators: IZ , IX , ZI , XI , ZZ , and XX .

1 # Setupsixdifferentobservables. 2 fromqiskit.quantum_infoimportSparsePauliO p 3 4 observables_labels=["IZ", "IX", "ZI", "XI", "ZZ", "XX 5 observables=[SparsePauliO p(label) forlabelinobserv

## Operator Notation

Here, something like the ZZ operator is a shorthand for the tensor product , which means measuring Z on qubit 1 and Z on qubit 0 together, and obtaining information about the correlation between qubit 1 and qubit 0. Expectation values like this are also typically written as . Z ⊗ Z ⟨ Z Z ⟩ 1 0

If the state is entangled, then the measurement of should be 1. ⟨ Z Z ⟩ 1 0

## Step 2. Optimize the circuits and operators

When executing circuits on a device, it is important to optimize the set of instructions that the circuit contains and minimize the overall depth (roughly the number of instructions) of the circuit. This ensures that you obtain the best results possible by reducing the effects of error and noise. Additionally, the circuit's instructions must conform to a backend device's Instruction Set Architecture (ISA) and must consider the device's basis gates and qubit connectivity.

The following code instantiates a real device to submit a job to and transforms the circuit and observables to match that backend's ISA.

1 fromqiskit_ibm_runtimeimportQ iskitR untimeServic e 2 3 # Ifyoudidnotpreviouslysaveyourcredentials, use 4 # service=Q iskitR untimeService(channel="ibm_quantum" 5 service=Q iskitR untimeService() 6 7 backend=service.least_busy(simulator=False, operatio 8 9 # C onverttoanISAcircuitandlayout-mappedobservab 1 0 pm=generate_preset_pass_manager(backend=backend, opt 1 1 isa_circuit=pm.run(qc) 1 2 1 3 isa_circuit.draw('mpl', idle_wires=False)

Output:

<!-- image -->

## Step 3. Execute using the quantum primitives

Quantum computers can produce random results, so you usually collect a sample of the outputs by running the circuit many times. You can estimate the value of the observable by using the Estimator class. Estimator is one of two primitives; the other is Sampler , which can be used to get data from a quantum computer. These objects possess a run() method that executes the selection of circuits, observables, and parameters (if applicable), using a primitive unified bloc (PUB).

1 # C onstructtheEstimatorinstance. 2 fromqiskit_ibm_runtimeimportEstimatorV2 asEstimato 3 4 estimator=Estimator(mode=backend) 5 estimator.options.resilience_level=1 6 estimator.options.default_shots=5000 7 8 mapped_observables=[ 9 observable.apply_layout(isa_circuit.layout) forob 1 0 ] 1 1 1 2 # O nepub, withonecircuittorunagainstfivediffer 1 3 job=estimator.run([(isa_circuit, mapped_observables) 1 4 1 5 # UsethejobID toretrieveyourjobdatalater 1 6 print(f">>>JobID: {job.job_id()}")

Output:

>>>JobID: cva1 4t3sgfsg008g7a50

After a job is submitted, you can wait until either the job is completed within your current python instance, or use the job_id to retrieve the data at a later time. (See the section on retrieving jobs for details.)

After the job completes, examine its output through the job's result() attribute.

1 # Thisistheresultoftheentiresubmission. You s u b 2 # sothiscontainsoneinnerresult(andsomemetadata 3 job_result=job.result() 4 5 # Thisistheresultfromoursinglepub, whichhadsix 6 # socontainsinformationonallsix. 7 pub_result=job.result()[0]

## Alternative: run the example using a simulator

When you run your quantum program on a real device, your workload must wait in a queue before it runs. To save time, you can instead use the following code to run this small workload on the fake_provider with the Qiskit Runtime local testing mode. Note that this is only possible for a small circuit. When you scale up in the next section, you will need to use a real device.

1 2 # Usethefollowingcodeinsteadifyouwanttorun 3 4 fromqiskit_ibm_runtime.fake_providerimportFakeAl 5 backend=FakeAlmadenV2() 6 estimator=Estimator(backend) 7 8 # C onverttoanISAcircuitandlayout-mappedobser 9 1 0 pm=generate_preset_pass_manager(backend=backend, 1 1 isa_circuit=pm.run(qc) 1 2 mapped_observables=[ 1 3 observable.apply_layout(isa_circuit.layout) for 1 4 ] 1 5 1 6 job=estimator.run([(isa_circuit, mapped_observabl 1 7 result=job.result() 1 8 1 9 # Thisistheresultoftheentiresubmission. You 20 # sothiscontainsoneinnerresult(andsomemetad 21 22 job_result=job.result() 23 24 # Thisistheresultfromoursinglepub, whichhad 25 # socontainsinformationonallfive. 26 27 pub_result=job.result()[0]

## Step 4. Analyze the results

The analyze step is typically where you might postprocess your results using, for example, measurement error mitigation or zero noise extrapolation (ZNE). You might feed these results into another workflow for further analysis or prepare a plot of the key values and data. In general, this step is specific to your problem. For this example, plot each of the expectation values that were measured for our circuit.

The expectation values and standard deviations for the observables you specified to Estimator are accessed through the job result's PubR esult.data.evs and PubR esult.data.stds attributes. To obtain the results from Sampler, use the

PubR esult.data.meas.get_counts() function, which will return a dict of measurements in the form of bitstrings as keys and counts as their corresponding values. For more information, see Get started with Sampler.

1 # Plottheresult 2 3 frommatplotlibimportpyplotasplt 4 5 values=pub_result.data.evs 6 7 errors=pub_result.data.stds 8 9 # plottinggraph 1 0 plt.plot(observables_labels, values, '-o') 1 1 plt.xlabel('O bservables') 1 2 plt.ylabel('Values') 1 3 plt.show()

Output:

<!-- image -->

Notice that for qubits 0 and 1, the independent expectation values of both X and Z are 0, while the correlations ( XX and ZZ ) are 1. This is a hallmark of quantum entanglement.

## Scale to large numbers of qubits

In quantum computing, utility-scale work is crucial for making progress in the field. Such work requires computations to be done on a much larger scale; working with circuits that might use over 100 qubits and over 1000 gates. This example demonstrates how you can accomplish utility-scale work on IBM® QPUs by creating and analyzing a 100-qubit GHZ state. It uses the Qiskit patterns workflow and ends by measuring the expectation value for each qubit. ⟨ Z Z ⟩ 0 i

## Step 1. Map the problem

Write a function that returns a Q uantumC ircuit that prepares an -qubit GHZ state (essentially an extended Bell state), then use that function to prepare a 100-qubit GHZ state and collect the observables to be measured. n

1 fromqiskitimportQ uantumC ircuit 2 3 defget_qc_for_n_qubit_GHZ_state( n: int) ->Q uantumC ir 4 """Thisfunctionwillcreateaqiskit.Q uantumC ircu 5 6 Args: 7 n( int): Numberofqubitsinthen-qubitGHZ s 8 9 R eturns: 1 0 Q uantumC ircuit: Q uantumcircuitthatgenerate 1 1 """ 1 2 ifisinstance( n, int) andn>=2: 1 3 qc=Q uantumC ircuit( n) 1 4 qc.h( 0) 1 5 foriinrange( n-1 ): 1 6 qc.cx( i, i+1 ) 1 7 else: 1 8 raiseException( "nisnotavalidinput") 1 9 returnqc 20 21 # C reateanewcircuitwithtwoqubits( firstargument 22 # bits( secondargument) 23 n=1 00 24 qc=get_qc_for_n_qubit_GHZ_state( n)

Next, map to the operators of interest. This example uses the ZZ operators between qubits to examine the behavior as they get farther apart. Increasingly inaccurate (corrupted) expectation values between distant qubits would reveal the level of noise present.

Output: 1 fromqiskit.quantum_infoimportSparseP auliO p 2 3 # ZZII...II, ZIZI...II, ... , ZIII...IZ 4 operator_strings=['Z' + 'I'*i+ 'Z' + 'I'*( n-2-i) for 5 print( operator_strings) 6 print( len( operator_strings)) 7 8 operators=[SparseP auliO p( operator) foroperatorinop ['ZZIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII II I 99

Step 2. Optimize the problem for execution on quantum hardware

Transform the circuit and observables to match the backend's ISA.

1 fromqiskit.transpiler.preset_passmanagersimport ge n e 2 fromqiskit_ibm_runtimeimportQ iskitR untimeService 3 4 # Ifyoudidnotpreviouslysaveyourcredentials, use 5 # service=Q iskitR untimeService(channel="ibm_quantum" 6 service=Q iskitR untimeService() 7 8 backend=service.least_busy(simulator=False, operatio 9 pm=generate_preset_pass_manager(optimization_level=1 1 0 1 1 isa_circuit=pm.run(qc) 1 2 isa_operators_list=[op.apply_layout(isa_circuit.layo

## Step 3. Execute on hardware

Submit the job and enable error suppression by using a technique to reduce errors called dynamical decoupling. The resilience level specifies how much resilience to build against errors. Higher levels generate more accurate results, at the expense of longer processing times. For further explanation of the options set in the following code, see Configure error mitigation for Qiskit Runtime.

Output: 1 fromqiskit_ibm_runtimeimportEstimatorO ptions 2 fromqiskit_ibm_runtimeimportEstimatorV2 asEstimato 3 4 options=EstimatorO ptions() 5 options.resilience_level=1 6 options.dynamical_decoupling.enable=True 7 options.dynamical_decoupling.sequence_type="XY4" 8 9 # C reateanEstimatorobject 1 0 estimator=Estimator(backend, options=options) 1 # SubmitthecircuittoEstimator 2 job=estimator.run([(isa_circuit, isa_operators_list)] 3 job_id=job.job_id() 4 print(job_id) cva1 6pjemvv0008541 g0

## Step 4. Post-process results

After the job completes, plot the results and notice that decreases with increasing , even though in an ideal simulation all should be 1. ⟨ Z Z ⟩ 0 i i ⟨ Z Z ⟩ 0 i

1 importmatplotlib.pyplotasplt 2 importnumpyasnp 3 fromqiskit_ibm_runtimeimportQ iskitR untimeService 4 5 # data 6 data=list(range(1 , len(operators)+1 )) # Distancebet 7 result=job.result()[0] 8 values=result.data.evs# ExpectationvalueateachZ 9 values=[v/ values[0] forvinvalues] # Normalizet 1 0 1 1 # plottinggraph 1 2 plt.plot(data, values, marker='o', label='1 00-qubitGH 1 3 plt.xlabel('Distancebetweenqubits$i$') 1 4 plt.ylabel(r'$\langleZ_iZ_0 \rangle/ \langleZ_1 Z_ 1 5 plt.legend() 1 6 plt.show()

Output:

<!-- image -->

The previous plot shows that as the distance between qubits increases, the signal decays because of the presence of noise.

## Next steps

## Recommendations

Learn how to build circuits in more detail.

Try one of the workflow example tutorials.

Was this page helpful?

Report a bug or request content on GitHub .


<!-- image -->