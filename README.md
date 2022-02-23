# WHAT-THE-CELL...-AND-WHERE-
Manually counting cells in microscopy samples, e.g., to test the efficacy of cancer drugs to selectively inhibit the growth of cancer cells, is a tedious and time-consuming effort. Here, we automated this process using deep learning algorithms to identify the location and size of cellular mass.


## Environment
Make sure you have the latest version of macOS (currently Monterey) installed.
Also make sure that xcode is installed and updated: 

```BASH
xcode-select --install
```

Then we can go on to install hdf5:

```BASH
 brew install hdf5
```
With the system setup like that, we can go and create our environment and install tensorflow

```BASH
pyenv local 3.9.4
python -m venv .venv
source .venv/bin/activate
export HDF5_DIR=/opt/homebrew/Cellar/hdf5/1.12.1

pip install -U pip
pip install --no-binary=h5py h5py
pip install tensorflow-macos
pip install tensorflow-metal
pip install -r requirements.txt
pip install git+https://github.com/tensorflow/examples.git
```

### Execute the following code in the shell in the main directory of this notebook to create the necessary data subfolders

```BASH
mkdir -p data/data_original

mkdir -p data/data_preprocessed/mask_groundtruth
mkdir -p data/data_preprocessed/sliced_images/images
mkdir -p data/data_preprocessed/sliced_images/masks
mkdir -p data/data_preprocessed/sliced_images/predictions/masks
mkdir -p data/data_preprocessed/mask_predicted/masks
mkdir -p data/data_preprocessed/mask_predicted/intersections
mkdir -p data/data_preprocessed/mask_predicted/unions

mkdir -p data/data_preprocessed/mask_cluster/before_preprocessing
mkdir -p data/data_preprocessed/mask_cluster/masks_cg
mkdir -p data/data_preprocessed/mask_cluster/masks_cs
mkdir -p data/data_preprocessed/sliced_images/masks_cg
mkdir -p data/data_preprocessed/sliced_images/masks_cs
```
