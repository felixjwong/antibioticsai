<!--
*** Thanks for checking out this README Template. If you have a suggestion that would
*** make this better, please fork the repo and create a pull request or simply open
*** an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->




<!-- PROJECT LOGO -->
<br />
<p align="center">

  <h3 align="center">Discovery of a structural class of antibiotics with explainable deep learning</h3>

  <p align="center">
    Supporting code for the paper
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of contents

* [About the code](#about-the-code)
* [Running the code](#running-the-code)
  * [Checkpoint files](#checkpoint-files)
  * [Chemprop usage and rationale calculations using Monte Carlo tree searches](#chemprop-usage-and-rationale-calculations-using-monte-carlo-tree-searches)
    * [Key dependencies](#key-dependencies)
    * [Training and predicting](#training-and-predicting)
    * [Calculating rationales](#calculating-rationales)
  * [Python notebook for filtering hits and computing rationale scaffolds](#python-notebook-for-filtering-hits-and-computing-rationale-scaffolds)
  * [Python notebook for maximal common substructure analyses](#python-notebook-for-maximal-common-substructure-analyses)
  
* [Contact](#contact)



<!-- ABOUT THE PROJECT -->
## About the code

The files in this repository allow for repeat analyses of those described in the paper "Discovery of a structural class of antibiotics with explainable deep learning". The code requires <a href="https://github.com/chemprop/chemprop">ChemProp</a> and Python with the appropriate packages installed, as indicated in the <i>Methods</i> section of the paper. In particular, please ensure that at least the following two packages are installed:
<ul>
<li><a href="https://github.com/chemprop/chemprop">Chemprop</a> (commit 9c8ff4074bd89b93f43a21adc49b458b0cab9e7f was used)</li>
<li><a href="https://www.rdkit.org/">RDKit</a> (at least version 2021.09.01)</li>
</ul>
Details on installation, the packages required, and the Jupyter notebook code platform are provided below.

<!-- GETTING STARTED -->
## Running the code

### Checkpoint files

In the folder "final_checkpoints", there are three directories, each with 20 ChemProp checkpoints corresponding to the final trained ensembles of models used to predict antibiotic activity against <i>Staphylococcus aureus</i>, cytotoxicity against HepG2 cells, and cytotoxicity against human primary skeletal muscle cells, as described in the paper. For the convenience of users, the training datasets of 39,312 compounds are reproduced from Supplementary Dataset 1 in the "final_checkpoints" folder.

### Chemprop usage and rationale calculations using Monte Carlo tree searches

This section will guide readers to train their own Chemprop models using the data in the provided training datasets, thereby providing an integrated platform for interested readers to develop and apply their own Chemprop models to antibiotic  discovery. We will also discuss how to use the trained models provided in "final_checkpoints" and how to compute rationales for predicted hits. 

#### Key dependencies

Key dependencies for this section focusing on Chemprop usage are provided in the folder entitled "working_example":

<ul>
<li>train.csv: A CSV file containing the SMILES strings and activity values of compounds in the training set. The default file provided is the training set of 39,312 compounds described in the main text, with activity values defined by the <i>S. aureus</i> growth inhibition activity criteria described in the main text. There are a total of 512 active (ACTIVITY=1) compounds; the remaining compounds are inactive (ACTIVITY=0).
</li>
<li>
hyperparameters.json: A JSON file containing key hyperparameters specifying the architecture of the Chemprop model used. By default, the parameters used (for antibiotics) are: depth=5, dropout=0.35, ffn_num_layers=3, and hidden_size=1600, as detailed further in Extended Data Table 4 of the paper.
</li>
<li>test.csv: A CSV file containing the SMILES strings of compounds for which Chemprop predictions of antibiotic activity will be made. The default file provided is the set of 327 procured and experimentally tested compounds in our study; this smaller subset was provided to facilitate evaluation for setups with less computational power. For larger sets of compounds computationally evaluated in the paper, please refer to Supplementary Dataset 2 of the paper. 
</li>
<li>hit.csv: A CSV file containing the SMILES string "CC1N(C2C=CC(F)=C(Cl)C=2)N=NC=1C(NCC1(C(=O)O)CCOCC1)=O", which has a high antibiotic prediction score of 0.409 (when using the provided trained ensemble) and a rationale, "O=C(N[CH3:1])c1nnn(-c2ccc(F)c(Cl)c2)[cH:1]1", with rationale prediction score 0.138. 
</li>
<li>environment.yml: A file listing the Python dependencies needed for successful installation of Chemprop. 
</li>
</ul>


#### Training and predicting


##### Installing and activating Chemprop

It is important to have Chemprop installed. Our recommended way of doing so is to follow the <a href="https://github.com/chemprop/chemprop">Official Instructions</a> using Conda: at the command line, enter:

<ul>
<li>conda create -n chemprop python=3.8</li>
<li>conda activate chemprop</li>
<li>conda install -c conda-forge rdkit</li>
<li>pip install git+https://github.com/bp-kelley/descriptastorus</li>
<li>pip install chemprop</li>
</ul>

These commands will help install Chemprop and associated dependent Python packages (the accompanying "environment.yml" file details all of Chemprop's dependencies). To activate Chemprop, run:

<ul>
<li>conda activate chemprop</li>
</ul>

##### Training Chemprop

After Chemprop is activated, a Chemprop model can be trained using the following command:

```sh
chemprop_train --data_path working_example/train.csv --dataset_type classification --config_path working_example/hyperparameters.json --save_dir "working_example/checkpoints" --ensemble_size 1 --features_generator rdkit_2d_normalized --no_features_scaling
```

This will provide one model checkpoint in the folder "working_example/checkpoints". The Chemprop model will be defined using the hyperparameters in "working_example/hyperparameters.json" and trained on the data in "working_example/train.csv." The flag --dataset_type specifies binary classification, the flag --ensemble_size specifies the number of models in the ensemble, and the --features_generator and --no_features_scaling flags specify that we would like the model to be augmented with computed RDKit features, as detailed further in the paper. For advanced details about each of these flags and additional options, see Chemprop's <a href="https://github.com/chemprop/chemprop">Official Instructions</a>.

##### Predicting with Chemprop

After trained models are generated, any ensemble of at least one model can be used to make predictions using the following command: 

```sh
chemprop_predict --test_path working_example/test.csv --checkpoint_dir "working_example/checkpoints" --preds_path "working_example/test_pred.csv" --features_generator rdkit_2d_normalized --no_features_scaling 
```

The checkpoint directory, path to the test file, and path to the output file are specified using the corresponding flags. As the models have been augmented with computed RDKit features, the two --features_generator and --no_features_scaling flags are retained. 


#### Calculating rationales

Given any compound with a high prediction score ("hit"), <a href="https://chemprop.readthedocs.io/en/latest/interpret.html">a Monte Carlo tree search can be run on the compound in order to determine a "rationale"</a>, or substructure responsible for a threshold amount of the compound's prediction score, as follows:
```sh
chemprop_interpret --data_path "./hit.csv" --checkpoint_dir "working_example/checkpoints" --property_id 1 --features_generator rdkit_2d_normalized --no_features_scaling
```
Here, the "chemprop_interpret" command finds the rationale for the compound in "hit.csv" using the models in the specified checkpoint directory. As the models have been augmented with computed RDKit features, the two --features_generator and --no_features_scaling flags are retained. 

At present, editing the parameters used in the Monte Carlo tree search is not supported via the command line in Chemprop. To edit the parameters to suitable values, as described in the paper, the relevant section of Chemprop's args.py (in chemprop/) must be updated as follows:

```sh
class InterpretArgs(CommonArgs):
    """:class:`InterpretArgs` includes :class:`CommonArgs` along with additional arguments used for interpreting a trained Chemprop model."""

    data_path: str
    """Path to data CSV file."""
    batch_size: int = 500
    """Batch size."""
    property_id: int = 1
    """Index of the property of interest in the trained model."""
    rollout: int = 10
    """Number of rollout steps."""
    c_puct: float = 10.0
    """Constant factor in MCTS."""
    max_atoms: int = 20
    """Maximum number of atoms in rationale."""
    min_atoms: int = 8
    """Minimum number of atoms in rationale."""
    prop_delta: float = 0.1
    """Minimum score to count as positive."""
```
Please note that prop_delta should be lower than any prediction score of the hits in order to be able to find rationales. 


### Python notebook for filtering hits and computing rationale scaffolds
  
Dependencies for successful execution of both Python notebooks are described in the "requirements.txt" file. In order to use the Python notebook, run the following commands at the command line:

<ul>
<li>conda install jupyter</li>
<li>conda install notebook</li>
<li>jupyter notebook</li>
</ul>

The last command should launch the Jupyter notebook. All cells in the notebook can then be executed to reproduce the filtering and scaffold computation steps used in the paper; additional comments are provided with each cell to describe what each evaluating is performing. 

### Python notebook for maximal common substructure analyses

This notebook reproduces the maximal common substructure analyses described in Supplementary Note 2, Extended Data Figs. 4 and 5, and the <i>Methods</i> section of the paper. All cells in the notebook can be executed to reproduce the analysis. 



<!-- CONTACT -->
## Contact

For questions or comments about this repository, please reach out to felix j wong at gmail (no spaces, add domain name). 

