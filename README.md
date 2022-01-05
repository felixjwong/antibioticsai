<!--
*** Thanks for checking out this README Template. If you have a suggestion that would
*** make this better, please fork the repo and create a pull request or simply open
*** an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->




<!-- PROJECT LOGO -->
<br />
<p align="center">

  <h3 align="center">Discovering structural classes of antibiotics with explainable deep learning</h3>

  <p align="center">
    Supporting code for the paper
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Code](#about-the-project)
* [Running the code](#running-the-code)
  * [What the code contains](#what-the-code-contains)
  * [Usage](#usage)
* [Contact](#contact)



<!-- ABOUT THE PROJECT -->
## About the Code

The files in this repository allow for repeat analysis of the one described in the paper "Discovering structural classes of antibiotics with deep learning". The code requires <a href="https://github.com/chemprop/chemprop">ChemProp</a> and Python with the appropriate packages installed, as indicated in the <i>Methods</i> section of the paper. In particular, it is important that at least the following two packages are installed:
<ul>
<li><a href="https://github.com/chemprop/chemprop">Chemprop</a> (commit 9c8ff4074bd89b93f43a21adc49b458b0cab9e7f was used)</li>
<li><a href="https://www.rdkit.org/">RDKit</a> (version 2021.09.01)</li>
</ul>

<!-- GETTING STARTED -->
## Running the code

### Checkpoint files

In the folders "initial_checkpoints" and "final_checkpoints", there are 20 ChemProp checkpoints each corresponding to the initial and final models described in the paper.

### CSV files containing predicted hits

The two files "initial_hits.csv" and "final_hits.csv" contain a list of all the hits predicted by the initial and final models described in the main text, as represented by their SMILES strings. There are 2,668 hits in "initial_hits.csv" and 5,332 hits in "final_hits.csv". For additional information regarding the hits and their prediction scores, please see the Supplementary Information of the paper. 

### Substructure analysis code

"group.ipynb" is the main Python script for performing substructure grouping as detailed in the main text. This file contains a notebook to implement the grouping procedure and requires RDKit to run.


### Usage

The checkpoint directories can be passed directly into ChemProp to generate new predictions. For instance, for SMILES strings in a file "test.csv", run:
```sh
chemprop_predict --test_path test.csv --checkpoint_dir "initial_checkpoints" --preds_path "test_predictions.csv" --features_generator rdkit_2d_normalized --no_features_scaling &
```
To run the substructure analysis, execute the code in "group.ipynb".

<!-- CONTACT -->
## Contact

For questions or comments about this repository, please reach out to felix j wong at gmail (no spaces, add domain name). 

