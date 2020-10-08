# Rainfall-Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network

Accompanying code for the paper **"Rainfall-Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network"**

    Gauch, M., Kratzert, F., Klotz, D., Nearing, G., Lin, J., and Hochreiter, S.:
    Rainfall--Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network, 2020.

The code in this repository, together with the `neuralhydrology` Python package, was used to produce all results and figures in our paper.
If you want to play around and train models yourself, we recommend taking a look at the [neuralhydrology documentation](https://neuralhydrology.readthedocs.io/), which also contains some tutorials on how to get started.

## Contents of this repository
- `results_analysis.ipynb` --- Jupyter notebook to reproduce tables and figures from the paper
- `environment.yml` --- Conda environment used to train and evaluate the models
- `configs/` -- configuration files to train the models from scratch
- `results/` --- Folder to place ensembled model predictions used in `results-analysis.ipynb` (available on [Zenodo](https://doi.org/10.5281/zenodo.4071886), see below)
- `data/datadir/` --- Folder to place datasets (see below on where to obtain the data)

## Required setup
1. First, create the conda environment: `conda env create -f environment.yml`
2. Second, download the following data:
   1. the CAMELS US dataset (CAMELS time series meteorology, observed flow, meta data, version 1.2) from [NCAR](https://ral.ucar.edu/solutions/products/camels) into `data/datadir/CAMELS_US`.
   2. for experiments with Maurer forcings, we use an extended forcings set available on [HydroShare](https://www.hydroshare.org/resource/17c896843cf940339c3c3496d0c1c077/). Place this dataset in the folder `data/datadir/CAMELS_US/basin_mean_forcing/maurer_extended`.
   3. the hourly NLDAS forcings and USGS streamflow data from [Zenodo (data)](https://doi.org/10.5281/zenodo.4072701). We recommend using the combined NetCDF file, but you can also use the csv files (but it will take much longer to load the data). Place this NetCDF file in the folder `data/datadir/CAMELS_US/hourly`.
3. If you don't want to train models yourself but use pre-trained models or simply run the Jupyter notebooks that analyze the results, you find the trained models and their predictions on [Zenodo (models)](https://doi.org/10.5281/zenodo.4071886).

## Training and evaluating a model
The folder `configs/` contains one configuration .yml file for each model. To train models from scratch, use these configuration files. If you wish to use pretrained models, download them from Zenodo (see above). Then follow these steps:
1. Activate the conda environment: `conda activate pytorch`
2. Start training with `nh-run train --config-file config.yml` (unless you use pretrained models)
3. Start the inference and evaluation with `nh-run evaluate --run-dir <run directory> --period test` (to evaluate on the validation period, use `--period validation`)

## Merging multiple runs into one ensemble
To average the predictions of a number of runs (located in `$DIR1`, `$DIR2`, ...) into one ensemble prediction, calculate the ensemble's metrics (NSE, MSE, ...), and store the results in `$RESULTS_FILE`, run:
`nh-results-ensemble --run-dirs $DIR1 $DIR2 ... --period test --save-file $RESULTS_FILE --metrics NSE MSE ...`.

## Contact
Martin Gauch: `gauch (at) ml.jku.at`

## Citation
```
@article{Gauch2020mtslstm,
    author = {Gauch, Martin and Kratzert, Frederik and Klotz, Daniel and Nearing, Grey and Lin, Jimmy and Hochreiter, Sepp},
    title = {Rainfall--Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network},
    journal = {n/a},
    volume = {n/a},
    number = {n/a},
    pages = {},
    doi = {n/a}
}
```

