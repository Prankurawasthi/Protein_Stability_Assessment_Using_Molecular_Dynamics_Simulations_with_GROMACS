## Overview

This repository demonstrates a molecular dynamics (MD) simulation workflow performed using GROMACS for evaluating protein structural stability in an aqueous environment. The project was developed as part of the "Introduction to Molecular Dynamics Simulations" training conducted by Rajiv Gandhi Centre for Biotechnology (RGCB), India.

The workflow follows a standard biomolecular simulation pipeline including protein preparation, system solvation, ion neutralization, energy minimization, equilibration, and trajectory analysis.

## Objectives

* Understand molecular dynamics simulation principles.
* Prepare a protein structure for MD simulation.
* Generate topology and simulation parameter files.
* Perform system solvation and ion addition.
* Run energy minimization and equilibration.
* Evaluate protein stability using trajectory analysis.

## Software

* GROMACS
* Linux Command Line
* VMD / PyMOL (Visualization)

## Workflow

### Step 1: Protein Preparation

Generate topology and processed structure.

```bash
gmx pdb2gmx -f protein.pdb -o protein_processed.gro
```

Outputs:

* protein_processed.gro
* topol.top
* posre.itp

### Step 2: Define Simulation Box

```bash
gmx editconf -f protein_processed.gro -o newbox.gro -bt cubic -d 1.0
```

### Step 3: Solvate the System

```bash
gmx solvate -cp newbox.gro -cs spc216.gro -p topol.top -o solv.gro
```

### Step 4: Add Ions

```bash
gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr

gmx genion -s ions.tpr -o solv_ions.gro -p topol.top -pname NA -nname CL -neutral
```

### Step 5: Energy Minimization

```bash
gmx grompp -f em.mdp -c solv_ions.gro -p topol.top -o em.tpr

gmx mdrun -v -deffnm em
```

### Step 6: NVT Equilibration

```bash
gmx grompp -f nvt.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr

gmx mdrun -v -deffnm nvt
```

### Step 7: NPT Equilibration

```bash
gmx grompp -f npt.mdp -c nvt.gro -t nvt.cpt -r nvt.gro -p topol.top -o npt.tpr

gmx mdrun -v -deffnm npt
```

### Step 8: Production MD Simulation

```bash
gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -o md.tpr

gmx mdrun -deffnm md
```

## Trajectory Analysis

The following analyses can be performed:

* Root Mean Square Deviation (RMSD)
* Root Mean Square Fluctuation (RMSF)
* Radius of Gyration (Rg)
* Hydrogen Bond Analysis
* Solvent Accessible Surface Area (SASA)

Example:

```bash
gmx rms
gmx rmsf
gmx gyrate
gmx hbond
gmx sasa
```

## Skills Demonstrated

* Molecular Dynamics Simulation
* GROMACS
* Linux
* Protein Structure Preparation
* Force Field Selection
* Energy Minimization
* NVT/NPT Equilibration
* Trajectory Analysis
* Structural Stability Assessment

## Training

Introduction to Molecular Dynamics Simulations using GROMACS

Rajiv Gandhi Centre for Biotechnology (RGCB), India

## Author

Prankur Awasthi

Cancer Bioinformatics | Molecular Docking | Molecular Dynamics Simulation
