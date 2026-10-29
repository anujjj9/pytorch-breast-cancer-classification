# Breast Cancer Classification with PyTorch

This is a beginner-friendly **learning project** that uses PyTorch to demonstrate binary classification on the Breast Cancer Wisconsin (Diagnostic) dataset. It covers basic preprocessing, a custom PyTorch dataset, a small neural network, model training, and test-set evaluation.

> **Educational use only. This project is not intended for medical diagnosis, screening, treatment decisions, or other clinical use.**

## Learning objectives

- Prepare tabular data for a PyTorch model.
- Split data into training and test sets.
- Standardize features and encode labels.
- Implement a custom `Dataset` and a neural network with `torch.nn`.
- Train with binary cross-entropy and stochastic gradient descent.
- Evaluate binary predictions on held-out test data.

## Project structure

```text
pytorch-breast-cancer-classification/
├── .gitignore
├── README.md
├── requirements.txt
└── Bcancer_DL.ipynb
```

## Dataset

The notebook loads `data.csv` from the following GitHub-hosted dataset mirror when it runs:

<https://raw.githubusercontent.com/gscdit/Breast-Cancer-Detection/refs/heads/master/data.csv>

The data is associated with the Breast Cancer Wisconsin (Diagnostic) dataset. The notebook expects an internet connection to retrieve the CSV; it does not require a separate local dataset file. The notebook drops the `id` and `Unnamed: 32` columns, uses the first remaining column as the target, and uses the remaining columns as features.

## Technologies

- Python
- PyTorch
- NumPy
- pandas
- scikit-learn
- Jupyter Notebook

## How the model works

### Preprocessing

1. Load the CSV with `pandas.read_csv`.
2. Remove the `id` and `Unnamed: 32` columns.
3. Split the rows into training and test sets with `train_test_split` (`test_size=0.2`).
4. Fit `StandardScaler` on the training features and use it to transform both sets.
5. Fit `LabelEncoder` on the training labels and use it to transform both sets.
6. Convert the arrays to PyTorch tensors and wrap them in a custom `Dataset`.

The notebook does not set a random seed or `random_state`, so the data split and model initialization can vary between runs.

### Neural network architecture

`MySimpleNN` has one `Linear` layer from the input features to one output, followed by a `Sigmoid` activation. It returns a probability for the positive class.

### Training

- Loss: `BCELoss`
- Optimizer: stochastic gradient descent (`SGD`)
- Learning rate: `0.1`
- Epochs: `30`
- Training mode: full-batch; each epoch uses the complete training tensor

The notebook prints the training loss for each epoch.

### Evaluation

The notebook evaluates the test set in batches of 32 with gradients disabled. It thresholds predicted probabilities at `0.5`, compares the resulting binary predictions with the encoded labels, and prints an `Accuracy` value.

The implementation averages the accuracy of each batch rather than weighting batches by their size. Since the final batch may be smaller, the printed value can differ slightly from sample-weighted accuracy.

## Results

The notebook's saved output reports a test accuracy of **0.9470** for that recorded run. This value is the notebook's unweighted average of batch accuracies, not a separately calculated sample-weighted accuracy. It can vary between runs because the notebook does not fix the data split or model initialization; it is an educational example, not evidence of clinical performance.

## Limitations

- This is a small educational example, not a clinically validated model.
- The notebook does not set a random seed, so runs are not reproducible by default.
- Evaluation reports accuracy only; it does not calculate other classification metrics.
- The printed accuracy averages batch-level values without weighting for batch size.
- There is no validation set, hyperparameter search, or saved model artifact.
- The CSV is loaded from a third-party GitHub-hosted mirror at runtime.

## Possible future improvements

- Set random seeds and use a stratified, reproducible train/test split.
- Calculate sample-weighted accuracy and additional metrics such as precision, recall, F1-score, and a confusion matrix.
- Add a validation set and compare a few documented training settings.
- Save and reload the trained model and preprocessing objects.
- Add input/data checks and consider a more robust dataset download source.

## Installation

Use Python with `pip`. From the project folder, create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell prevents virtual-environment activation, use the Python executable inside `.venv` directly, or follow your system's Python environment guidance.

## Run the project

Start Jupyter from the project folder:

```powershell
jupyter notebook Bcancer_DL.ipynb
```

In the notebook, run the cells from top to bottom. An internet connection is needed for the dataset download. The training-loss lines and final accuracy are printed in the notebook when the corresponding cells execute.
