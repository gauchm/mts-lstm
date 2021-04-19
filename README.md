# Rainfall&ndash;Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network

Accompanying code for the paper [**"Rainfall&ndash;Runoff Prediction at Multiple Timescales with a Single Long Short-Term Memory Network"**](https://doi.org/10.5194/hess-25-2045-2021)

    Gauch, M., Kratzert, F., Klotz, D., Nearing, G., Lin, J., and Hochreiter, S.: Rainfall–runoff prediction at multiple timescales
    with a single Long Short-Term Memory network, Hydrol. Earth Syst. Sci., 25, 2045–2062, https://doi.org/10.5194/hess-25-2045-2021, 2021. 

The code in this repository, together with the `neuralhydrology` Python package, was used to produce all results and figures in our paper.
If you want to play around and train models yourself, we recommend taking a look at the [neuralhydrology documentation](https://neuralhydrology.readthedocs.io/), which also contains some tutorials on how to get started.

## Contents of this repository
- `results_analysis.ipynb` -- Jupyter notebook to reproduce tables and figures from the paper
- `odelstm-analysis.ipynb` -- Jupyter notebook to reproduce our results on time-continuous prediction
- `environment.yml` -- Conda environment used to train and evaluate the models
- `configs/` -- configuration files to train the models from scratch
- `results/` -- Folder to place ensembled model predictions used in `results-analysis.ipynb` (available on [Zenodo](https://doi.org/10.5281/zenodo.4071886), see below)
- `data/datadir/` -- Folder to place datasets (see below on where to obtain the data)

## Required setup
1. Create the conda environment: `conda env create -f environment.yml`
2. Activate the environment: `conda activate pytorch`
3. Install the neuralhydrology package: `pip install https://github.com/neuralhydrology/neuralhydrology/archive/9626578.zip` (for the most up-to-date version, use the installation instructions on the [neuralhydrology documentation](https://neuralhydrology.readthedocs.io/en/latest/)).
4. Download the following data:
   1. the CAMELS US dataset (CAMELS time series meteorology, observed flow, meta data, version 1.2) from [NCAR](https://ral.ucar.edu/solutions/products/camels) into `data/datadir/CAMELS_US`.
   2. for experiments with Maurer forcings, we use an extended forcings set available on [HydroShare](https://www.hydroshare.org/resource/17c896843cf940339c3c3496d0c1c077/). Place this dataset in the folder `data/datadir/CAMELS_US/basin_mean_forcing/maurer_extended`.
   3. the hourly NLDAS forcings and USGS streamflow data from [Zenodo (data)](https://doi.org/10.5281/zenodo.4072700). We recommend using the combined NetCDF file, but you can also use the csv files (but it will take much longer to load the data). Place this NetCDF file in the folder `data/datadir/CAMELS_US/hourly`.
5. If you don't want to train models yourself but use pre-trained models or simply run the Jupyter notebooks that analyze the results, you find the trained models and their predictions on [Zenodo (models)](https://doi.org/10.5281/zenodo.4071885).

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
```bib
@article{Gauch2021mtslstm,
    author = {Gauch, M. and Kratzert, F. and Klotz, D. and Nearing, G. and Lin, J. and Hochreiter, S.},
    title = {Rainfall--runoff prediction at multiple timescales with a single Long Short-Term Memory network},
    journal = {Hydrology and Earth System Sciences},
    volume = {25},
    year = {2021},
    number = {4},
    pages = {2045--2062},
    url = {https://hess.copernicus.org/articles/25/2045/2021/},
    doi = {10.5194/hess-25-2045-2021}
}
```

