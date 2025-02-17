# Robust model-based deep reinforcement learning for flow control
## Abstract
Active flow control has the potential of achieving remarkable drag reductions in applications for fluid mechanics, when 
combined with deep reinforcement learning (DRL). The high computational demands for CFD simulations currently limits the 
applicability of DRL to rather simple cases, such as the flow past a cylinder, as a consequence of the large amount of simulations 
which have to be carried out throughout the training. One possible approach of reducing the computational requirements is 
to substitute the simulations partially with models, e.g. deep neural networks; however, model uncertainties and error 
propagation may lead an unstable training and deteriorated performance compared to the model-free counterpart. The present 
thesis aims to modify the model-free training routine for controlling the flow past a cylinder towards a model-based one. 
Therefore, the policy training alternates between the CFD environment and environment models, which are trained successively 
over the course of the policy optimization. In order to reduce uncertainties and consequently improve the prediction accuracy, 
the CFD environment is represented by two model-ensembles responsible for predicting the states and lift force as well as 
the aerodynamic drag, respectively. It could have been shown that this approach is able to yield a comparable performance 
to the model-free training routine at a Reynolds number of $Re = 100$ while reducing the overall runtime by up to $68.91\%$. 
The model-based training, however, showed a high dependency of the performance and stability on the initialization, which 
needs to be investigated further. An increase of the Reynolds number to $Re = 500$ and $Re = 1000$ revealed several issues 
within the model-free training routine, such as a dependency of the stability of the policy optimization on its initialization, 
which were encountered in the subsequently conducted model-based trainings as well.

## Overview
This student thesis project aims to implement a model-based deep reinforcement learning algorithm for controlling the
flow past a cylinder. Therefore, the [drlfoam](https://github.com/OFDataCommittee/drlfoam) repository, which already 
provides a model-free version is used as a starting point. The full report of this thesis can be found [here](#report).

This project is a continuation of the work done by [Darshan Thummar](https://github.com/darshan315/flow_past_cylinder_by_DRL) and
[Fabian Gabriel](https://github.com/FabianGabriel/Active_flow_control_past_cylinder_using_DRL), a first attempt to use a model-based
approach in order to accelerate the training process was implemented by [Eric Schulze](https://github.com/ErikSchulze1796/Active_flow_control_past_cylinder_using_DRL).

**Note: The encountered stability issues in a model-based training described in the report as well as in the overview
notebook were a consequence of an implementation error when computing the action in the model-based episodes. This error
was discovered after the [submission of the report](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/tree/submission)
and corrected afterwards ([commit](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/commit/b0e3b8c8322a72aec1a7314aa8b93f7369e4f67f)).**

## Current version of the MB-training routine
This project will not be further updated (except bug fixes). For the current version of the model-based training 
please go to [drlfoam (branch 'mb_drl')](https://github.com/JanisGeise/drlfoam/tree/mb_drl). Updated versions of the post-processing scripts for the PPO-training can be found 
[here](https://github.com/JanisGeise/MB_DRL_for_accelerated_learning_from_CFD) along with example datasets
generated with the current version of the model-based training routine. 

## Getting started
### General information
An overview of this repository and information on how to choose parameters for training can be found in the
[overview notebook](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/overview.ipynb). This 
repository only contains all altered and added scripts of *drlfoam* in order to modify the MF-DRL algorithm towards an
MB-version. These scripts can e.g. be downloaded and pasted into an existing (local) *drlfoam* version. The modified scripts are 
located in the *mb_drl* directory and need to be sorted into *drlfoam* as follows:

- *create_dummy_policy.py*, *run_training.py* and *get_number_of_probes.py* have to be located in *drlfoam/examples/*
- *execute_prediction.py*, *env_model_rotating_cylinder.py*, *train_env_models.py* and *predict_trajectories.py* have to
  be located in *drlfoam/drlfoam/environment/*
- *rotating_cylinder.py* in *drlfoam/drlfoam/environment/* can be replaced with this version of *rotating_cylinder.py*

Alternatively, a completed MB-version of *drlfoam* can be found [here](https://github.com/JanisGeise/drlfoam/tree/mb_drl), which is forked from the
[original drlfoam](https://github.com/OFDataCommittee/drlfoam) repository. The remaining setup is the same as presented in the
*Readme* file of the *drlfoam* repository.

### Outdated scripts
Some scripts were developed during the thesis and are not used anymore. Further, some scripts may not work anymore since
there have been various changes since the submission of the report. [This table](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/outdated_scripts.MD)
lists all outdated scripts, which are either not used anymore or which may not work anymore. These scripts will most likely not be updated in the future. The
referenced commit for each script indicates the point until these script should work, in some cases, e.g. scripts for 
parameter studies may also work in later versions. All scripts, which are not listed here are still working and used in 
the MB-training routine or for post-processing the results.

### Testing the different approaches of modeling the CFD environment
All approaches of modeling the CFD environment with fully-connected neural networks presented in the report are located in the *test_env_models* directory.
In order to use these scripts, a model-free training using *drlfoam* is required to be conducted in advance. More information on how to use these scripts can
be found in the [overview notebook](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/overview.ipynb) and in the documentation at the top of each script.
It is important to note that the training routine implemented in these scripts corresponds to the training routine implemented in
*env_model_rotating_cylinder.py*. Further, since the scripts use the *matplotlib* library, they should be executed on a local machine (not an HPC cluster).
The overall runtimes of these scripts are generally in the order of minutes up to approximately one hour when executed on a local machine, depending on the setup.
The *test_env_models* directory as well as the *scripts_py_plots* directory is not required for conducting model-free or model-based trainings.
The additional requirements for using the scripts in the *test_env_models* directory can be found in the [requirements.txt](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/requirements.txt)

### Installation and running a training
The installation of *drlfoam* is thoroughly described in the [drlfoam](https://github.com/OFDataCommittee/drlfoam)
repository along with a comprehensive guide on how to conduct trainings either on a local machine or on HPC clusters.
Since the *drlfoam* repository is frequently updated, some instructions or dependencies may change in the future and are therefore
not presented here in order to avoid any discrepancies between *drlfoam* and this repository.

Examples of shell-scripts for submitting jobs on an HPC cluster (here for the [Phoenix](https://www.tu-braunschweig.de/it/dienste/21/phoenix)
cluster of TU Braunschweig) can be found in [run_job.sh](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/run_job.sh)
and [submit_jobs.sh](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/submit_jobs.sh).

## Post-processing and visualization of the results
The results of the model-free as well as of the model-based training can directly be post-processed and visualized using 
the scripts located in the *scripts_py_plots* directory. These scripts work only when running on a local machine, since
they rely on *matplotlib*. For post-processing the results of a PPO-training, the *plot_ppo_results.py* script is the main
script, which needs to be executed. Prior execution all paths and settings need to be adjusted in the *setup* dictionary
of the script.

The scripts *compare_training_routines.py*, *influence_buffer_and_trajectory_length.py*, 
*influence_network_architecture_new_training* and *influence_ratio_MB_MF_episodes.py* can be used for conducting and post-precessing
parameter studies regarding the MF- and MB-training. Further information on how to use these scripts can be found in the documentation
at the top of each script.

## Troubleshooting
In case something is not working as expected or if you find any bugs, please feel free to open up a new
[issue](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/issues). You can also check out the last section of the 
[overview notebook](https://github.com/JanisGeise/robust_MB_DRL_for_flow_control/blob/main/overview.ipynb) for tips.

## Report
The report of this thesis can be found under: https://zenodo.org/record/7642927

BibTex citation:
```
@misc{janis_geise_2023_7642927,
  author       = {Janis Geise},
  title        = {{Robust model-based deep reinforcement learning for 
                   flow control}},
  month        = feb,
  year         = 2023,
  publisher    = {Zenodo},
  version      = 1,
  doi          = {10.5281/zenodo.7642927},
  url          = {https://doi.org/10.5281/zenodo.7642927}
}
```

## References
- the original [drlfoam repository](https://github.com/OFDataCommittee/drlfoam), currently maintained by
  [Andre Weiner](https://github.com/AndreWeiner)
- implementation of the (model-free) PPO algorithm for active flow control:
  * **Thummar, Darshan**. *Active flow control in simulations of fluid flows based on deep reinforcement learning*,
  https://doi.org/10.5281/zenodo.4897961 (May, 2021).
  * **Gabriel, Fabian**. *Aktive Regelung einer Zylinderumströmung bei variierender Reynoldszahl durch bestärkendes Lernen*,
  https://doi.org/10.5281/zenodo.5634050 (October, 2021).

- first attempt of implementing a model-based version for accelerating the training process:
  * **Schulze, Eric**. *Model-based Reinforcement Learning for Accelerated Learning From CFD Simulations*,
  https://doi.org/10.5281/zenodo.6375575 (March, 2022).
