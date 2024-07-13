# Emergency Services Location optimisation using DSM for maximizing coverage, minimizing the response time for Fatal Accidents on Hyderabad Outer Ring Road :motorway:
## [Refer to the Execution Guide PDF/Doc for using the modules](https://github.com/AGAMPANDEYY/Ambulance-Optimisation-EfkonStrabag/blob/main/execution-guide/Execution%20Guide%20for%20Ambulance%20Optimisation.pdf)

## This repo contains:
- Algorithms and Source Codes
- Outputs as .csv
- Execution Guide
- Reference materials
- Presentation discussing the solution

  # 🚑 Ambulance Optimization Quick Guide - EfkonStrabag 🚑

Welcome to the **Ambulance Optimization** repository! This project aims to optimize the deployment of ambulances on highways using accident clustering data and mathematical optimization.

## Overview

This repository contains several modules to process raw data, cluster demand points, calculate travel times, and optimize ambulance deployment using the PuLP library. Below is a step-by-step guide on how to run each module on a custom dataset.

## Table of Contents

- [Pre-processing](#1-pre-processing)
- [Clustering](#2-clustering)
- [OD Time Matrix](#3-od-time-matrix)
- [PuLP Optimization Module](#4-pulp-optimization-module)
- [Additional Resources](#additional-resources)
- [Installation](#installation)
- [Contributing](#contributing)
- [License](#license)

---

## 1. Pre-processing

### Description

This module maps accident chainage to the nearest equipment's latitude and longitude. Use this step if your dataset lacks accident lat-lon and frequency.

### Input Fields

- `Accident_Chainage`
- `Accident_Frequency`
- `Equipment_Chainage`
- `Equipment_Lat`
- `Equipment_Lon`

### Output

- `.csv` file with `Accident_Chainage`, `Accident_Lat`, `Accident_Lon`

### Execution Steps

1. **Prepare your raw dataset** with the mandatory fields.
2. **Run the pre-processing script**:
    ```sh
    python pre_processing.py --input <path_to_raw_dataset> --output <path_to_processed_output>
    ```

## 2. Clustering

### Description

This module clusters the processed accident data to form demand points.

### Input Fields

- `Accident_Chainage`
- `Accident_Lat`
- `Accident_Lon`

### Output

- `.csv` file with `Cluster_ID`, `Demand_Point_Lat`, `Demand_Point_Lon`, `Demand_Frequency`

### Execution Steps

1. **Ensure you have the processed dataset** from the Pre-processing step.
2. **Run the clustering script**:
    ```sh
    python clustering.py --input <path_to_processed_dataset> --output <path_to_demand_points_output>
    ```

## 3. OD Time Matrix

### Description

This module calculates the shortest travel time from each ambulance location to each demand point.

### Input Fields

- Ambulance locations with fields: `Ambulance_ID`, `Ambulance_Lat`, `Ambulance_Lon`
- Demand points with fields: `Demand_Point_ID`, `Demand_Point_Lat`, `Demand_Point_Lon`

### Output

- `.csv` file with `Ambulance_ID`, `Demand_Point_ID`, `Travel_Time`

### Execution Steps

1. **Prepare your dataset** with ambulance locations and demand points.
2. **Run the OD Time Matrix script**:
    ```sh
    python od_time_matrix.py --ambulance <path_to_ambulance_locations> --demand <path_to_demand_points> --output <path_to_od_matrix_output>
    ```

## 4. PuLP Optimization Module

### Description

This module uses mathematical optimization to maximize the coverage of each demand point with the given primary (r1) and secondary (r2) response times and alpha reliability.

### Input Fields

- OD Time Matrix from the previous step with fields: `Ambulance_ID`, `Demand_Point_ID`, `Travel_Time`
- Additional parameters: `Primary response time (r1)`, `Secondary response time (r2)`, `Alpha reliability`

### Output

- Optimized ambulance deployment plan including fields: `Ambulance_ID`, `Assigned_Demand_Point_ID`

### Execution Steps

1. **Ensure you have the OD Time Matrix** from the previous step.
2. **Define your primary and secondary response times, and alpha reliability.**
3. **Run the optimization script**:
    ```sh
    python pulp_optimization.py --input <path_to_od_matrix> --r1 <primary_response_time> --r2 <secondary_response_time> --alpha <alpha_reliability> --output <path_to_optimization_output>
    ```

## Additional Resources

- **Comparison of each optimization iteration with old ambulance deployment**: 
  - [optimization-comparison.ipynb](src/optimization/optimization-comparison.ipynb)
- **Loop iteration for multiple parameters and visualization**:
  - [optimisation_pipeline.ipynb](src/optimization/optimisation_pipeline.ipynb)

## Installation

Clone the repository and install the required dependencies:

```sh
git clone https://github.com/AGAMPANDEYY/Ambulance-Optimisation-EfkonStrabag.git
cd Ambulance-Optimisation-EfkonStrabag
source venv-efkon/bin/activate

