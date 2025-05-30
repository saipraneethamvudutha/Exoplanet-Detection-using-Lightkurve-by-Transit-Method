# Exoplanet Detection Using Lightkurve by Transit Method

This project utilizes NASA's Lightkurve Python package to detect exoplanets through the transit method. By analyzing light curves from the Kepler and TESS missions, the pipeline automates data acquisition, preprocessing, transit detection using the Box Least Squares (BLS) algorithm, and visualization of potential exoplanet transits.

## Table of Contents

- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Data Acquisition](#data-acquisition)
- [Data Preprocessing](#data-preprocessing)
- [Methodology](#methodology)
  - [Lightkurve Basics](#lightkurve-basics)
  - [Transit Method](#transit-method)
  - [Detection Pipeline](#detection-pipeline)
- [Usage Example](#usage-example)
- [Configuration](#configuration)
- [Results and Evaluation](#results-and-evaluation)
- [Sample Plot Outputs](#sample-plot-outputs)
- [Contributing](#contributing)
- [Troubleshooting](#troubleshooting)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Project Overview

The transit method detects exoplanets by observing periodic dips in a star's brightness, indicating a planet passing in front of it. This project automates the detection process using Lightkurve, streamlining the workflow from data retrieval to transit identification and visualization.

## Key Features

- Automated retrieval of light curve data from Kepler and TESS missions
- Preprocessing of light curves to remove noise and artifacts
- Application of the Box Least Squares (BLS) algorithm for transit detection
- Visualization of light curves and identified transit events
- Modular and extensible codebase for customization

## Prerequisites

- Python 3.7 or higher
- Lightkurve
- NumPy
- Matplotlib
- Astropy

## Installation

1. Clone the repository:

```bash
git clone https://github.com/saipraneethamvudutha/Exoplanet-Detection-using-Lightkurve-by-Transit-Method.git
cd Exoplanet-Detection-using-Lightkurve-by-Transit-Method
```

2. Install the required packages:

```bash
pip install lightkurve numpy matplotlib astropy
```

## Data Acquisition

The pipeline uses Lightkurve to download light curve data for specified target stars. Users can specify the target by its TIC or KIC ID.

## Data Preprocessing

Preprocessing steps include:

- Removing NaN values
- Flattening the light curve to remove long-term trends
- Normalizing the flux values

## Methodology

### Lightkurve Basics

Lightkurve is a Python package for analyzing astronomical light curve data, particularly from NASA's Kepler and TESS missions. It provides tools for downloading, visualizing, and processing light curves.

### Transit Method

The transit method involves detecting periodic dips in a star's brightness, which occur when a planet transits in front of the star from our line of sight. These dips can reveal information about the planet's size and orbit.

### Detection Pipeline

1. Download the light curve data for the target star
2. Preprocess the data to remove noise and trends
3. Apply the Box Least Squares (BLS) algorithm to identify periodic transit signals
4. Visualize the phase-folded light curve to confirm transit events

## Usage Example

```python
import lightkurve as lk
import numpy as np

# Search and download light curve
search_result = lk.search_lightcurve('Kepler-10', mission='Kepler')
lc = search_result.download_all().stitch()

# Preprocess the light curve
lc = lc.remove_nans().flatten().normalize()

# Apply BLS algorithm
periodogram = lc.to_periodogram(method='bls', period=np.linspace(0.5, 10, 10000))
best_period = periodogram.period_at_max_power

# Fold the light curve
folded_lc = lc.fold(period=best_period)

# Plot the folded light curve
folded_lc.plot()
```

## Configuration

Users can adjust parameters such as:

- Target star ID
- Period range for BLS algorithm
- Preprocessing window length

These configurations can be set directly in the script or through a configuration file.

## Results and Evaluation

The pipeline outputs:

- Periodogram plots showing the power spectrum
- Phase-folded light curves highlighting potential transits
- Estimated parameters such as period and transit depth

Evaluation of results involves assessing the signal-to-noise ratio and the consistency of detected transits.

## Sample Plot Outputs

**Figure 1:** BLS periodogram indicating the most likely transit period.

**Figure 2:** Phase-folded light curve showing the transit event.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request. For major changes, open an issue first to discuss your proposed modifications.

## Troubleshooting

- Ensure all dependencies are installed correctly
- Check the target star ID for accuracy
- Verify internet connectivity for data downloads

For further assistance, please open an issue in the repository.

## License

This project is licensed under the MIT License.

## Acknowledgements

- Lightkurve development team
- NASA's Kepler and TESS missions for providing the data
- Contributors and the open-source community
