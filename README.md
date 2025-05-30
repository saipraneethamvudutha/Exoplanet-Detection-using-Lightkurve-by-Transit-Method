Exoplanet Detection Using Lightkurve by Transit Method

Table of Contents

Project Overview

Key Features

Prerequisites

Installation

Data Acquisition

Data Preprocessing

Methodology

Lightkurve Basics

Transit Method

Detection Pipeline

Usage Guide

Running the Analysis

Visualizing Results

Results and Evaluation

Configuration and Parameters

Contributing

Troubleshooting

License

Acknowledgements

Project Overview

This repository implements a pipeline to detect exoplanets by analyzing light curves using the Lightkurve library and the transit method. By examining periodic dips in stellar brightness, we identify candidate exoplanet transits and characterize their properties.

Key Features

Download and process Kepler/K2/TESS light curve data via Lightkurve

Preprocessing: detrending, normalization, and outlier removal

Automated transit search using Box Least Squares (BLS)

Parameter estimation of transit events (period, depth, duration)

Visualization of light curves, phase-folded transits, and BLS periodograms

Jupyter Notebook workflow for interactive exploration

Prerequisites

Python 3.8 or higher

pip or conda

Internet connection for data download

Installation

Clone this repository:

git clone https://github.com/saipraneethamvudutha/Exoplanet-Detection-using-Lightkurve-by-Transit-Method.git
cd Exoplanet-Detection-using-Lightkurve-by-Transit-Method

(Optional) Create a virtual environment:

python3 -m venv venv
source venv/bin/activate  # on Windows: venv\Scripts\activate

Install dependencies:

pip install -r requirements.txt

Data Acquisition

Lightkurve simplifies downloading mission data directly in Python. By specifying a target identifier (e.g., KIC or TIC ID), the code fetches the corresponding light curve:

from lightkurve import search_lightcurvefile
lc_file = search_lightcurvefile('KIC 10264202', mission='Kepler').download()

Data Preprocessing

Quality Masking: Remove bad-quality cadences.

Normalization: Divide by median to flatten baseline.

Detrending: Apply a Savitzky–Golay filter or polynomial fit to remove long-term trends.

Outlier Removal: Sigma clipping to discard flares and cosmic rays.

Example snippet:

lc = lc_file.PDCSAP_FLUX
lc = lc.remove_outliers(sigma=5)
lc_flat = lc.flatten(window_length=401)

Methodology

Lightkurve Basics

Lightkurve is a community-driven package for Kepler, K2, and TESS time series analysis. It provides functions for data access, cleaning, periodograms, and plotting.

Transit Method

Exoplanet transits cause periodic dips in stellar brightness. We use the Box Least Squares (BLS) algorithm to search for such box-shaped signals in the light curve.

Detection Pipeline

Generate BLS Periodogram: Scan a grid of trial periods and durations.

Identify Peaks: Locate the highest-power peaks corresponding to potential transits.

Estimate Parameters: Extract period, epoch (transit midpoint), depth, and duration.

Validation: Visual inspection of phase-folded light curve.

from astropy.timeseries import BoxLeastSquares
model = BoxLeastSquares(lc_flat.time, lc_flat.flux)
results = model.autopower(0.2)  # max duration 20% of period
best = results.period[np.argmax(results.power)]

Usage Guide

Running the Analysis

Open Exoplanet_Detection.ipynb in Jupyter Notebook or JupyterLab and run cells sequentially, or execute the main script:

python detect_transits.py --target 'KIC 10264202' --mission 'Kepler'

Visualizing Results

Light Curve Plot: Raw and detrended light curves

BLS Periodogram: Power vs. period

Phase-Folded Transit: Stacked view of transits

Plots are automatically saved to the outputs/ directory.

Results and Evaluation

Example results for KIC 10264202:

Detected Period: 10.304 days

Transit Depth: 500 ppm

Duration: 3.2 hours

Further validation with independent tools (e.g., Transit Least Squares) is encouraged.

Configuration and Parameters

Adjustable parameters can be modified in config.yaml:

data:
  mission: Kepler
  target: 'KIC 10264202'
preprocessing:
  flatten_window: 401
  outlier_sigma: 5
bls:
  duration_grid: [0.01, 0.3]
  oversample: 10

Contributing

Contributions are welcome! Please follow these steps:

Fork the repository

Create a feature branch: git checkout -b feature/YourFeature

Commit your changes: git commit -m 'Add YourFeature'

Push to branch: git push origin feature/YourFeature

Open a Pull Request

Please adhere to the Code of Conduct.

Troubleshooting

Import Errors: Ensure all packages are installed (pip install -r requirements.txt).

Data Download Issues: Check internet connection; verify mission name.

BLS Failures: Adjust the duration grid or oversampling factor in config.yaml.

License

This project is licensed under the MIT License. See LICENSE for details.

Acknowledgements

Lightkurve team for providing the core library

NASA Kepler and TESS missions for the publicly available data

Open-source community for contributions
