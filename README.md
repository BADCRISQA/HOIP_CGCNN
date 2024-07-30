# Hybrid Organic-Inorganic Perovskites - Crystal Graph Convolutional Neural Networks

This model implements the Crystal Graph Neural Networks (CGCNN) to predict band gap values for Hybrid Organic-Inorganic Perovskites (HOIP). 

## Table of Contents

- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Define a customized dataset](#define-a-customized-dataset)
  - [Predict material properties with a pre-trained CGCNN model](#predict-material-properties-with-a-pre-trained-cgcnn-model)
- [Data](#data)
- [Authors](#authors)
- [License](#license)

##  Prerequisites

This package requires:

- [PyTorch](http://pytorch.org)
- [scikit-learn](http://scikit-learn.org/stable/)
- [pymatgen](http://pymatgen.org)

We recomend installing the prerequisites is using [conda](https://conda.io/docs/index.html). 

Run the following command to create an environment named `cgcnn` and install all prerequisites:

```bash
conda create -n cgcnn python=3 scikit-learn pytorch torchvision pymatgen -c pytorch -c conda-forge
conda activate cgcnn
```

To exit the environment use:

```bash
conda deactivate
```
To see the list of commands

```bash
conda help
```

## Usage

### Define a dataset 

Create a customized dataset by creating a directory `root_dir` with the following files or use the one we have provided: 

1. `id_prop.csv` has two columns. The first column contains the file name without the file extension (.cif) of each crystal, and the second column should contain the target values. This file is required even when predicting although the values in the second column are required but ignored.

2. `atom_init.json`: The one that is provided is applicable to almost all situations.

3. A list of cif files

The structure of the `root_dir` should be:

```
root_dir
├── id_prop.csv
├── atom_init.json
├── cif0.cif
├── cif1.cif
├── ...
```

An example `root_dir` is located in this github repository.

### Train a CGCNN model

Ensure a dataset is accessible under the folder name `root_dir`

```bash
python main.py root_dir
```

Hyperparameters are also taken as arguments:

```bash
python main.py root_dir --epochs 100
```

After training, you will get three files in `cgcnn` directory.

- `model_best.pth.tar`: stores the CGCNN model with the best validation accuracy.
- `checkpoint.pth.tar`: stores the CGCNN model at the last epoch.
- `test_results.csv`: stores the file name (without extension), target value, and predicted value.

### Predict material properties with a pre-trained CGCNN model

To predict material properties:

Ensure a dataset is accessible under the folder name `root_dir`

```bash
python predict.py best_model.pth.tar root_dir
```

`test_results.csv`: stores the `ID`, target value, and predicted value for each crystal in test set. As mentioned before, the target value column is ignored for predictions.

## Authors

This software is adapted from the following paper:
Xie, T., & Grossman, J. C. (2018). Crystal Graph Convolutional Neural Networks for an Accurate and Interpretable Prediction of Material Properties. Physical Review Letters, 120(14). https://doi.org/10.1103/physrevlett.120.145301

## License

This version of CGCNN is released under the MIT License.
