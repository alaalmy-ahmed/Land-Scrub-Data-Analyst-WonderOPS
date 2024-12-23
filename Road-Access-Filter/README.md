# Road Access Classification Script

## Overview

This script is designed to classify properties based on their road access using a weighted scoring system. It reads property data from an Excel file, calculates a road access score for each property based on its geographic location and acreage, and adds two new columns to the dataset:
- **`Road-Access`**: Indicates whether the property has road access (`Yes` or `No`).
- **`Percentage`**: Represents the percentage score reflecting the likelihood of road access.

The results are saved in a new Excel file.

## Prerequisites

Before running the script, ensure that you have the following Python libraries installed:

- `pandas`
- `numpy`
- `requests`
- `tqdm`
- `logging`

You can install these dependencies using pip:

```bash
pip install pandas numpy requests tqdm logging
```

## How It Works

1. **Calculate Radius**: The script calculates the radius of a circle (in meters) corresponding to the area of the property, considering a margin for road access checks.

2. **Check Road Access**: The script uses the Overpass API to determine if there is a highway (road) within the calculated radius around the property’s latitude and longitude. It retries the request up to three times in case of failure.

3. **Weighted Road Access Check**: The script evaluates road access using different margins, with each margin having an associated weight. A percentage score is calculated based on the accumulated score from all weighted margins. A threshold determines the final classification (`Yes` or `No`).

4. **Filter Properties**: The script processes each property in the dataset, applies the weighted road access check, and records the results in new columns: `Road-Access` and `Percentage`.

5. **Save Results**: The script saves the filtered dataset, with the new columns added, into a new Excel file.

## How to Use

1. **Prepare Your Data**: Ensure your data is in an Excel file with columns for `LATITUDE`, `LONGITUDE`, and `ACREAGE`.

2. **Run the Script**:
   - Update the `excel_file_path` variable in the `main()` function to point to your input Excel file.
   - Execute the script. The script will process the data, add the `Road-Access` and `Percentage` columns, and save the results to a new Excel file.

```bash
python road_access_classification.py
```

3. **Review Results**: The output file will be saved in the same directory with `_WITH_ROAD_ACCESS` appended to the original filename.

## Example

If your input file is named `Export-Baldwin-MI-target-rocket.xlsx`, the output file will be named `Export-Baldwin-MI-target-rocket_WITH_ROAD_ACCESS.xlsx`.

## Logging

The script uses Python's `logging` module to log information about its execution. Logs include information about the road access checks and any issues encountered during API requests.

## Customization

- **Margins and Weights**: You can customize the `margins` and `weights` arrays in the `filter_properties_with_weighted_road_access` function to adjust how the script evaluates road access.

- **Threshold**: The script currently uses a threshold of one-third of the total weight sum to determine the road access classification (`Yes` or `No`). You can adjust this threshold based on your specific requirements.

## Troubleshooting

- **API Timeouts**: If the script frequently encounters timeouts when making requests to the Overpass API, consider increasing the timeout duration or the number of retries.

- **Data Issues**: Ensure that the `LATITUDE`, `LONGITUDE`, and `ACREAGE` columns are correctly populated in your input file. The script will drop rows with missing or non-numeric values in the `ACREAGE` column.