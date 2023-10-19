![header](support/logo.png)   

# Unsupervised and supervised discovery of tissue cellular neighborhoods from cell phenotypes with CytoCommunity



## Contents

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Maintainers](#maintainers)
- [Citation](#citation)

## Overview

<div align=center><img src="https://github.com/huBioinfo/CytoCommunity/blob/main/support/overview.png" width="650" height="650" alt="pipline"/></div>  

It remains poorly understood how different cell phenotypes organize and coordinate with each other to support tissue functions. To better understand the structure-function relationship of a tissue, the concept of tissue cellular neighborhoods (TCNs) has been proposed. Furthermore, given a set of tissue images associated with different conditions, it is often desirable to identify condition-specific TCNs with more biological and clinical relevance. However, there is a lack of computational tools for de novo identification of condition-specific TCNs by explicitly utilizing tissue image labels. 

We developed the CytoCommunity algorithm for identifying TCNs that can be applied in either an unsupervised or a supervised learning framework. The direct usage of cell phenotypes as initial features to learn TCNs makes it applicable to both single-cell transcriptomics and proteomics data, with the interpretation of TCN functions facilitated as well. Additionally, CytoCommunity can not only infer TCNs for individual images but also identify condition-specific TCNs for a set of images by leveraging graph pooling and image labels, which effectively addresses the challenge of TCN alignment across images.

CytoCommunity is the first computational tool for end-to-end unsupervised and supervised analyses of single-cell spatial maps and enables direct discovery of conditional-specific cell-cell communication patterns across variable spatial scales.

## Installation

### Hardware requirement 

CPU: i7

Memory: 16G or more

Storage: 10GB or more

### Software requirement

Conda version: 22.9.0

Python version: 3.10.6

R version: >= 4.0 suggested

Clone this repository (v1.0.0) and cd into it as below, or directly download this CytoCommunity version at https://github.com/huBioinfo/CytoCommunity/releases/tag/v1.0.0
```
git clone https://github.com/huBioinfo/CytoCommunity.git
cd CytoCommunity
```
### For Windows

#### Preparing the virtual environment

1. Create a new conda environment using the environment.yml file or the requirements.txt file with one of the following commands:

    ```bash
    conda env create -f environment.yml
    # or
    conda create --name CytoCommunity --file requirements.txt
    ```

Note that the command should be executed in the directory containing the environment.yml or requirements.txt file. And if you use the .txt file, please convert it to the UTF-8 format.

Alternatively, the requirements can also be installed directly in a new conda environment: 
```
conda create --name CytoCommunity pyhton=3.10.6
conda activate CytoCommunity
conda install --yes --file requirements.txt
```

2. Install the diceR package (R has already been included in the requirements) with the following command:

    ```bash
    R.exe
    > install.packages("diceR")
    ```

### For Linux

#### Preparing the virtual environment 

1. Create a new conda environment using the environment_linux.yml file and activate it:

    ```bash
    conda env create -f environment_linux.yml
    conda activate CytoCommunity
    ```

2. Install R and the diceR package:
    
    ```bash
    conda install R
    R
    > install.packages("diceR")
    ```

The whole installation should take less than 20 minutes.
    
## Usage

The CytoCommunity algorithm for TCN indentification can be used in either an unsupervised or a supervised learning mode. It consists of two components: a soft TCN assignment learning module and a TCN ensemble module to determine the final robust TCNs.

You can apply CytoCommunity algorithm in the following five steps:

 - Step0: Constructing KNN graghs.

 - Step1: Importing data.

 - Step2: Performing soft TCN asssignment for cells through supervised or unsupervised learning.

 - Step3: Conducting TCN ensemble for more robust result.

 - Step4: Visualization of the final TCN map.

You can see [Documentation and Tutorials](https://cytocommunity.readthedocs.io/en/latest/index.html) to get easier start and also reproduce TCN partitions shown in the paper using the commands below. The associated code scripts and example input data can be found in the folder "Tutorial".

### Unsupervised CytoCommunity

The example input data to the unsupervised learning mode of CytoCommunity is derived from a mouse brain MERFISH dataset generated in [2], including cell type label, cell spatial coordinate files and an image name list. These files can be found under the directory "Tutorial/Unsupervised/MERFISH-Brain_Input/".

Run the following steps in Windows Powershell or Linux Bash shell:

#### 1. Use Step1 to construct KNN-based cellular spatial graghs and convert the input data to the standard format of torch.

```bash
conda activate CytoCommunity
cd Tutorial/Unsupervised
python Step1_ConstructCellularSpatialGraphs.py
```

#### 2. Use Step2 to perform soft TCN assignment learning in an unsupervised fashion.

This step generates a folder "Step2_Output" including multiple runs (subfolders) of soft TCN assignment learning module. Each subfolder contains a cluster adjacent matrix, a cluster assignment matrix, a node mask file and a loss recording file.

```bash
python Step2_TCNLearning_Unsupervised.py
```

#### 3. Use Step3 to perform TCN assignment ensemble.

The result of this step will be saved in the "Step3_Output/TCNLabel_MajorityVoting.csv" file. Make sure that the diceR package has been installed before Step3.

```bash
Rscript Step3_TCNEnsemble.R
```

#### 4. Use Step4 to visualize single-cell spatial maps colored based on cell type annotations and final TCN partitions.

After this step, we will obtain two single-cell saptial maps (in PNG and PDF formats) colored by input cell type annotations and identified TCNs, respectively.

```bash
python Step4_ResultVisualization.py
```
The running time for a sample/image in this dataset should take around 12 minutes.



## Maintainers

Yafei Xu(22031212416@stu.xidian.edu.cn)

Yuxuan Hu (huyuxuan@xidian.edu.cn)

Kai Tan (tank1@chop.edu)

## Citation

* Hu Y, Rong J, Xie R, Xu Y, Peng J, Gao L, Tan K. Learning predictive models of tissue cellular neighborhoods from cell phenotypes with graph pooling. *bioRxiv*, 2022.
 
    https://www.biorxiv.org/content/10.1101/2022.11.06.515344v1


