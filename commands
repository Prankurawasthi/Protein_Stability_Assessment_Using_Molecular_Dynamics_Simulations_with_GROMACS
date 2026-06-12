# GROMACS Lysozyme MD Workflow Commands

## Step 1: Generate Topology

```bash
gmx pdb2gmx -f 1AKI.pdb -o 1AKI_processed.gro -water tip3p
```

## Step 2: Define Simulation Box

```bash
gmx editconf -f 1AKI_processed.gro -o 1AKI_newbox.gro -c -d 1.0 -bt cubic
```

## Step 3: Solvate System

```bash
gmx solvate -cp 1AKI_newbox.gro -cs spc216.gro -o 1AKI_solv.gro -p topol.top
```

## Step 4: Add Ions

```bash
gmx grompp -f ions.mdp -c 1AKI_solv.gro -p topol.top -o ions.tpr

gmx genion -s ions.tpr -o 1AKI_solv_ions.gro -p topol.top -pname NA -nname CL -neutral
```

## Step 5: Energy Minimization

```bash
gmx grompp -f minim.mdp -c 1AKI_solv_ions.gro -p topol.top -o em.tpr

gmx mdrun -v -deffnm em
```

## Step 6: NVT Equilibration

```bash
gmx grompp -f nvt.mdp -c em.gro -p topol.top -o nvt.tpr

gmx mdrun -deffnm nvt
```

## Step 7: NPT Equilibration

```bash
gmx grompp -f npt.mdp -c nvt.gro -t nvt.cpt -p topol.top -o npt.tpr

gmx mdrun -deffnm npt
```

## Step 8: Production MD

```bash
gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -o md_0_1.tpr

gmx mdrun -deffnm md_0_1
```

## Step 9: RMSD Analysis

```bash
gmx rms -s md_0_1.tpr -f md_0_1.xtc -o rmsd.xvg -tu ns
```

## Step 10: Radius of Gyration

```bash
gmx gyrate -s md_0_1.tpr -f md_0_1.xtc -o gyrate.xvg
```

