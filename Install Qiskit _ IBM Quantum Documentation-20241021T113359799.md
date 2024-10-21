## Install Qiskit

Coding with Qiskit 1.x, Episode 2: How to install Qiskit


<!-- image -->

Whether you will work locally or in a cloud environment, the first step for all users is to install Qiskit. For those wanting to run on a real quantum processing unit (QPU), your next step is to choose one of two channels in order to access IBM® QPUs: IBM Quantum Platform or IBM Cloud®.

## Upgrade from Qiskit 0.x to Qiskit 1.0 and beyond

(If you are installing Qiskit for the first time, skip ahead to the Install and set up section. This notice is relevant only to users who have installed Qiskit previously.)

For those upgrading from version 0.x to 1.0 or later : note that because Qiskit v1.0 uses a new packaging structure, you cannot use pipinstall-U qiskit to upgrade from any Qiskit 0.x version to 1.0.

See the Qiskit 1.0 migration guide for details and instructions.

Future updates starting with Qiskit 1.0 will allow for in-place upgrades.

## Install the Qiskit SDK and the Qiskit Runtime client

1. Install Python. Check the "Programming Language" section on the Qiskit PyPI project page to determine which Python versions are supported by the most recent release. For download instructions, see the Python Beginners Guide.

It is recommended that you use Python virtual environments to separate Qiskit from other applications.

<!-- image -->

Note

These instructions use the standard Python distribution from pypi.org . However, you can use other Python distributions, such as Anaconda or miniconda , along with other dependency management workflows like Poetry .

If you're new to virtual environments, click here for more information.

First, create a minimal environment with only Python installed in it.

macOS

Linux

Windows

python3 -mvenv/path/to/virtual/environment

Next, activate your new environment.

macOS

Linux

Windows

source/path/to/virtual/environment/bin/activate

2. Install pip if it's not already installed in your environment. Pip is a Python package manager that you use to install Qiskit and other Python packages. Use piplist to see what is in your virtual environment. In most Python environments, pip is already installed.

3. Install the Qiskit SDK. If you plan to run jobs on quantum hardware, also install Qiskit Runtime.

pipinstallqiskit

<!-- image -->

pipinstallqiskit-ibm-runtime

If you intend to use visualization functionality or Jupyter notebooks, it is recommended to install Qiskit with the extra visualization support ( 'qiskit[visualization]' ).

Default

zsh

pipinstallqiskit[visualization]

4. If you want to run a Jupyter notebook with the Qiskit packages you just installed, you will need to install Jupyter in your environment.

pipinstalljupyter

Then open your notebook as follows:

jupyternotebookpath/to/notebook.ipynb

If you are planning to work locally and use simulators built into Qiskit, then your installation is done. If you want to run jobs on IBM QPUs, next select an access channel and finish your setup.

## Stay current with the latest versions

Periodically check the Qiskit release notes and the Qiskit Runtime release notes to see new releases. We recommend frequently updating your requirements for qiskit and qiskit-ibm-runtime by, for example, changing the versions in requirements.txt to the latest versions, then running pipinstall-rrequirements.txt or the appropriate command for your dependency management workflow.

## Qiskit Code Assistant

Need help? Try asking Qiskit Code Assistant.

# PrinttheversionofQ iskitwe'reusing

# R eturnTrueiftheversionofQ iskitis1 .0 orgreater

# InstallQ iskit1 .0.2

New to the code assistant? See Qiskit Code Assistant for installation and usage. Note this is an experimental feature and only available to

## Troubleshooting

"No Module 'qiskit'" error with Jupyter Notebook

Compilation errors during installation

## Operating system support

Qiskit strives to support as many operating systems as possible, but due to limitations in available testing resources and operating system availability, not all operating systems can be supported. Operating system support for Qiskit is broken into three tiers with different levels of support for each tier. For platforms outside these, such as FreeBSD or WebAssembly (WASI), Qiskit may still be installable, but it is not tested and you will have to build Qiskit (and likely Qiskit's dependencies) from source.

Additionally, Qiskit only supports the CPython implementation of the Python language. Running with other Python interpreters such as PyPy is not supported.

Tier 1

Tier 2

Tier 3

## Next steps

## Recommendations

Select and set up an IBM Quantum channel.

Configure Qiskit locally.

Follow the steps in Hello world to write and run a quantum program.

Try one of the workflow example tutorials.

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .