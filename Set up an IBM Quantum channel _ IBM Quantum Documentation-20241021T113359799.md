## Set up an IBM Quantum channel

You can access IBM® quantum processing units (QPUs) by using the IBM Quantum Platform or IBM Cloud® channel. Channelis the term used to describe the method you use to access IBM Quantum services.

## Select an IBM Quantum channel

You have the option to access IBM Quantum hardware through either IBM Quantum Platform or IBM Cloud.

## IBM Quantum Platform

IBM Quantum Platform has Open (free access) and Premium (enterprise subscription) plans. See IBM Quantum access plans for details.

Before setting up with IBM Quantum Platform, make sure you have the Qiskit SDK and Qiskit Runtime installed.

## Available plans:

Open Plan - Run your quantum circuits on the world's best QPUs for free (up to 10 minutes quantum time per month).

Premium Plan - Run quantum circuits on the world's best QPUs using an enterprise quantum time subscription.

## IBM Cloud

IBM Cloud offers pay-as-you-go access plans. See IBM Quantum access plans for details.

IBM Cloud has Lite (free access) and Standard (pay-as-you-go access) plans. See Qiskit Runtime plans on IBM Cloud for details.

This channel does not support a cloud-based development environment. Therefore, you will need to install and set up Qiskit and Qiskit Runtime and set up to use IBM Cloud.

## Available plans:

Standard (Pay-as-you-go) Plan - Run quantum circuits on the world's best QPUs and pay only for the quantum time you use.

Lite plan : Debug and learn about quantum circuits using free simulators.

## Set up to use IBM Quantum Platform

1. Before setting up with IBM Quantum Platform, ensure you are working in an active Python environment with the Qiskit SDK and Qiskit Runtime installed.

2. If you do not already have a user account, get one at the IBM Quantum login page. Your user account is associated with one or more instances (in the form hub / group / project) that give access to IBM Quantum services. Additionally, a unique token is assigned to each account, allowing for IBM Quantum access from Qiskit. The instructions in this section use our default instance. For instructions to choose a specific instance, see Connect to an instance.

<!-- image -->

Note

The Instances section in your IBM Quantum account page lists the instances that you can access.

3. Retrieve your IBM Quantum token from the IBM Quantum account page .

4. Authenticate to the service by calling Q iskitR untimeService with your IBM Quantum API key and CRN:

1 fromqiskit_ibm_runtimeimportQ iskitR untimeSer vic e 2 3 service=Q iskitR untimeService(channel="ibm_quantum 4

Or, optionally use the save_account() method to save your credentials for easy access later on, before initializing the service.

1 fromqiskit_ibm_runtimeimportQ iskitR untimeSe rvi c 2 3 # SaveanIBM Q uantumaccountandsetitasyourd 4 Q iskitR untimeService.save_account( 5 channel="ibm_quantum", 6 token="<MY_IBM_Q UANTUM_TO KEN>", 7 set_as_default=True, 8 # Use'overwrite=True'ifyou'reupdatingyour 9 overwrite=True, 1 0 ) 1 1 1 2 # Loadsavedcredentials 1 3 service=Q iskitR untimeService()

If you save your credentials to disk, you can use Q iskitR untimeService() in the future to initialize your account. The channel parameter distinguishes between different account types.

If you are saving multiple accounts per channel, consider using the nam e parameter to differentiate them.

Credentials are saved to $HO ME/.qiskit/qiskit-ibm.json . Do not manually edit this file.

If you don't save your credentials, you must specify them every time you start a new session.

The channel parameter allows you to distinguish between different account types. When initializing the account, IBM Cloud is the default account used if have saved credentials for an IBM Quantum Platform and an IBM Cloud account.

<!-- image -->

Caution

Account credentials are saved in plain text, so only do so if you are using a trusted device.

## Note

IBM Cloud is the default account used if you don't specify a different channel or account name.

<!-- image -->

## Qiskit Code Assistant

If you forget the setup code, try asking Qiskit Code Assistant.

# SetupmyIBM R untimeaccountusingTO KEN formyt oke n

New to the code assistant? See Qiskit Code Assistant for installation and usage. Note this is an experimental feature and only available to IBM Quantum™ Premium Plan users.

5. Test your setup. Run a simple circuit using Sampler to ensure that your environment is set up properly:

1 fromqiskitimportQ uantumC ircuit 2 fromqiskit_ibm_runtimeimportQ iskitR untimeService, S 3 4 # C reateemptycircuit 5 example_circuit=Q uantumC ircuit(2) 6 example_circuit.measure_all() 7 8 # You'llneedtospecifythecredentialswheninitiali 9 service=Q iskitR untimeService() 1 0 backend=service.least_busy(operational=True, simulat 1 1 1 2 sampler=Sampler(backend) 1 3 job=sampler.run([example_circuit]) 1 4 print(f"jobid: {job.job_id()}") 1 5 result=job.result() 1 6 print(result)

## Set up to use IBM Quantum Platform with REST API

Alternatively, you can also access quantum processors with REST APIs, enabling you to work with QPUs using any programming language or framework.

1. Retrieve your IBM Quantum token from the IBM Quantum account page .

2. If you do not already have a user account, get one at the IBM Quantum login page. Your user account is associated with one or more instances (in the form hub / group / project) that give access to IBM Quantum services. Additionally, a unique token is assigned to each account, allowing for IBM Quantum access from Qiskit. The

instructions in this section use our default instance. For instructions to choose a specific instance, see Connect to an instance.

<!-- image -->

Note

The Instances section in your IBM Quantum account page lists the instances that you can access.

3. Optionally obtain a temporary access token by supplying the API token obtained above. This is especially useful if you would like control over tokens, such as token invalidation. Alternatively, you can work directly with your IBM Quantum Platform API token.

4. View your available backends:

View your available backends: 1 importrequests 2 3 url='https://auth.quantum-computing.ibm.com/api/u 4 input={'apiToken': "<MY_IBM_Q UANTUM_TO KEN>"} 5 auth_response=requests.post(url, json=input) 6 auth_id=auth_response.json()['id'] 1 url_backends='https://api.quantum-computing.i bm. c 2 headers={'C ontent-Type': 'application/json', 3 'x-access-token':auth_id} 4 5 backends_response=requests.get(url_backends, head 6 7 print(backends_response.json()['devices'][:5],"..."

You can then transpile circuits using REST API and run them using either the Sampler or the Estimator primitives.

5. After your experiments are complete, you can proceed to invalidate your token and then test its invalidation.

1 logout_url='https://auth.quantum-computing.ib m.c o 2 headers={'x-access-token':auth_id} 3 logout_response=requests.post(logout_url, headers 4 print("responseok?:",logout_response.ok,logout_res

This should yield an error (Error 401) once the access token is invalidated.

1 logout_url='https://auth.quantum-computing.i bm. c 2 headers={'x-access-token':auth_id} 3 logout_response=requests.post(logout_url, header 4 5 iflogout_response.status_code==200: 6 job_id=logout_response.json().get('id') 7 print("Jobcreated:",logout_response.text) 8 eliflogout_response.status_code==401 : 9 print("invalidcredentials. Accesstokenshoul 1 0 else: 1 1 print(logout_response.text," ") 1 2 print(f"Error: {logout_response.status_code}")

Output

invalidcredentials. Accesstokenshouldbesucces sf u l

## Set up to use IBM Cloud

1. Before setting up with IBM Cloud, ensure you are working in an active Python environment with the Qiskit SDK and Qiskit Runtime installed.

2. If you do not already have one, set up an IBM Cloud account from the IBM Cloud Registration page .

3. Create a service instance, if necessary. Open your IBM Cloud Instances page . If you have one or more instances shown, continue to the next step. Otherwise, click Create instance . When creating your instance you can name it, tag it, select a resource group for it, and select a performance strategy. Next, agree to the license agreements by checking the box in the bottom right corner of the page, and click Create .

<!-- image -->

Note

If you are an administrator who needs to set up Qiskit Runtime on Cloud for your organization, refer to Plan Qiskit Runtime for an organization .

4. Find your access credentials.

1. Find your API key. From the API keys page , view or create your API key, then copy it to a secure location so you can use it for authentication.

2. Find your Cloud Resource Name (CRN). Open the Instances page and click your instance. In the page that opens, click the icon to copy your CRN. Save it in a secure location so you can use it for authentication.

1. Authenticate to the service by calling Q iskitR untimeService with your saved credentials or with your IBM Cloud API key and CRN:

1 fromqiskit_ibm_runtimeimportQ iskitR untimeSer vic e 2 3 service=Q iskitR untimeService(channel="ibm_cloud",

You can optionally use the save_account() method to save your credentials for easy access later on, before initializing the service.

1 fromqiskit_ibm_runtimeimportQ iskitR untimeS erv i 2 3 # Saveaccounttodisk. 4 Q iskitR untimeService.save_account( 5 channel="ibm_cloud", 6 token="<IBM C loudAPI key>", 7 instance="<IBM C loudC R N>", 8 name="<account-name>", 9 # Use'overwrite=True'ifyou'reupdatingyou 1 0 overwrite=True, 1 1 ) 1 2 1 3 # Loadsavedcredentials 1 4 service=Q iskitR untimeService(name="<account-nam

If you save your credentials to disk, you can use Q iskitR untimeService() in the future to initialize your account. The channel parameter distinguishes between different account types. When initializing the account, IBM Cloud is the default account used if you have saved credentials for both an IBM Quantum Platform and an IBM Cloud account.

If you are saving multiple accounts per channel, consider using the nam e parameter to differentiate them.

Credentials are saved to $HO ME/.qiskit/qiskit-ibm.json . Do not manually edit this file.

If you don't save your credentials, you must specify them every time you start a new session.

Caution

Account credentials are saved in plain text, so only do so if you are using a trusted device.

2. Test your setup. Ensure that you can connect to the service:

1 fromqiskit_ibm_runtimeimportQ iskitR untimeServic e 2 3 service=Q iskitR untimeService()

## Next steps

## Recommendations

Configure the Qiskit SDK locally.

Follow the steps in Hello world to write and run a quantum program.

Get started with IBM Quantum on Cloud.

Try one of the workflow example tutorials.

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .