# Mechanical Property Extrapolation for Friction Correction

## Overview
This project provides a Python-based solution for extrapolating compressive stress-strain data to ideal frictionless conditions (m=0) using linear regression over multiple specimen geometries. The method corrects for friction effects in mechanical testing by analyzing data from specimens with different diameter-to-height ratios.

## Features
- **Data Cleaning**: Removes duplicate strain values and averages corresponding stresses
- **Common Range Identification**: Finds overlapping strain ranges across multiple datasets
- **Cubic Spline Interpolation**: Creates smooth stress-strain curves for consistent comparison
- **Linear Extrapolation**: Extrapolates stress to m=0 condition at each strain point
- **Quality Assessment**: Calculates R² values to evaluate extrapolation reliability
- **Visualization**: Generates comprehensive plots of results and analysis
- **Data Export**: Saves corrected frictionless curve to CSV format

## Theoretical Background
The method assumes linear relationship between stress (σ) and diameter-to-height ratio (m = d/h) at constant strain:

**σ(ε, m) = A(ε)·m + σ₀(ε)**

Where:
- σ(ε, m) = measured stress at strain ε and geometry m
- A(ε) = slope representing friction sensitivity
- σ₀(ε) = extrapolated stress at m=0 (frictionless condition)

By measuring specimens with different m values and extrapolating to m=0, we obtain the ideal frictionless stress-strain curve.

## Installation Requirements

```bash
pip install numpy pandas matplotlib scipy
```

## Usage

### 1. Prepare Input Data
Create CSV files containing experimental stress-strain data:
- `data_m1.csv`: Data for m = 0.504032 (Al20)
- `data_m2.csv`: Data for m = 0.678161 (Al15)  
- `data_m3.csv`: Data for m = 0.988142 (Al10)

Each CSV should have two columns: Strain and Stress (MPa)

### 2. Run the Script
```python
python interpolation.py
```

### 3. Output Files
- **friction_corrected_curve.csv**: Contains the extrapolated frictionless curve
- **Visual plots**: Four-panel visualization of results

## Code Structure

### Main Functions:

#### `clean_duplicate_strain(data)`
- Removes duplicate strain values by averaging corresponding stresses
- Returns sorted, cleaned data

#### `interpolate_stress(strain_data, stress_data, target_strains)`
- Performs cubic spline interpolation to common strain points

#### `get_common_strain_range(all_data)`
- Identifies overlapping strain range across all datasets

#### `extrapolate_to_m0(m_values, all_data, num_points=50)`
- Main extrapolation function
- Returns extrapolated stress at m=0 and statistical details

#### `plot_results(...)`
- Generates comprehensive visualization:
  - Original and extrapolated curves
  - Linear extrapolation at sample strains
  - Slope variation with strain
  - R² quality metrics

#### `save_results(...)`
- Exports results to CSV format

## Output Description

### Data Output (CSV):
- **Strain**: Common strain points
- **Stress_m0_MPa**: Extrapolated stress at m=0
- **Slope_dSigma_dm**: Sensitivity of stress to geometry
- **R_squared**: Quality of linear fit at each strain

### Visual Output (4-panel plot):
1. **Top-left**: Original stress-strain curves and extrapolated m=0 curve
2. **Top-right**: Linear extrapolation at selected strain values
3. **Bottom-left**: Variation of slope (dσ/dm) with strain
4. **Bottom-right**: R² values indicating extrapolation quality

## Example Results

```
Extrapolation Statistical Summary:
==================================================
Mean R²: 0.9743
Min R²: 0.9214 at strain 0.127
Max R²: 0.9987 at strain 0.358
Mean slope: 145.67 MPa
Output strain range: 0.070 to 0.470
```

## Interpretation Guidelines

1. **High R² (>0.95)**: Reliable extrapolation, linear assumption valid
2. **Low R² (<0.90)**: Potential issues with data or linear assumption
3. **Slope magnitude**: Indicates friction sensitivity
4. **Corrected curve**: Represents ideal frictionless material response

## Applications
- Material property characterization
- Friction effect quantification in compression testing
- Comparative analysis of lubricant effectiveness
- Finite element model calibration
- Quality control in mechanical testing

## Limitations and Assumptions
1. Linear relationship between stress and m (valid for small m variations)
2. Similar deformation mechanisms across different geometries
3. Consistent testing conditions and material homogeneity
4. Adequate number of geometry variations (minimum 3 recommended)

## Future Improvements
- Non-linear extrapolation models
- Uncertainty quantification
- Batch processing for multiple materials
- Integration with FEM software
- Real-time monitoring during testing

---
*This tool was developed for academic research in mechanical property characterization and friction correction in materials testing.*
