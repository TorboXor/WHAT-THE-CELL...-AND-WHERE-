# WHAT-THE-CELL...-AND-WHERE-
Manually counting cells in microscopy samples, e.g., to test the efficacy of cancer drugs to selectively inhibit the growth of cancer cells, is a tedious and time-consuming effort. Here, we automated this process using deep learning algorithms to identify the location and size of cellular mass.

More specifically, we use U-Net models for semantic segmentation of nervous tissue cell bodies in light microscope images. The data set we use to train and test our models is from a kaggle challenge and can be found here:

https://www.kaggle.com/c/sartorius-cell-instance-segmentation

The dataset contains pure (i.e. no mixtures of cell types) cell culture images of three cell types which can all be found in human brains: 

* shsy5y: This is the neuroblastoma cell line
* cort: neurons which should be unaffected by cancer drugs
* astrocytes: glial cells that help neurons to electrically shield themselves from other neurons and therefore, should also not be targeted by drugs.

Just to avoid confusion: Although the data set provided by the kaggle challenge (here, the evaluation metric is the mean average IoU since instance segmentation is the task), we decided to focus on semantic segmentation since analyzing the cell body area is expedient for testing drug efficacy. Hence, we use IoU (intersection of union) for just two pixel classes: cell type and none-cell type.

While optimizing our models, we noticed that the astrocyte segmentation masks provided by the dataset in form of running length annotations have a remarkable deviation from the actual cell bodies. To deal with this issue we implemented notebooks that create segmentation masks that can be used alternatively to train and test the model.

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
* 01: Define U-Net architures
* 02: Defining data pipeline functions for U-Nets: choosing masks, creating train-test-splits, data augmentation
* 03: Model training with different U-Net model options: from Scratch and semi-pretrained using VGG16 and MobileNetV2 for the down convolution path.
* 04: Evaluation of models including IoU analysis

**EDA**



* All notebooks are tested on MacBook Air M1 chip
* load data from kaggle
* copy only train directory and train.csv in data_original
