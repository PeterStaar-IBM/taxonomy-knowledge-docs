## Introduction to Qiskit Functions

<!-- image -->

Note

Qiskit Functions are an experimental feature available only to IBM Quantum™ Premium Plan users. They are in preview release status and subject to change.

Qiskit Functions simplify and accelerate utility-scale algorithm discovery and application development, by abstracting away parts of the quantum software development workflow. In this way, Qiskit Functions free up time normally spent hand-writing code and fine-tuning experiments.

<!-- image -->

<!-- image -->

Functions come in two forms:

| Type                 | What does it do?                                                                                                                     | Example inputs and outputs                                     | Who is it for?                                                                                                                                                                |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Circuit function     | Simplified interface for running circuits. Abstracts transpilation, error suppression, error mitigation                              | Input : Abstract P U B s Output : Mitigated expectation values | Researchers using Qiskit to discover new algorithms and applications, without needing to focus on optimizing for hardware or handling error. Circuit functions can be used to |
| Application function | Covers higher- level tasks, like exploring algorithms and domain-specific use cases. Abstracts quantum workflow to solve tasks, with | Input : Molecules, graphs Output : Energy, cost                | Researchers in non-quantum domains, integrating quantum into existing large- scale classical workflows, without needing to map classical                                      |

Functions are provided by IBM® and third-party partners. Each is performant for specific workload characteristics and have unique performance-tuning options. Premium Plan users can get started with IBM Qiskit Functions for free, or procure a license from one of the partners who have contributed a function to the catalog.

## Get started with Qiskit Functions

## Install Qiskit Functions Catalog client

To start using Qiskit Functions, install the IBM Qiskit Functions Catalog client:

pipinstallqiskit-ibm-catalog

Once installed, authenticate using your IBM Quantum Platform API token as follows:

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC atal og 2 3 catalog=Q iskitFunctionsC atalog(token='<YO UR _IQ P_API_T

Optionally, you can use the save_account() method to save your credentials for easy access later on, before initializing the service. This saves your credentials in the same place as

Q iskitR untimeService.save_account() , so you can skip this step if you had previously used Q iskitR untimeService to save your account. Notethataccountcredentialsaresavedinplaintext,soonlydosoifyou areusingatrusteddevice.

1 fromqiskit_ibm_catalogimportQ iskitFunctionsC atal og 2 3 # ThiscanbeskippedifyoupreviouslydidQ iskitR unti 4 Q iskitFunctionsC atalog.save_account(token="<YO UR _IQ P_AP

Once you have authenticated, you can list the functions from the Qiskit Functions Catalog that you have access to:

Output: catalog.list() [Q iskitFunction(ibm/circuit-function)]

## Run enabled functions

Once a catalog object has been instantiated, you can select a function using catalog.load(provider/function-name) :

ibm_cf=catalog.load( 'ibm/ circuit-function')

Each Qiskit Function has custom inputs, options, and outputs. Check the specific documentation pages for the function you want to run for more information. By default, all users can only run one function job at a time:

1 job=ibm_cf.run( 2 instance=..., 3 pubs=[], 4 backend="backend_name" 5 ) 6 7 job.job_id 0ede4d0f-06d6-4360-86a1 -5028ca51 1 b3f

## Check job status

Tip

Currently, the IBM Quantum workloads table only reflects Qiskit Runtime workloads. Use job.status( ) to see your Qiskit Function workload's current status.

With your Qiskit Function job_id , you can check the status of running jobs. This includes the following statuses:

Q UEUED : The remote program is in the Qiskit Function queue. The queue priority is based on how much you've used Qiskit Functions.

INITIAL IZING : The remote program is starting; this includes setting up the remote environment and installing dependencies.

R UNNING : The program is running.

D O NE : The program is complete, and you can retrieve result data with job.results( ) .

ER R O R : The program stopped running because of a problem. Use job.result( ) to get the error message.

C ANC EL ED : The program was canceled; either by a user, the service, or the server.

Output: job.status() 'DO NE'

## Retrieve results

Once a program is DO NE , you can use job.results() to fetch the result. This output format varies with each function, so be sure to follow the specific documentation:

Output: 1 result=job.result() 2 print(result)

PubR esult(data=DataBin(evs=np.ndarray(<shape=(20,), dtype=f 'exponential', 'exponential', 'exponential', 'expone 'exponential', 'exponential', 'exponential', 'expone 'exponential', 'exponential', 'exponential', 'expone 'exponential', 'exponential', 'exponential', 'expone dtype='<U1 1 ')}, 'layer_noise': {'noise_overhead': 1 ,

At any time, you can also cancel a job:

Output: job.stop() 'Jobhasbeenstopped.'

List previously run jobs run with Qiskit Functions

You can use jobs() to list all jobs submitted to Qiskit Functions:

1

old_jobs=catalog.jobs()

2

old_jobs

Output:

[<J ob| 0ede4d0f-06d6-4360-86a1 -5028ca51 1 b3f>, <J ob| abf78e9a-b554-4e38-966a-f99cff877b8c>, <J ob| 90e1 1 09e-809f-4768-a2dc-f45bf71 a97b4>, <J ob| 31 3050f2-aa78-4d7d-99f4-44bdfe98e4d7>]

## Fetch error messages

If a program status is ER R O R , use job.result( ) to fetch the error message to help debug as follows:

print( job.result( ))

## Next steps

## Recommendations

Explore circuit functions to build new algorithms and applications, without needing to manage transpilation or error handling.

Explore application functions to solve domain-specific tasks, with classical inputs and outputs.

<!-- image -->

Report a bug or request content on GitHub .