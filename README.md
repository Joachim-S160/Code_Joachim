# Melting Point Prediction using Discrete, Heating Curve, and Two-Phase Coexistence Methods

This repository contains source code for predicting the melting points of halides using the CHGNet model with three methods: the discrete temperature density method, the heating curve method, and the two-phase coexistence method. Additionally, the repository includes a script for calculating cohesive energy. Note that this repository includes only the source code and scripts; it does not include data analysis or results.

## Files in this Repository

- `Internship_report_Joachim_.pdf`: Detailed report of the internship project including methodologies and results.
- `Meltingpoints_discrete_Tr.py`: Python script for running molecular dynamics simulations to predict melting points using the discrete temperature density method.
- `heatingcurve_method.py`: Python script for running molecular dynamics simulations using the heating curve method.
- `Meltingpoints_2phase_coexistence.py`: Python script for running molecular dynamics simulations using the two-phase coexistence method.
- `Cohesive_energy.py`: Python script for calculating the cohesive energy of a structure using the CHGNet model.
- `README.md`: This file, providing an overview of the repository.
- `get_data.py`: General purpose script used to process and analyze simulation data.

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

1. **Relaxing the Solid Structure:** The structure is relaxed to ensure that atoms are in positions of lower potential energy.
2. **Creating Supercell:** A supercell of the relaxed solid structure is created.
3. **Molecular Dynamics Simulations:** Simulations are run to obtain the fluid phase at a specified temperature. The results are stored in trajectory and log files.

**Important Note:** The molecular dynamics simulation can become unstable if the structure is not relaxed first. Ensure to follow the relaxation step before running the simulations.

### Cohesive Energy Calculation

The `Cohesive_energy.py` script calculates the cohesive energy of a structure using the CHGNet model. Cohesive energy is the energy needed to transform one mole of a crystalline solid at 0 K to isolated gas-phase molecules. This script involves:

1. **Relaxing Structures:** Both the unit cell and a single molecule are relaxed to minimize potential energy.
2. **Energy Prediction:** The CHGNet model predicts the energy of the relaxed unit cell and the relaxed single molecule.
3. **Cohesive Energy Calculation:** The cohesive energy is computed as the difference between the predicted energy of the unit cell and the single molecule.

The `plot_cohesive_energy` function generates a plot of cohesive energy versus material.

### Generating Images

Previously, the `get_data.py` script was used to process the simulation data and generate images showing various plots. These images provided visual insights into the behavior of the system at various temperatures and times, helping to identify the melting point. As of the latest update, these images have been removed.

## Results and Discussion

For detailed results and discussions on the methodologies and findings, please refer to the `Internship_report_Joachim_.pdf` file. This report contains comprehensive analysis and interpretations of the data obtained from the different melting point prediction methods and cohesive energy calculations.

## Note

This repository contains only the source code and scripts used for melting point prediction and cohesive energy calculations. Data analysis and detailed results are not included here. Please refer to the `Internship_report_Joachim_.pdf` for results and discussions.

## Conclusion

This repository provides a comprehensive set of tools and scripts for predicting melting points using the CHGNet model with discrete, heating curve, and two-phase coexistence methods, as well as for calculating cohesive energy. The scripts and methodologies can be adapted for other similar studies in material science.

For more details, refer to the `Internship_report_Joachim_.pdf`.
