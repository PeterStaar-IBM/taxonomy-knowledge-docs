## Latest updates

## Lastupdated9October2024

Keep up with the latest and greatest from Qiskit and IBM Quantum™! Gathered here are the the most recent Qiskit package release summaries, documentation updates, blogs, community events, and more.

## Latest release summaries

## Qiskit SDK v1.2.0

Toseethereleasenotesforallversions,visittheQiskitSDKrelease notes.

Qiskit 1.2 (stable release) is now available on PyPI and GitHub . Thanks to the maintainers and open-source contributors!

Many of the core structures have been moved to Rust, allowing for significant speedups mostly in the construction, manipulation (including copy and parameter assignment), and synthesis of circuits.

Better compiler quality due to the addition of unitary peephole optimization (for optimization levels 2 and 3) and improvements in Sabre.

generate_preset_pass_manager importance highlight, which is now importable from the top level.

Important deprecations:

Primitive V1 reference implementations (like qiskit.primitives.Estimator ) and tooling are deprecated in favor of V2 primitives. The primitive V1 base classes remain available.

BackendV1 is now deprecated in favor of BackendV2 , including the deprecation of Q obj , assemble , and other scaffolding related to the old V1.

Support for Python 3.8 will be dropped in the next release, v1.3. Python 3.9 will then be the new minimal required version.

Remember, because this a minor release, it will not introduce breaking changes for users moving from Qiskit SDK v1.0. Any deprecations announced with this release refer only to things that will now start throwing deprecation warnings. None of these will be removed until the release of Qiskit v2.0, pursuant to Qiskit's official deprecation policy .

For more details, read the post about the release on the IBM Quantum blog , and explore the v1.2 release notes.

## Qiskit Runtime Client 0.28.0

Toseethereleasenotesforallversions,visittheQiskitRuntimeClient releasenotes.

## Highlights from the 0.28.0 (2024-08-15) version release

## Removal

The V1 primitives SamplerV1 and EstimatorV1 have been completely removed. Refer to the migration guide for instructions on migrating to V2 primitives.

The service parameter is now required in Session.from_id() .

## New features

You can now use the layer_noise_model option in R esilienceO ptionsV2 to pass in a noise model. When this field is set, all the mitigation strategies that require noise data skip the noise learning stage, and instead gather the required information from layer_noise_model . You can use the NoiseLearner class to learn noise and use it in a subsequent Estimator call.

See the full 0.28.0 release notes.

## Qiskit Transpiler Service Client v0.3.0

We're pleased to share that the beta release of the Qiskit Transpiler Service is now available to all IBM Quantum™ Premium Plan users.

The Qiskit Transpiler Service leverages the resources of IBM Cloud® to provide users with the latest transpilation capabilities from the Qiskit SDK. It offers a Python library that helps users seamlessly integrate the service into their current Qiskit patterns and workflows. Perhaps most importantly, the service invites users to experiment with new and improved AI-powered transpiler passes - cutting-edge tools that might be faster and produce better results than traditional transpilation methods.

On the IBM Quantum blog, we take a deep dive into the new beta release with a special focus on the new AI-powered passes. The blog includes detailed explanations showing how to use the Qiskit Transpiler Service for both full circuit transpilation and standalone AI-powered passes, with code examples for each. Take a look.

## Qiskit Functions Catalog Service Client v0.1.0

Introducing the Qiskit Functions preview, for IBM Quantum Premium Plan users. To get started, pipinstallqiskit-ibm-catalog , and explore the Qiskit Functions documentation. With the Qiskit Functions Catalog client, you can submit workloads to abstracted services designed to accelerate your research - sign in with your existing IBM Quantum Platform credentials.

Premium Plan users can get started right away with the IBM® Circuit function, or request access to circuit or application functions from our partners.

## Qiskit Runtime service updates

This summary of the latest changes to the Qiskit Runtime service applies to anyone using Qiskit Runtime, regardless of how you communicate with the service (using qiskit-ibm-runtime or otherwise), or which version of the client SDK you use.

## 15 August 2024

Probabilistic error amplification (PEA) error mitigation method is now available for Estimator V2. See the ZneO ptions API reference for more details.

A new helper program noise-learner is now available. It allows characterizing the noise processes affecting the gates in one or more circuits of interest, based on the Pauli-Lindblad noise model. The output noise model can then be passed to Estimator via the layer_noise_model option.

V1 primitives are no longer supported.

## 3 July 2024

The following endpoints are deprecated and will be removed on or after 3 October 2024: GET /stream/jobs and GET /stream/jobs/{id} . This removal has the following impacts:

After the endpoints are removed, job.stream_results() and job.interim_results() will be removed from the qiskit-ibmruntime client.

Additional methods, such as job.result() currently use the deprecated endpoints. Upgrade to qiskit_ibm_runtime 0.25 or later before the endpoints are removed to avoid disruption.

## 18 June 2024

The optimization_level option is deprecated for Estimator V2 and will be removed no sooner than 30 September 2024.

## 3 June 2024

Probabilistic error amplification (PEA) error mitigation method is now available as an experimental option for Estimator V2. See the EstimatorO ptions API reference for more details.

## 28 March 2024

Qiskit Runtime primitives now support OpenQASM 3 as the input format for circuits and standard JSON output format. See Qiskit Runtime REST API for examples.

## Qiskit Functions Catalog updates

## 16 September 2024

The Qiskit Functions Catalog preview provides access to Premium Plan users to explore the available functions, including those written by IBM and those written by other members of our ecosystem. The catalog contains two kinds of functions: circuit functions and application functions.

Circuit functions provide a simplified interface for running circuits. They receive user-provided abstract circuits and observables as input, then manage synthesis, optimization, and execution of the representative ISA circuit. Circuit functions bring together the latest capabilities in transpilation, error suppression, and error mitigation to make utility-grade performance accessible out of the box. This allows computational scientists to focus on mapping their problems to circuits, rather than building the pattern for each problem from scratch.

Application functions cover higher-level tasks, like exploring algorithms and domain-specific use cases. Enterprise developers and data scientists may not have the background quantum information science knowledge for working with circuits, and instead hope to bring their domain knowledge to advance quantum computing algorithms and applications. With application functions, users can enter their classical inputs and receive solutions so they can more easily experiment with plugging quantum into their domain-specific workflows.

With the launch of the Qiskit Functions Catalog, Premium Plan developers will be able to start exploring the IBM Circuit function. The IBM Circuit function includes the latest AI-powered extensions to Qiskit for circuit synthesis, optimization, and scheduling, as well as advanced error mitigation methods to return the most accurate estimations possible with today's hardware.

Users can purchase licenses for the following functions contributed by our partners at Q-CTRL, QEDMA, Algorithmiq, and Qunasys.

## Circuit functions

Q-CTRL is releasing a circuit function that applies AI-driven quantum control techniques, with which users can scale successfully to larger problems.

Algorithmiq is releasing a circuit function that applies TEM (tensornetwork error mitigation), an error mitigation method for obtaining estimators with fewer shots than the PEC (probabilistic error cancellation) method.

QEDMA is releasing a circuit function that uses proprietary protocols for efficient and accurate characterization of the noisy QPU operations, and applies error suppression and error mitigation based on the characterization data.

## Application functions

QunaSys is releasing a chemistry application function comprising three algorithms meant to solve the ground state energy estimation (GSEE) problem.

Q-CTRL is also releasing an optimization solver with which users can pass a graph or an objective, and receive solution costs.

To get started, explore the Qiskit Functions documentation.

## IBM Quantum blog: A closer look at Qiskit Code Assistant

BrowseallblogsattheIBMQuantumblogpage .

We recently made Qiskit Code Assistant available as a preview release for IBM Quantum Premium Plan users. In our latest post on the IBM Quantum blog, we take a look at the inner workings of the granite-8bqiskit model behind this powerful new quantum code generation tool, and the unique challenges that went into building it.

The blog opens with a high-level overview of our motivations for building Qiskit Code Assistant, long-term plans for open-sourcing the code assistant model, and instructions for getting started with Qiskit Code Assistant in popular coding environments VS Code and JupyterLab. A more detailed version of these instructions can be found in the documentation.

From there, the blog goes on to deliver popular summaries of two recent arXiv papers detailing our work developing the granite-8b-qiskit model, and the Qiskit HumanEval benchmarking method we use to evaluate the performance of quantum code generation tools. Interested readers can explore the blog , and those looking for more technical detail can find the aforementioned arXiv papers here and here .

## What's new in the documentation

IBM Quantum documentation recently added a number of user-facing improvements, including content updates and new features. Many of these changes are a result of specific user requests! Check out the highlights below.

## New and updated content

## New pages

Primitive inputs and outputs

Noise learning helper

Expanded and improved Qiskit Serverless docs, including four new pages that walk you through how to use Qiskit Serverless: Write your first Qiskit Serverless program, Run your first Qiskit Serverless workload remotely, Manage Qiskit Serverless compute and data resources, and Port code to Qiskit Serverless

## Content additions and improvements

Clarity around the maximum execution time for batches, how usage is reported for failed and canceled jobs, and how usage affects job priority within an instance.

Probabilistic error amplification (PEA) has been added to the Error techniques topic.

New options compatibility table for Qiskit Runtime error mitigation tools.

Updated docs to reflect that V1 primitives are no longer supported.

Copyediting and typo fixes across the docs, including bugs reported by community contributors.

## New API reference docs

Qiskit SDK v1.2

Qiskit Runtime v0.28

qiskit-ibm-transpiler v0.5

## User experience improvements

The table of contents for the IBM Quantum Platform docs is now organized into Get started, Workflow, and Tools sections. Whether you are looking for documentation specific to a Qiskit pattern step, or simply want to jump straight to the tool you need, you can quickly browse for the content you need.

The landing page has a fresh new design with an improved focus on API references.

We've launched an Additionalresourcestab that directs you to the migration guides, open-source resources, responsible quantum computing guidelines, and the Qiskit ecosystem .

Tables in the docs now have borders to improve readability.

Alt-text has been added to all images to make them more accessible.

Heading size has been adjusted so that it does not change when the screen size changes.

Code styling has been improved, including bigger fonts and better contrast for code block comments.

In Search, you can now filter out terms by prefixing it with -, such as -qiskit

When on outdated versions of docs, the link in the top banner to take you to the latest version now preserves the page you were on.

Ahugethankyougoesouttoeveryoneintheopen-sourcecommunity whocontributedandgavefeedback!Please open an issue if you find a bug, have a suggestion, or want to share your experience.

## Qiskit global community events

Join, participate, contribute! See what events are coming up on the Qiskit community events calendar .

## See all product updates

Visit the Product announcements to browse all product updates and news.

Was this page helpful?

<!-- image -->

Report a bug or request content on GitHub .