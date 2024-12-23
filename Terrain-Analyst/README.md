# Terrain Analysis Script - README

## Overview

This script processes property data and analyzes terrain characteristics using latitude, longitude, and acreage information. It calculates various slope and elevation statistics to assign a terrain score and classify the terrain type for each property.

### Key Features:
- Retrieves elevation data from the USGS Elevation Point Query Service.
- Calculates slopes between different points around the property.
- Analyzes terrain variability based on slopes and elevations.
- Outputs a terrain score and type for each property.
- Supports concurrent processing for faster execution.

---

## Requirements

- Python 3.x
- The following Python packages:
  - `pandas`
  - `requests`
  - `numpy`
  - `math`
  - `logging`
  - `concurrent.futures`
  - `ratelimit`
  - `scipy`
  - `argparse`
  - `tqdm`
  
You can install the necessary dependencies using:
```bash
pip install pandas requests numpy math logging ratelimit scipy tqdm
```

---

## Input & Output

### Input File:
- The input file should be an Excel spreadsheet (`.xlsx`) with at least the following columns:
  - **LATITUDE**: Latitude of the property
  - **LONGITUDE**: Longitude of the property
  - **Lot Acres**: Acreage of the property
  
### Output File:
- The script generates an output Excel file (`.xlsx`) with the following added columns:
  - **Terrain Score**: The calculated terrain score based on slope and elevation metrics.
  - **Terrain Type**: A classification of the terrain type (e.g., Very Flat, Hilly, etc.).

---

## How to Use

### 1. Running the Script (Command Line)
From the command line, you can run the script with the following command:

```bash
python terrain_analysis.py --output_file <output_filepath> --sample_size <number_of_samples>
```

- `output_file`: The file path where the output Excel file will be saved (default: `properties_with_terrain_scores.xlsx`).
- `sample_size`: Number of properties to process from the input file (default: 100).

Example:
```bash
python terrain_analysis.py --output_file terrain_results.xlsx --sample_size 50
```

### 2. Running in Jupyter or IPython
If you're running the script in an IPython environment like Jupyter, default settings will be used:
- Input file: `sampled_properties.xlsx`
- Output file: `properties_with_terrain_scores.xlsx`
- Sample size: 50 properties

---

## Detailed Explanation

### Functions

1. **get_elevation(session, lat, lon)**
   - Fetches the elevation of a location from the USGS Elevation Point Query Service.

2. **get_offset_from_acreage(acreage, percentage)**
   - Calculates the distance offset based on the property size (acreage) and a percentage movement.

3. **calculate_slope(elevation1, elevation2, lat1, lon1, lat2, lon2)**
   - Computes the slope between two geographic points using their elevations and coordinates.

4. **analyze_terrain(session, lat, lon, acreage)**
   - Analyzes terrain around a property, calculates slopes and elevation stats, and assigns a terrain type.

5. **process_properties(input_file, output_file, sample_size)**
   - Processes the properties from an Excel sheet, analyzes the terrain for each, and writes the results to a new Excel file.

### Terrain Classification:
The script classifies the terrain based on the final score calculated from elevation and slope stats. Examples of terrain types:
- Very Flat
- Flat
- Hilly
- Mountainous

---

## Logging & Progress

- **Logging**: Logs are provided at various stages of execution to track progress, errors, and warnings.
- **Progress Bar**: The script uses `tqdm` to display the progress of processing each property.

---

## Notes

- **Rate Limiting**: The script respects the rate limit of the USGS Elevation API, limiting to 5 calls per second.
- **Concurrency**: The script uses multithreading to speed up elevation data retrieval and terrain analysis.
