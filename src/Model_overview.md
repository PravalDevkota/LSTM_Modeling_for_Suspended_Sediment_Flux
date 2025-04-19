# Overview of LSTM Modeling Code for Suspended Sediment Flux (SSF) Prediction

Here we present a summary of code capable of running for a single basin, for a single seed, and a specific set of hyperparameters. This code has been applied to a sample input file named `input_data_sample_COMID_74068275.0.csv`, where the first column is the COMID, the second column is the corresponding date of the observed values, the third column is the SSF values, and the remaining columns are predictors. The description of the predictors is given in another file named `Description_of_column_names.csv`. The output for training and testing is saved in the `tr_0.7_bs_32_id_85_hd_256_nl_1_lr_0.0001_sl_30_ep_100_se_1_last_epoch` folder. This code can be extended for multiple files to replicate our model results.

This document outlines the major steps and components involved in training, testing and saving LSTM (Long Short-Term Memory) model results. The code is structured in a modular and sequential manner to handle preprocessing, training, testing and result saving. It includes custom dataset classes, data normalization, and a standard PyTorch-based LSTM architecture.

## Steps:

### 1. Import Required Libraries
The script begins by importing core Python libraries (`os`, `random`, `datetime`), data processing libraries (`numpy`, `pandas`), and PyTorch libraries (`torch`, `nn`, `DataLoader`, etc.).

Additional tools like `tqdm` (for progress tracking) and `ProcessPoolExecutor` (for parallelism) are also included.

### 2. Load and Preprocess Data
A CSV file containing sediment flux data for a single COMID is loaded.

The file is parsed into:

- `data`: Numerical values starting from the third column.

Only COMIDs with at least 50 valid observations are retained.

### 3. Train-Test Split (by Date)
Each valid date (i.e., with a non-NaN SSF value) is split into training and testing sets based on a 70/30 rule. Following chronological order, the first 70% is used for training and the remaining 30% for testing.

A filter ensures that only dates with enough preceding time steps (≥ sequence length) are retained for modeling.

### 4. Normalize and Prepare Training Data
All input data are stacked to compute global mean and standard deviation across all features.

Extremely small standard deviations are clipped to a minimum threshold (0.001).

These statistics are used to normalize input sequences.

The maximum SSF for each COMID is also stored for target normalization.

### 5. Custom Dataset Class: `CustomData`
This PyTorch `Dataset` class prepares input sequences of a specified length along with their corresponding target values.

- Inputs are normalized using precomputed means and standard deviations.
- Targets are normalized using the COMID-specific maximum SSF value.
- Each item returns the COMID, the normalized input sequence, the normalized target, and the corresponding date.

### 6. Model Definition: `LSTMModel`
A simple PyTorch LSTM model is defined with:

- Input layer based on feature count
- A single LSTM layer
- Dropout
- A fully connected layer to output the prediction

### 7. Training Loop
The LSTM model is trained for a predefined number of epochs (epochs = 100) using `L1Loss` (Mean Absolute Error Loss).

During each epoch:

- Batches are passed through the model
- Loss is computed and backpropagated
- Training metrics like average loss, NSE (Nash-Sutcliffe Efficiency), and relative root mean square error are computed
- Model states are saved after each epoch

### 8. Custom Sampler for Evaluation
A `SubsetSampler` class is defined to index the test dataset, mimicking the PyTorch `SubsetRandomSampler`.

### 9. Evaluation on Test Set
The model from the last epoch is loaded and evaluated on the test dataset.

- Predictions are rescaled using the COMID-specific maximum.
- Results are saved to a CSV file in the designated output directory with dynamic naming based on train split fraction (`tr`), batch size(`bs`), input dimension(`id`), hidden dimension(`hd`), number of layers(`nl`), learning rate(`lr`), sequence length(`sl`), epochs(`ep`) and seed(`se`).

### 10. Evaluation on Train Set
The same model is also evaluated on the training dataset.

- Results are similarly saved to a separate CSV file in the same output directory.
