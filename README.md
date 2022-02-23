# WHAT-THE-CELL...-AND-WHERE-
Manually counting cells in microscopy samples, e.g., to test the efficacy of cancer drugs to selectively inhibit the growth of cancer cells, is a tedious and time-consuming effort. Here, we automated this process using deep learning algorithms to identify the location and size of cellular mass.

**Please go through the whole set up in the exact order as follows**


## 1. Set up of the Environment
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

mkdir -p data/data_preprocessed/mask_cluster/before_preprocessing/segmented_img
mkdir -p data/data_preprocessed/mask_cluster/before_preprocessing/segmented_img_sift
mkdir -p data/data_preprocessed/mask_cluster/masks_cg
mkdir -p data/data_preprocessed/mask_cluster/masks_cs
mkdir -p data/data_preprocessed/sliced_images/masks_cg
mkdir -p data/data_preprocessed/sliced_images/masks_cs
```
### 2. Running order for notebooks

In order to run all scripts flawlessly, it is mandatory to run the notebooks in the right order. Please use the notebook and directory labels as reference to find out what to do and in which order.

Here, we summarize the content of all paths.

**RUN_ONCE_PREPROCESSING:** The notebooks have to be executed at first. They do the following:
* 01: Creates segmentation masks with Kmeans clustering (alternative masks to train our U-Net models)
* 02: Creates segmentation masks by the help of SIFT keypoints (alternative masks to train our U-Net models)
* 03: Creates masks from running length annotations delivered by Sartorius Kaggle data set. Slices images and masks from 01 to 03 into four quadrants.

**U-NET**
* 01: define U-Net architures
* 02: Defining data pipeline functions for U-Net: choosing masks, creating train-test-splits, data augmentation
* 03: Model training with different U-Net model options
* 04: Evaluation of models including IoU analysis

**EDA**



* All notebooks are tested on MacBook Air M1 chip
* load data from kaggle
* copy only train directory and train.csv in data_original
