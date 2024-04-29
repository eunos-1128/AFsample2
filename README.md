# AFsample2
![20240226_mov.gif](20240226_mov.gif)

## Introduction

AFsample2 is a generative protein strutcre prediction system based on AF2 that is able to induce significant conformational diversity for a given protein.

## Installation

1. [Install Miniconda](https://docs.anaconda.com/free/miniconda/miniconda-install/)
2. Setup environment

```
# Clone this repository
git clone https://github.com/iamysk/AFsample2.git
cd AFsample2/

# install dependencies
conda env create -n <env_name> --file=environment.yaml
conda activate <env_name>
python -m pip install -r requirements.txt
```
3. Make sure that all sequence databases are available at ```<data_path>```. Follow the official AlphaFold guide [here](https://docs.anaconda.com/free/miniconda/miniconda-install/) to set up databases. 
```bash
cd scripts
chmod +x download_all_data.sh
./download_all_data.sh <data_path> reduced_dbs
```

4. Install Rosetta suite for clustering tasks ([Download link](https://en.wikipedia.org/wiki/Tar_(computing))). Make sure that a C++ compiler is installed. 

```bash
## Optional. Ignore if compilers already installed
$ sudo apt-get install build-essential      # install C++ compilers

## Unzip tarball and compile
tar -xvzf rosetta[releasenumber].tar.gz
cd rosetta*/main/source
./scons.py -j <num_cores> mode=release bin/rosetta_scripts.mpi.linuxgccrelease       # Significiantly fast with multithreading

Refer to this [guide](https://new.rosettacommons.org/demos/latest/tutorials/install_build/install_build#installing-rosetta) for further details.
```

## Usage

Step-by-step instructions to (1) generate model ensembles (2) Analyze diversity and (3) Clustering and downstream analysis

### Ensemble generation
Follow the steps to generate a diverse conformational ensemble for a given ```<fasta_path>```. 
```bash
'''
# Inputs: 
# <models_to_use>: Path to generated models
# <fasta_paths>: Reference PDB of state1
# <msa_rand_fraction>: Reference PDB of state1

# Outputs:
# <output_dir>: Path to output directory
'''

# Example usage
python AF_multitemplate/run_alphafold.py --models_to_use model_1
                                         --fasta_paths example/8E6Y/8E6Y.fasta      
                                         --output_dir example/8E6Y
                                         --msa_rand_fraction 0.1
                                         --flagfile AFmultitemplate/monomer_full_dbs.flag

```

### Diversity analysis

Analyse generated models to quantify diversity. The following 

```bash
'''
# Inputs: 
# <afout_path>: Path to generated models
# <pdb_state1>: Reference PDB of state1
# <pdb_state1>: Reference PDB of state1

# Outputs:
# final_df_ref1-ref2.csv file saved at results/
'''

# Example usage
python src/analyse_models.py --afout_path examples/8E6Y/ 
                             --pdb_state1 examples/8E6Y/referencea/2fs1_A.pdb 
                             --pdb_state2 examples/8E6Y/referencea/8e6y_A.pdb

```

### Clustering and reference-free state determiantion
```bash
$ pip install af_sample2

```

## How to Cite

Bibtex citation

## Contributing


## License

APACHE
