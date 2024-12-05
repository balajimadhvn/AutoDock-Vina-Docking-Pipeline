# AutoDock Vina Docking Pipeline

This pipeline automates the process of docking ligands with a receptor using AutoDock Vina. The pipeline fetches the receptor from the Protein Data Bank (PDB) and the ligands from PubChem, prepares both the receptor and ligands by adding charges, converting to 3D, minimizing energy, and converting them to PDBQT format. It then performs docking with AutoDock Vina, and finally extracts the best binding scores into a CSV file.

## Prerequisites

- **Snakemake**: Workflow management system
- **AutoDock Vina**: Molecular docking software
- **Open Babel**: Chemical toolbox used for file conversion and charge addition
- **Conda**: Package manager for setting up the environment

## Setup

1. **Clone the repository:**

   ```bash
   git clone https://github.com/balajimadhvn/AutoDock-Vina-Docking-Pipeline.git
   cd docking-pipeline

    Create the Conda environment:

    The repository includes an environment.yml file which defines the required dependencies. To create the Conda environment, run the following command:

conda env create -f environment.yml

Activate the Conda environment:

Once the environment is created, activate it using:

conda activate docking_env

Install Snakemake:

If Snakemake is not already installed, you can install it using:

    conda install -c bioconda snakemake

Input Files

    input_ids.txt: This file contains the PDB ID of the receptor and the PubChem CIDs of the ligands you want to dock. The format should be as follows:

PDB_ID: [Your PDB ID here]
PubChem_ID: [PubChem CID of ligand 1]
PubChem_ID: [PubChem CID of ligand 2]
PubChem_ID: [PubChem CID of ligand 3]
...

Example:

    PDB_ID: 1A2C
    PubChem_ID: 123456
    PubChem_ID: 789012
    PubChem_ID: 345678

    Ensure that the input_ids.txt file is placed in the root directory of this repository.

Directory Structure

Before running the pipeline, make sure the following directories are created:

    receptor/ — To store the fetched receptor files.
    ligands/ — To store the fetched ligand files.
    docked/ — To store the docked ligand output.
    logs/ — To store the docking logs.

You can create these directories manually or Snakemake will do it for you (if configured correctly).
Workflow Overview

    Fetch Receptor: The receptor (PDB file) is downloaded from the Protein Data Bank (PDB) using the provided PDB ID.
    Fetch Ligands: Ligands are downloaded in SDF format from PubChem using the PubChem CIDs provided.
    Prepare Receptor: The receptor is prepared by converting it to PDBQT format, adding partial charges, and converting to 3D using Open Babel.
    Prepare Ligands: Ligands are converted to 3D, minimized in energy, and converted to PDBQT format with charges using Open Babel.
    Docking: AutoDock Vina is used to dock each ligand with the receptor.
    Extract Affinities: After docking, the best binding scores for each ligand are extracted from the Vina log files and stored in a CSV file (affinities.csv).

Running the Pipeline

    Prepare Input File: Ensure that input_ids.txt is in the root directory with the proper format.

    Run Snakemake: Once your environment is set up and the input file is ready, run the following command to start the docking process:

    snakemake --cores 1

    This will execute the workflow, perform the docking, and create an affinities.csv file containing the best docking scores for each ligand.

    Check Results:
        The docked ligands will be saved in the docked/ directory.
        The docking logs will be saved in the logs/ directory.
        The affinities (best docking scores) will be saved in the affinities.csv file.

Configuration

    Vina Configuration: The Vina configuration file (vina.conf) is automatically generated with default values for the docking box (centered at (0, 0, 0) with dimensions 20x20x20 Å). You can modify this file to change the docking grid parameters as needed.

Troubleshooting

    Ensure that the required dependencies (AutoDock Vina, Open Babel, Snakemake) are correctly installed and available in the conda environment.
    If you encounter errors related to file paths or directories, make sure the necessary directories (receptor/, ligands/, docked/, logs/) exist before running the pipeline.
    If the ligands are not fetched correctly, verify that the PubChem CIDs in the input_ids.txt file are valid.
