# TimeGraph: Synthetic Benchmark Datasets for Robust Time-Series Causal Discovery

This repository provides **TimeGraph**, a synthetic benchmark suite for time-series causal discovery. It offers a robust framework for generating diverse datasets under realistic conditions, enabling rigorous evaluation and comparison of causal discovery algorithms.

## Table of Contents

* [Introduction](#introduction)
* [Features](#features)
* [Installation](#installation)
* [Dataset Generation](#dataset-generation)

  * [Overview of Generators](#overview-of-generators)
  * [Common Parameters](#common-parameters)
  * [Linear Time Series](#linear-time-series)
  * [Nonlinear Time Series](#nonlinear-time-series)
  * [Confounded Time Series](#confounded-time-series)
  * [Irregularly Sampled Time Series](#irregularly-sampled-time-series)
  * [Trends and Seasonality](#trends-and-seasonality)
  * [Missing Data (MCAR & Block)](#missing-data-mcar--block)
  * [Mixed Noise Types](#mixed-noise-types)
* [Output Structure](#output-structure)
* [Usage Examples](#usage-examples)
* [Pre-generated Datasets](#pre-generated-datasets)
* [Customization](#customization)
* [Contributing](#contributing)
* [Citation](#citation)
* [License](#license)

## Introduction

Time-series causal discovery is essential for understanding dynamic systems, but effective evaluation requires realistic benchmark datasets. **TimeGraph** addresses this gap by offering a suite of synthetic data generators that simulate key temporal complexities: autocorrelation, nonstationarity, confounding, irregular sampling, missing data, and mixed noise. This repository supports our KDD 2025 paper to promote transparent and reproducible research.

## Features

* **Diverse Causal Structures**: Supports linear and nonlinear causal relationships.
* **Confounders**: Incorporate latent variables influencing multiple observed variables.
* **Noise Distributions**: Gaussian, Student's t, and Laplace.
* **Sampling Patterns**: Regular and irregular.
* **Trends and Seasonality**: Deterministic components with adjustable strength.
* **Missing Data**:

  * MCAR (randomly missing)
  * Block missingness (sensor-like failures)
* **Mixed Noise**: Combine Gaussian and Laplace noise.
* **Comprehensive Outputs**: Time series (with/without missingness), causal graphs, plots, and true model descriptions.

## Installation

Install dependencies using pip:

```bash
pip install numpy pandas scipy tigramite matplotlib
```

## Dataset Generation

All scripts and notebooks are in the `Codes/` directory. Each file corresponds to a specific configuration.

### Overview of Generators

* `LinearTimeSeriesGenerator`
* `LinearTimeSeriesGeneratorMCAR`
* `LinearTimeSeriesGeneratorConfounded`
* `LinearTimeSeriesGeneratorMCARConfounded`
* `NonlinearConfoundedGenerator`
* `NonlinearTimeSeriesGeneratorIrregular`
* `BlockMissingNonlinearGenerator`
* `BlockMissingNonlinearConfoundedGenerator`
* `NonlinearTimeSeriesGeneratorMixedMissing`
* `NonlinearTimeSeriesGeneratorMixedMissingConfounded`

### Common Parameters

* `n_points`: Number of time steps
* `n_vars`: Number of variables
* `max_lag`: Causal lag
* `noise_type`: `'gaussian'` or `'student_t'`
* `noise_params`: e.g., `{'scale': 0.1, 'df': 3}`
* `random_state`: Reproducibility

### Linear Time Series

* `a1.ipynb` / `a1.py`: Basic linear model
* `a1c.ipynb` / `a1c.py`: Linear with confounder `U[t]`

### Nonlinear Time Series

* `b1.ipynb` / `b1.py`: Polynomial terms
* `b1c.ipynb` / `b1c.py`: Nonlinear with confounder

### Confounded Time Series

* Add `U[t]` terms to multiple variable equations

### Irregularly Sampled Time Series

* `a2.ipynb` / `a2.py`: Irregular sampling
* `a2c.ipynb` / `a2c.py`: Irregular sampling + confounder
* `c1.ipynb` / `c1.py`: Irregular, nonlinear with trend/seasonality
* `c1c.ipynb` / `c1c.py`: Irregular, nonlinear + confounder

### Trends and Seasonality

* `trend = strength * modifier * t`
* `seasonality = strength * (sin(...) + cos(...))`

### Missing Data (MCAR & Block)

* `d1.ipynb` / `d1.py`: Linear + MCAR
* `d1c.ipynb` / `d1c.py`: Linear + MCAR + confounder
* `d2.ipynb` / `d2.py`: Nonlinear + block
* `d2c.ipynb` / `d2c.py`: Nonlinear + block + confounder
* `d3.ipynb` / `d3.py`: Nonlinear + mixed missing
* `d3c.ipynb` / `d3c.py`: Nonlinear + mixed missing + confounder

### Mixed Noise Types

* `b2.py`, `b2c.py`: Gaussian + Laplace, controlled by `noise_mix_ratio`

## Output Structure

Outputs are saved to `output_dir`:

```bash
/output/linear_ts_n1000_vars4_lag2_gaussian.csv
/output/linear_causal_graph_n1000_vars4_lag2_gaussian.png
/output/linear_structural_eq.txt
```

## Usage Examples

Example script block (e.g., `a1.py`):

```python
if __name__ == "__main__":
    for n in [500, 1000, 3000]:
        for vars in [4, 6, 8]:
            for lag in [2, 3]:
                for noise in ['gaussian', 'student_t']:
                    gen = LinearTimeSeriesGenerator(noise_type=noise, noise_params={'scale':0.1}, random_state=42)
                    df = gen.generate_multivariate_ts(n_points=n, n_vars=vars, max_lag=lag)
                    save_dataset_and_graph(df, vars, lag, n, noise)
```

## Pre-generated Datasets

All types of generated datasets are provided in the `datasets/` directory. Each subfolder is named after its corresponding generator ID (e.g., `a1`, `a1c`, `b1`, `c1c`, etc.) and contains representative outputs such as time series files, causal graphs, and structural descriptions.

## Customization

Users can modify dataset configurations by adjusting script parameters or editing the underlying code. Custom sample sizes, noise parameters, and even new causal equations can be defined directly within the generator classes or the main execution blocks of the scripts.

## Contributing

Contributions are welcome! Please open issues or pull requests.

## Citation

If you use TimeGraph, please cite:

```bibtex
@inproceedings{Ferdous2025TimeGraph,
  author    = {Muhammad Hasan Ferdous and Emam Hossain and Md Osman Gani},
  title     = {{TimeGraph}: Synthetic Benchmark Datasets for Robust Time-Series Causal Discovery},
  booktitle = {Proceedings of the 31st ACM SIGKDD Conference on Knowledge Discovery and Data Mining V.2 (KDD '25)},
  series    = {KDD '25},
  year      = {2025},
  isbn      = {979-8-4007-1454-2/2025/08},
  publisher = {ACM},
  address   = {Toronto, ON, Canada},
  doi       = {10.1145/3711896.3737439},
  numpages  = {11},
  location  = {Toronto, ON, Canada},
  month     = {August #3--7},
}
```

## License

This project is licensed under the MIT License.
