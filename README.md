# Code for the analyses of "Conditioned pain expect..."

## Reproducing the analyses

Most analyses performed can be reproduced using the Docker container mpcoll2/eeg_XX. All Python scirpts can be executed using the following command with the appropriate path. The data path should point to a folder cotaining the bids formatted dataset in a "source" folder (e.g. for */data/source* path would be */data*).


```
docker run -it -v $PATHTODATA$:/data mpcoll2/eeg_2020:latest python $script.py$
```

R scripts can be executed using the command
```
docker run -it -v $PATHTODATA$:/data mpcoll2/r_lme:latest Rscript $script.R$
```

SCR analyses as well as computation models fitting and comparison were done in Matlab using the PsPM toolbox (https://bachlab.github.io/PsPM/), the TAPAS toolbox (https://github.com/translationalneuromodeling/tapas) and the VBA toolbox (https://mbb-team.github.io/VBA-toolbox/).

Multilevel mediation analyses were performed in Matlab using the Canlab mediation toolbox (https://github.com/canlab/MediationToolbox).

## 0- Import raw data

For the sake of simplicity and reducing online storage space requirements, we openly share the BIDS structured "almost raw" dataset. The BIDS formatted dataset is "almost raw" because some downsampling was applied to the physiological signals during import so that all signals would be sampled at 1000 Hz for all participants. The script *raw_to_bids.py*  documents all operations performed to restructure the raw files outputted by the various recording softwares into the BIDS format.

## 1- SCR

### 1.1. Import data for PsPM

The *scr_prepare_pspm.py* script imports the scr data and information (onsets, durations) in a matlab structure for each participant to facilitate the preprocessing in PSPM. The script outputs figures to visually inspect the SCR, a .mat structure for PSPM and the data in a .txt file for PSPM.

### 1.2 Process the data with PsPM

The *scr_glm_pspm.m* script imports the scr data in PsPM, performs the data cleaning and produces a GLM for each trial. It outputs figures illustrating the design matrix and other information on the GLM as well as the amplitude estimates for each trial in "_responses.mat" file. The */data/derivatives* path needs to be changed at the top of the script. SCR responses were further to fit computational models as described below.

### 1.3 Mixed models on SCR data

The script *starts_scr_lmer.R*

## 2- Computational models

The computational models are described in their respective code in /code/compomodels. The MATLAB script comp_fitmodels.m fits the models specified at the start of the script and performs model comparison. For each model, it outputs the trial-wise values in a mat/csv files, some figures and a .mat file containing the results of the model comparison.

## 3- EMG

The amplitude of the nociceptive flexion reflex (NFR) in responses to shock is calculated in the script *emg_nfr.py*. This scripts outputs a csv file for each participant with the the NFR for each shock as well as the pain rating. Some figures are also produced to inspect the data.

## 4- EEG

### 4.1 Preprocessing

EEG data import and preprocessing using MNE-python is performed in the *eeg_import_clean.py* script. This will produce an html report for each participant illustrating the ICA decomposition and the ICs that were manually removed.

### 4.2 ERPs

The creation of event-related potential data to cues and shocks is performed by running the script *eeg_erps.py*.

### 4.3 TFR

Time-frequency decomposition is perfomed using the script *eeg_tfr.py*

### 5- Statistical analyses

*Note that the TFCE permutation analyses can take a few hours to a few days to run depending on your hardware. Ideally, these scripts should be run on a high performance cluster. Otherwise, one could reduce the number of permutations or comment out the TFCE analyses and use a less computationnaly intensive correction method (e.g. FDR).*

5.1 Model free analyses

The model free analyses (difference between conditions) are performed on ERPs and TFR by running the scripts *stats_erps_modelfree.py* and *stats_tfr_modelfree.py* respectively.

5.2 Model based analyses

### 6- Figures

All data figures presented in the manuscript can be reproduced by running each of the script in the figures directory after all the data processing scripts were run.
