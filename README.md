# ASIRK-3E: Data and Computational Records

Supporting material for **An Efficient Additive Semi-Implicit Runge-Kutta Method for Compressible Reacting Flows with Stiff Chemistry**, by Yao Xiao, Xinliang Guo, and Zhenxun Gao.

Prepared on 9 November 2026. Figure references below follow the current 64-page manuscript, `ASIRK3E_C&F.pdf`.

This repository contains Tecplot layout packages, computational records, and two vortex-structure images for the benchmark and reacting-flow applications. The 56 supplied data/record files occupy approximately 6.3 MB. Recorded results are preserved; the efficiency table presents final wall-clock costs and relative errors.

## Directory

```text
./
|-- README.md
|-- MANIFEST.csv
|-- SHA256SUMS.txt
|-- section_5_1_burgers/
|   |-- case_1_solution.lpk
|   |-- case_1_convergence.lpk
|   |-- case_4_solution.lpk
|   `-- case_4_convergence.lpk
|-- sections_5_2_5_3_brusselator/
|   |-- weak_profiles.lpk
|   |-- weak_convergence_and_cost.lpk
|   |-- nonequilibrium_profiles.lpk
|   |-- nonequilibrium_convergence_and_cost.lpk
|   `-- records/
|       |-- all_runs.csv
|       |-- sweep.log
|       |-- reference_checks.log
|       |-- reference_bdf_check.log
|       |-- linear_stability_limits.csv
|       |-- profile_runs.csv
|       |-- profile_runs.log
|       `-- linearized_theory_check.csv
|-- section_6_1_reacting_shock_tube/
|   |-- shock_tube_temperature_profiles.lpk
|   |-- shock_tube_error_cost.lpk
|   `-- records/
|       |-- efficiency_measurements.csv
|       |-- efficiency_analysis.json
|       |-- temperature_profile_audit.json
|       `-- logs/
|           |-- erk3/       (comparison runs and continuation reference)
|           |-- asirk3c/
|           |-- asirk3e/
|           |-- ark3/
|           |-- strang_sp/
|           `-- wu_sbs/
|-- section_6_2_methane_air_mixing_layer/
|   |-- mixing_layer_statistics.lpk
|   `-- records/
|       |-- asirk3e.log
|       `-- erk3.log
`-- section_6_3_hydrogen_air_mixing_layer/
    |-- q_criterion_inert.png
    |-- q_criterion_reacting.png
    `-- velocity_statistics.lpk
```

The `section_6_3` folder identifies the three-dimensional application for data organization. In the current manuscript, this material is included in the latter part of Section 6.2, rather than a separately numbered Section 6.3.

## Contents and manuscript correspondence

| Folder | Contents | Related manuscript figures |
|---|---|---|
| `section_5_1_burgers` | Viscous Burgers benchmark: selected cases 1 and 4; solution profiles and temporal convergence | Figs. 2-6 |
| `sections_5_2_5_3_brusselator` | Weak-perturbation and nonequilibrium advection-diffusion-reaction benchmarks; final profiles, error versus step size and cost, and shared calculation records | Figs. 7-10; records also support Tables 1-3 |
| `section_6_1_reacting_shock_tube` | Temperature profiles, error-cost data, analysis records, and 29 original run logs | Figs. 11-12; recorded measurements support the interpolation used for Fig. 13 |
| `section_6_2_methane_air_mixing_layer` | Two-dimensional methane-air mixing-layer statistics and the latest logs from the two selected runs | Figs. 15-16 |
| `section_6_3_hydrogen_air_mixing_layer` | Inert/reacting Q-criterion images and velocity-statistics comparison with experimental data | Figs. 17-18 |

### Section 5.1: Burgers benchmark

Case numbers retain the original selection: case 1 has viscosity `nu = 0.001`, and case 4 has `nu = 0.1`. The solution packages contain numerical and analytical profiles; the convergence packages contain relative space-time L2 errors versus time step. The case 1 solution package also supplies the data for the enlarged view in Fig. 3.

### Sections 5.2 and 5.3: Reaction benchmarks

Both initial conditions use the five stiffness parameters `epsilon = 1e-2, 1e-3, 1e-4, 1e-6, 1e-8` and final time `T = 0.2`. 

- `all_runs.csv` contains the complete recorded fixed-step sweep, including both Strang variants, failed/nonfinite results, and the acceptance flag. Columns identify `epsilon`, initial-condition `case`, `method`, step count `N`, `dt`, `relative_error`, `walltime_s`, and `valid_le_0p1`. Wall-clock times are in seconds. Nonfinite or rejected entries must not be treated as successful runs.
- `sweep.log` preserves the original execution record. The reference logs document high-accuracy reference calculations and checks. `linear_stability_limits.csv` contains the modal stability estimates and sampled step limits.
- `profile_runs.csv` and `profile_runs.log` record the runs selected for the final profiles. Their recomputation timings are separate from the sweep timings used in the cost plots.
- The weak-profile theoretical curves represent the analytical solution of the equilibrium-linearized continuous equations. The convergence errors instead use a high-accuracy numerical reference for the nonlinear semi-discrete equations. `linearized_theory_check.csv` records their differences.

The plotted **Strang-Sp** selection uses implicit midpoint at `epsilon = 1e-2, 1e-3, 1e-4`, and SDIRK2 at `1e-6, 1e-8`. Both branches remain distinguishable in the original records. Historical internal labels `ASIRK3F` or `ASIRK-3F` in the selected records refer to the method reported as ASIRK-3E.

### Section 6.1: Reacting shock tube

`shock_tube_temperature_profiles.lpk` contains temperature comparisons at 170, 190, and 230 microseconds. Its ERK3 reference uses a 1 ns time step. `temperature_profile_audit.json` records source values, hashes, and the displayed-method selection, including additional curves not shown in the temperature plot.

`shock_tube_error_cost.lpk` contains relative centerline L2 errors at 170 microseconds, with cost measured over 130-180 microseconds. Only runs that successfully complete the full 130-180 microsecond interval are shown in the error-cost comparison.

`efficiency_measurements.csv` lists the methods, time steps, final wall-clock times (in seconds) for the 130-180 microsecond interval, and relative errors at 170 microseconds. The total cost is given directly in `total_wall_time_s`.

The efficiency reference uses an ERK3 time step of 1.5625 ns. It is a continuation reference, distinct from the 1 ns reference used for the temperature profiles. `efficiency_analysis.json` retains the original reference status, initial/restart-field comparisons, error definition, and timing definition.

### Sections 6.2 and 6.3: Mixing layers

The two-dimensional package contains first- and second-order statistics at `x/delta_omega = 600`. The selected ASIRK-3E and ERK3 logs use 4 ns and 1 ns, respectively, and record three NVIDIA A100 GPUs. The ASIRK-3E log retains its historical internal name, ASIRK-3F. These are records for the two selected statistical runs; they do not provide the complete timing evidence for every method in the manuscript's efficiency table.

The three-dimensional package contains velocity-fluctuation statistics at `x/delta_omega,0 = 800`, including the experimental comparison. The two PNG files show instantaneous Q-criterion visualizations for the inert and reacting cases. Full three-dimensional flow fields and the multi-gigabyte Q-criterion data packages are omitted because of their size.

## Contact

For further reasonable requests concerning the data, please contact gaozhenxun@buaa.edu.cn or yxiao@buaa.edu.cn. We welcome further discussion.
