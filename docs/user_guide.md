# User Guide for Thermal Conductivity Input File

This guide explains how to create a valid input file for the thermal conductivity processor. The input file specifies rock properties, phases, components, and optional parameter variations to calculate effective thermal conductivity (TC). Follow the structure and rules below to ensure the processor can parse and process the file correctly.

## General Structure
- The input file is a plain text file with lines specifying rock types, problem type, phase data, and optional parameter ranges.
- Lines are separated by newlines, and fields within data lines are separated by whitespace (spaces or tabs).
- Comments start with `#` and are ignored by the processor.
- Blank lines are ignored.
- The file must include a header line defining the columns for phase and component data, followed by data lines.
- Specific lines (e.g., `Rock Type`, `Problem`, `Phases_Volume`) use a `Key: Value` format.

## Required Components
The input file must include the following:

### 1. Rock Type Line
- **Format**: `Rock Type: <rock_type1>,<rock_type2>,...`
- **Description**: Specifies the rock types being analyzed.
- **Allowed Values**: `Carbonates`, `Sandstones` (case-insensitive). Other rock types (e.g., `Organic Rich Mudstone`) are not supported and will trigger a warning but allow processing to continue.
- **Example**: `Rock Type: Carbonates, Sandstones`
- **Note**: At least one rock type is recommended, but the line is optional.

### 2. Problem Line
- **Format**: `Problem: <problem_type> [| <relative_error>]`
- **Description**: Defines the type of problem to solve.
- **Allowed Values**:
  - `Forward`: Calculate effective TC based on input parameters.
  - `Inverse | <relative_error>`: Not currently supported (will exit with an error).
- **Example**: `Problem: Forward`
- **Note**: This line is optional, but `Forward` is assumed if omitted.

### 3. Data Table
- **Header Line**: Defines the columns for phase and component data.
  - **Required Columns** (in order): `Phase`, `Component`, `Thermal_Conductivity`, `Aspect_Ratio`, `Volume`, `Hierarchy`, `Model_Type`
  - **Format**: Columns are separated by whitespace, and the header must match exactly.
- **Data Lines**: Each line represents a component within a phase.
  - **Phase**: Name of the phase (e.g., `Solid_Phase`, `Fluid_Phase`, `Cracks_Phase`).
  - **Component**: Name of the component within the phase (e.g., `Quartz`, `Pore1`).
  - **Thermal_Conductivity**: Thermal conductivity value (numeric, positive, in W/m·K, e.g., `7.0`).
  - **Aspect_Ratio**: Aspect ratio of the component (numeric, positive, e.g., `1.0`, `0.001`).
  - **Volume**: Volume fraction of the component within its phase (numeric, non-negative, e.g., `0.90`).
  - **Hierarchy**: Integer indicating the mixing hierarchy level (e.g., `0`, `1`, `2`). Lower numbers are processed first.
  - **Model_Type**: Mixing model for the phase or `'-'` for single-component phases included in the final mixing step.
    - **Allowed Models**:     
      - `LIH`: Lichtenecker
      - `WIU`: Wiener Upper Bound
      - `WIL`: Wiener Lower Bound
      - `WIA`: Wiener Average
      - `HSU`: Upper Hashin-Shtrikman
      - `HSL`: Lower Hashin-Shtrikman
      - `HSA`: Hashin-Shtrikman Average
      - `GSA`: Generalized Self-consistent
      - `MAX`: Maxwell
      - `BRG`: Bruggeman Effective Medium (alias for `GSA`).
    - **Special Case**: `'-'` indicates the phase has a single component and is mixed only in the final step using its specified TC and aspect ratio.
- **Rules**:
  - All components within a phase must have the same `Hierarchy` and `Model_Type` (or `'-'`).
  - For a phase with `Model_Type='-'`, exactly one component is allowed.
  - The sum of `Volume` values within each phase should be positive (normalized internally).
- **Example**: PRM/tests/Input-1.txt


### 4. Final Mixing Parameters (Required for Multiple Hierarchy Levels)
If the data table contains multiple `Hierarchy` levels, the following lines are required to define the final mixing step. These lines are ignored if all components have the same `Hierarchy` level.

- **Phases_Volume Line** (or legacy `Phase_Volumes`):
- **Format**: `Phases_Volume: <Phase1>=<volume1>,<Phase2>=<volume2>,...`
- **Description**: Specifies the volume fraction of each phase in the final mixing step.
- **Rules**:
  - Must include all phases present in the data table.
  - Volumes must sum to 1.0 (within a tolerance of 0.01).
- **Example**: `Phases_Volume: Solid_Phase=0.80, Fluid_Phase=0.15, Cracks_Phase=0.05`
- **Phases_Aspect_Ratio Line** (or legacy `Aspect_Ratio`, optional):
- **Format**: `Phases_Aspect_Ratio: <Phase1>=<ar1>,<Phase2>=<ar2>,...`
- **Description**: Specifies the aspect ratio for each phase in the final mixing step.
- **Default**: If omitted, defaults to 1.0 for phases without a specified aspect ratio, or uses the volume-weighted average of component aspect ratios.
- **Example**: `Phases_Aspect_Ratio: Solid_Phase=1.0, Fluid_Phase=0.1, Cracks_Phase=0.001`
- **Final_Model_Type Line** (optional):
- **Format**: `Final_Model_Type: <model_type>`
- **Description**: Specifies the mixing model for the final step.
- **Allowed Values**: Same as for component `Model_Type`.
- **Default**: If omitted, defaults to `HSA` (Hashin-Shtrikman Average).
- **Example**: `Final_Model_Type: HSA`

## Optional Parameter Variations
You can specify ranges for parameters to perform sensitivity analysis:

- **Format**: `Parameter_Range: <parameter_name>=<start>:<end>:<steps>`
- **Description**: Creates a linear range of values for the specified parameter.
- **Allowed Parameters**: Any component's `Thermal_Conductivity`, `Aspect_Ratio`, or `Volume`.
- **Example**: `Parameter_Range: Solid_Phase.Quartz.Thermal_Conductivity=5.0:9.0:5`

## Example Input File: PRM/tests/Input-3.txt


## Error Handling
- The processor validates input data and reports errors for:
  - Missing required columns
  - Invalid numeric values
  - Inconsistent hierarchy levels
  - Volume sum mismatches
  - Unsupported rock types or model types
- Warnings are issued for potential issues but processing continues.
- Fatal errors stop execution and must be fixed before proceeding.
