<!--
*** Thanks for checking out this README Template. If you have a suggestion that would
*** make this better, please fork the repo and create a pull request or simply open
*** an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->




<!-- PROJECT LOGO -->
<br />
<p align="center">

  <h3 align="center">Evidence that coronavirus superspreading is fat-tailed</h3>

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

The code in this repository allows for repeat and independent analysis of the one described in the paper "Evidence that coronavirus superspreading is fat-tailed", by Felix Wong and James J. Collins. The code requires MATLAB 2019b or later to run. 

<!-- GETTING STARTED -->
## Running the code

### Data files

In the folder "SSEs", three different datasets are provided as detailed in the main text. Load any single one of them before proceeding with the analysis code below.

### Data analysis code

There are a total of seven files included in the main folder:

* sspreader.m
<p>
This file is the main analysis script. It loads all the data and generates most figures and analyses shown in Fig. 1 of the main text.
</p>

* sspreader_robustness.m
<p>
This file performs the robustness check shown in Fig. 1H of the main text.
</p>

* merging_imputation.m
<p>
This file performs the robustness check shown in Fig. 1I of the main text.
</p>

* cross_comparison.m
<p>
This file plots the summary statistics shown in Fig. 1J,K of the main text.
</p>

* hillestimator.m
<p>
Auxiliary script for computing the Hill estimator. 
</p>

* pickandestimator.m
<p>
Auxiliary script for computing the Pickands estimator. 
</p>

* DedHestimator.m
<p>
Auxiliary script for computing the Dekkers-Einmahl-de Haan estimator. 
</p>

* polyparci.m
<p>
Auxiliary script for estimating polynomial fit confidence intervals.
</p>

* frechetmodeling.m
<p>
Script for modeling the maxima of samples of a Fr&eacute;chet distribution.
</p>

<p>
<b>Please note:</b> Datasets S1 and S2 of the paper contains more details of, and references for, superspreading events.
</p>


### Network model of transmission code

In the folder "network", there are MATLAB scripts for reproducing the network models of transmissions detailed in the main text.

* graph_model.m
<p>
This file is the main simulation script. It generates BA or WS random graphs and runs the simulations shown in Fig. 2E,F of the main text.
</p>

* SEIR.m
<p>
This file simulates a well-mixed SEIR model and plots the resultant total infected curve.
</p>

* WattsStrogatz.m
<p>
Auxiliary script for generating a WS random graph with specified parameters.
</p>


### Usage

To run the main analysis, simply load any one of the datasets in "SSEs", then run in MATLAB:
```sh
sspreader
```
Most analysis results should appear. To run robustness checks after the main analysis has been executed, run in MATLAB the relevant script, e.g., 
```sh
sspreader_robustness
```
To model removing the tails and taking the maxima of samples of a Fr&eacute;chet distribution, run in MATLAB:
```sh
frechetmodeling
```
To model simulations of a network-based model of transmission or a well-mixed SEIR model, run in MATLAB:
```sh
graph_model
```
or
```sh
SEIR
```

<!-- CONTACT -->
## Contact

For questions or comments about this code, please reach out to felix j wong at gmail (no spaces, add domain name). 

