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


### Heating Curve Method

The heating curve method, implemented in `heatingcurve_method.py`, involves nearly continuous adiabatic heating to determine the melting point.

The process involves:

1. **NVT Thermalization** at the start temperature (Tstart).
2. **NPT Equilibration** at Tstart.
3. **NPT Heating Run** from Tstart to the end temperature (Tend).

The script ensures that no more than 500 atoms are present in the simulation box to keep computations manageable. It is crucial to relax the structure first to avoid potential issues during molecular dynamics simulations.

### Two-Phase Coexistence Method

The two-phase coexistence method predicts melting points by equilibrating a solid and liquid structure in contact under molecular dynamics simulations until they reach a steady state.

#### 1. Creating Solid and Liquid Structures

The script `Meltingpoints_2phase_coexistence.py` is used to create the solid and liquid phases of the material. This involves:
- Loading the initial structure from a CIF file.
- Relaxing the structure to optimize atomic positions and cell size.
- Generating supercells for both the solid and liquid phases.
- Performing molecular dynamics simulations to obtain the liquid phase at the desired temperature.

#### 2. Combining Solid and Liquid Structures

The script `Combining_structures.py` combines the solid and liquid phases into a single structure with a vacuum layer between them. The key steps include:
- Loading the solid and liquid phase structures.
- Creating a new lattice with an appropriate vacuum layer.
- Combining the two phases into one structure and saving it as a CIF file.

#### 3. NVE Simulation

The script `NVE.py` performs the NVE molecular dynamics simulation on the combined structure. The steps include:
- Expanding the combined structure if necessary.
- Performing ionic relaxation to optimize atomic positions while keeping the cell size fixed.
- Initializing velocities according to the desired temperature.
- Running the NVE molecular dynamics simulation to estimate the melting point.


## Dependencies

- `chgnet` for CHGNet model and molecular dynamics simulations.
- `pymatgen` for structure manipulation and CIF file handling.
- `ase` for molecular dynamics and structure visualization.
- `numpy` and `matplotlib` for numerical operations and plotting.


## Notes

- Ensure that the CIF files and GPU settings are properly specified in each script.
- The scripts assume that the lattice parameters and angles of the solid and liquid phases are compatible for combining.
- For the purpose of this repository, namely showing code representative of my work, I have changed nothing to code already created, and just put it here with this readme file
- With the insights I have now, a year later, I would change a few things, especially for larger scale projects:
	- Create a more clear workflow
	- Remove the redundancies across scripts
	- Implementing usefull classes
	- More error handling through try and except statements
	- Add unit tests
