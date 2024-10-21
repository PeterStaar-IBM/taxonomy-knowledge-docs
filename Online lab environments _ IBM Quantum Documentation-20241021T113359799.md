## Online lab environments

In addition to downloading and installing Qiskit to work on your local machine, there are several options available for those who prefer to work in an online development environment.

Several providers offer online Jupyter environments with Qiskit preinstalled for fast and convenient onboarding. These recommended providers include the cloud service provider OVHcloud and the quantum software development cloud platform qBraid.

Additional online Jupyter notebook environments like IBM Watson® Studio, Google Colab, and Microsoft Azure Machine Learning Studio can be used to work with Qiskit by following the instructions below.

## Recommended notebook environments with Qiskit preinstalled

## OVHcloud AI notebooks

OVHcloud Public Cloud and OVHcloud Startup users can use their OVHcloud quantum notebooks to access Qiskit and submit jobs to IBM® QPUs with their IBM Quantum™ or IBM Cloud® account. To learn more, visit the OVHcloud Documentation and tutorials page.

<!-- image -->

## Note

OVHcloud AI notebooks are not available in some regions, such as the United States. Additionally, user interfaces might look different in different regions.

## Follow these steps to get started:

1. Create an IBM Quantum or IBM Cloud account.

2. Set up an OVHcloud account.

3. Create an OVH Public Cloud project.

4. Follow the steps in Launch your first AI Notebook, selecting Qiskit for the framework.

## qBraid Lab

Qiskit users can use qBraid's preconfigured Python environments by following these instructions. For more information, see the qBraid Docs.

1. Set up an qBraid free account.

2. Create a Lab environment.

3. Launch qBraid Lab, click the ENVS icon on the right, click + ADD at the top of the new sidebar, and search for Qiskit. Click the version you want to use, then click Install .

4. Create a Jupyter notebook by clicking Python 3 [Qiskit] in the Notebook section.

5. You can install any additional Python packages (Such as Qiskit v1.2) in a notebook cell by running % pipinstall[package_name]. For more information, see the Pip (magic) commands topic.

## Additional notebook environments for Qiskit users

For platforms without Qiskit preconfigured, the setup instructions are similar, except that you must install Qiskit manually. You will create an account, start a new project (or notebook or workspace, depending on the platform), and install Qiskit.

## IBM Watson Studio

1. Set up Watson Studio free trial.

2. Create a Watson Studio Project.

3. To install the Qiskit packages and dependencies needed, open a new notebook and run the following command:

!pipinstallqiskitqiskit-ibm-runtimepylatexenc!y es

## Google Colab

1. Create a Google account.

2. Go to the Google Colab landing page and sign in with your Google account.

3. Create a Colab notebook and install Qiskit by running the following command:

% pipinstallqiskitqiskit-ibm-runtime

## Microsoft Azure Machine Learning Studio

1. Create an Azure account.

2. Create a workspace.

3. Create a Jupyter notebook in your workspace.

4. From the notebook, run this command to install Qiskit:

% pipinstallqiskitqiskit-ibm-runtime

## Helpful links

## Documentation resources

OVHcloud documentation

qBraid Lab documentation

IBM Watson Studio documentation

Google Colab documentation

Microsoft Azure Machine Learning Studio documentation

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .