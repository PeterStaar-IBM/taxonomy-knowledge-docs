## Configure the Qiskit SDK locally

After the Qiskit SDK is installed and running, there are some optional steps you can take to change the default Qiskit behavior.

## User configuration file

The main location for local configuration of Qiskit is the user configuration file. This is an .ini-format file that can be used to change Qiskit default settings.

## Example:

1 [default]

2 circuit_drawer=mpl

3 circuit_mpl_style=default

4 circuit_mpl_style_path=~:~/.qiskit

5 state_drawer=hinton

6 transpile_optimization_level=3

7 parallel=False

8 num_processes=1 5

By default, this file is in ~/.qiskit/settings.conf but the path can be overridden with the Q ISKIT_SETTINGS environment variable.

## Available options

circuit_drawer : Changes the default system for the circuit drawer. It can be set to latex , mpl , text , or latex_source . When the output kwarg is not explicitly set, this drawer system is used.

circuit_mpl_style : The default style sheet used for the mpl output system for the circuit drawer. Valid values are default or b w .

circuit_mpl_style_path : The paths to have the circuit drawer use to look for JSON style sheets when using the mpl output mode.

state_drawer : This is used to change the default system for the state visualization draw methods. Valid values are rep r , tex t , latex , latex_source , qsphere , hinton , or bloch . When the output kwarg is not explicitly set on the qiskit.quantum_info.DensityMatrix.draw method, the specified output method is used.

transpile_optimization_level : Change the default optimization level for qiskit.compiler.transpile. Specify an integer 0-3.

parallel : Whether Python multiprocessing is enabled for operations that support running in parallel. For example, transpilation of multiple qiskit.circuit.QuantumCircuit objects. This setting can be overridden by the Q ISK IT_P AR AL L EL environment variable. Specify a boolean value.

num_processes : The maximum number of parallel processes to launch for parallel operations if parallel execution is enabled. This setting can be overridden by the Q ISK IT_NUM_P R O C S environment variable. Specify an integer greater than 0 .

Note

Circuit drawer settings apply to qiskit.circuit.QuantumCircuit.draw and qiskit.visualization.circuit_drawer.

State visualization draw methods are qiskit.quantum_info.Statevector.draw and qiskit.quantum_info.DensityMatrix.draw.

## Environment variables

Set these environment variables to alter the default behavior of Qiskit:

Q ISK IT_ P AR AL L E L : Enables Python multiprocessing to parallelize certain operations; for example, transpilation over multiple circuits in Qiskit. Specify a boolean value.

Q ISK IT_ NUM_ P R O C S : The maximum number of parallel processes to launch for parallel operations if parallel execution is enabled. Specify an integer greater than zero.

R AYO N_ NUM_ THR E AD S : The number of threads to run multithreaded operations in Qiskit. By default, multithreaded code launches a thread for each logical CPU. To adjust the number of threads Qiskit uses, set this to an integer value. For example, setting RAYON_NUM_THREADS=4 launches four threads for multithreaded functions.

Q ISK IT_ FO R C E _ THR E AD S : Specifies that multithreaded code should always execute in multiple threads. By default, if you're running multithreaded code in a section of Qiskit that is already running in parallel processes, Qiskit does not launch multiple threads but instead executes that function serially. This is done to avoid potentially overloading limited CPU resources. However, if you want to force the use of multiple threads even when in a multiprocess context, set Q ISK IT_ FO R C E _ THR E AD S=TR UE .

Q ISK IT_ SABR E _ AL L _ THR E AD S : Controls the behavior of the layout and routing pass in Qiskit's preset pass manager. When set to 1 or TR UE , this uses all available CPUs for running multiple random trials. This can improve the quality of the results, especially for systems with greater than 20 CPUs/cores; the tradeoff, however, is that results are not reproducible when run on different local hardware.

## Next steps

## Recommendations

Learn how to build circuits.

Run the Hello world program.

Try a tutorial, such as Grover's algorithm .

Read the contributing guidelines if you want to contribute to the open-source Qiskit SDK.

Was this page helpful?

Yes

No

Report a bug or request content on GitHub .