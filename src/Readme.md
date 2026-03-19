# LSTM Modeling for Suspended Sediment Flux (SSF) – Sample Run

This repository contains a sample source code for the paper titled 'Towards Global Estimation of Riverine Suspended Sediment Flux Using Deep Learning' using a sample input file for a single COMID. It is designed to run for one basin, one random seed, and a specific set of hyperparameters.

## Requirements

Python 3.x environment with the following libraries:

Required Libraries

- Core Python Libraries: os, copy, random, datetime, collections, concurrent.futures  
- Data Processing: numpy, pandas  
- PyTorch: torch, torch.nn, torch.utils.data  
- Progress Display: tqdm

Install with pip:

pip install numpy pandas torch tqdm

Or with conda:

conda install numpy pandas pytorch tqdm -c pytorch

## Files Needed (available in the `data` folder)

`input_data_sample_COMID_74068275.0.csv`  
Input CSV file with the following structure:  
- Column 1: COMID  
- Column 2: Date  
- Column 3: SSF values  
- Columns 4 onward: Predictor variables  

`Description_of_column_names.csv` *(optional, also available in the `data` folder)*  
Contains names and descriptions of columns in the input CSV file.

## What to Update

To reproduce the sample run using LSTM_code.ipynb, you need to update the input path and output base path in the notebook.

### Input Path  
Locate and update the input_file_path variable in the notebook:

input_file_path = "/N/lustre/project/proj-212/praval/Global_sediment_flux/lstm_model/codes/cluster/Hydrogeosciences_codes/input_data_sample_COMID_74068275.0.csv"

Replace it with the full path to the location where you store the sample CSV file on your machine. 

### Output Base Path  
Locate and update the base_path variable in the notebook:

base_path = "/N/lustre/project/proj-212/praval/Global_sediment_flux/lstm_model/codes/cluster/Hydrogeosciences_codes/"

Replace this with a writable directory on your local machine.

### Output Folder  
The notebook will automatically create an output folder named based on the model configuration, such as:

tr_0.7_bs_32_id_85_hd_256_nl_1_lr_0.0001_sl_30_ep_100_se_1_last_epoch

This folder will be saved under the directory specified in base_path.
