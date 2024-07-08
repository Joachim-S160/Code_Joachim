# Melting Point Prediction using Discrete, Heating Curve, and Two-Phase Coexistence Methods

This repository contains source code for predicting the melting points of halides using the CHGNet model with three methods: the discrete temperature density method, the heating curve method, and the two-phase coexistence method. Additionally, the repository includes scripts for calculating cohesive energy and combining structures. Note that this repository includes only source code and scripts; it does not include data analysis or results.

## Files in this Repository

- `Internship_report_Joachim_.pdf`: Detailed report of the internship project including methodologies and results.
- `Meltingpoints_discrete_Tr.py`: Python script for running molecular dynamics simulations to predict melting points using the discrete temperature density method.
- `heatingcurve_method.py`: Python script for running molecular dynamics simulations using the heating curve method.
- `Meltingpoints_2phase_coexistence.py`: Python script for running molecular dynamics simulations using the two-phase coexistence method.
- `Cohesive_energy.py`: Python script for calculating the cohesive energy of a structure using the CHGNet model.
- `Combining_structures.py`: Python script for combining a solid and a fluid structure into a single structure for the two-phase coexistence method.
- `relax.py`: Python script for relaxing a structure before combining phases or running simulations.
- `NVE.py`: Python script for running an NVE (constant number of particles, volume, and energy) molecular dynamics simulation.
- `README.md`: This file, providing an overview of the repository.
- `get_data.py`: General-purpose script used to process and analyze simulation data.

## Project Description

### Discrete Temperature Density Method

The method implemented in `Meltingpoints_discrete_Tr.py` is based on the "heat-until-melts" approach. This involves performing multiple constant pressure and temperature simulations on a solid material. The temperature at which the material's density shows a discontinuity indicates the melting point.

However, it should be noted that:

- **The heat-until-melts method, with discrete points, is certainly not ideal.** This method provides only an upper bound to the melting point due to superheating effects, which can result in overestimations of the transition temperature.

Given these limitations, future work will focus on implementing the heating curve method, which involves nearly continuous adiabatic heating, to address the limitations of the discrete point approach.

### Heating Curve Method

The heating curve method, implemented in `heatingcurve_method.py`, involves nearly continuous adiabatic heating to determine the melting point. This method is expected to provide a more accurate estimation of the melting point by reducing the effects of superheating.

The process involves:

1. **NVT Thermalization** at the start temperature (Tstart).
2. **NPT Equilibration** at Tstart.
3. **NPT Heating Run** from Tstart to the end temperature (Tend).

The script ensures that no more than 500 atoms are present in the simulation box to keep computations manageable. It is crucial to relax the structure first to avoid potential issues during molecular dynamics simulations.

### Two-Phase Coexistence Method

The two-phase coexistence method, implemented in `Meltingpoints_2phase_coexistence.py`, involves performing molecular dynamics simulations on both solid and liquid phases of a material to find the temperature at which both phases coexist.

The process involves:

1. **Relaxing the Solid Structure:** The structure is relaxed to ensure that atoms are in positions of lower potential energy. This step is performed using the `relax.py` script, which uses the CHGNet model to optimize the atomic positions of the structure while keeping the cell size fixed.

2. **Combining Structures:** After relaxation, the `Combining_structures.py` script combines the relaxed solid and fluid structures into a single structure, incorporating a vacuum layer to separate the phases and avoid interactions at the boundaries. This combined structure is saved as a CIF file and used for further simulations.

3. **Running Molecular Dynamics Simulations:** Molecular dynamics simulations are then conducted on the combined structure to determine the melting point. The `Meltingpoints_2phase_coexistence.py` script runs these simulations and analyzes the phase coexistence to estimate the melting temperature.

**Important Note:** Ensure that the structure is relaxed before combining phases or running simulations to avoid instability and inaccuracies.

#### Relaxation Script

The `relax.py` script is used to relax a structure before combining phases or running molecular dynamics simulations. This script performs the following tasks:

- **Loading the Structure and Model:** It loads the structure from an input file and the CHGNet model.
- **Performing Relaxation:** It performs ionic relaxation to minimize the potential energy of the structure while keeping the cell size fixed.
- **Saving the Relaxed Structure:** The relaxed structure is saved to a CIF file for further use in combining structures or running simulations.

#### Combining Structures

The `Combining_structures.py` script combines the solid and fluid phases into a single structure for the two-phase coexistence method. The script performs the following steps:

1. **Loading Structures:** It loads the solid and fluid structures from CIF files.
2. **Creating Vacuum Layer:** It adds a vacuum layer between the solid and fluid phases to prevent interactions at the boundaries.
3. **Combining Structures:** It places the solid and fluid phases into a combined lattice with the vacuum layer and saves the final structure as a CIF file.

#### NVE Simulation Script

The `NVE.py` script runs an NVE (constant Number of particles, Volume, and Energy) molecular dynamics simulation. This script performs:

1. **Loading the Structure:** It reads the structure from a CIF file using ASE.
2. **Structure Expansion and Relaxation:** It expands the structure slightly, performs relaxation to minimize potential energy, and saves the relaxed structure.
3. **Running the NVE Simulation:** It initializes the simulation with Maxwell-Boltzmann distributed velocities at the specified temperature and runs the simulation, storing the trajectory and log files.

**Usage of `NVE.py`:**

- The script begins by loading the CHGNet model and the structure from an input file.
- It optionally expands the structure and performs ionic relaxation.
- It then sets up and runs an NVE molecular dynamics simulation, storing results in trajectory and log files.

This section ensures that structures are properly prepared and simulations are conducted accurately, contributing to reliable melting point predictions.
