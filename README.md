# ML - Zoo Animal Classification

![Wildlife cover featuring a variety of animals](docs/images/zoo-cover.png)

*AI-generated wildlife cover artwork; illustrative, not an application screenshot.*

An educational machine learning project that explores animal traits, trains a **K-Nearest Neighbors (KNN)** classifier, and provides a **Streamlit interface** for predicting an animal name from 16 biological attributes.

The repository brings together a dataset, an exploratory Jupyter notebook, a serialized model, and an interactive prediction app. It is a learning prototype: the saved evaluation shows **0% test accuracy**, and the preprocessing needs correction before predictions can be considered reliable.

## Project overview

- Explore animal records with descriptive statistics, missing-value checks, and visualizations.
- Examine feature relationships with a correlation heatmap.
- Train a five-neighbor classifier with scikit-learn.
- Inspect a confusion matrix and classification report.
- Enter traits using sliders and display a predicted animal name in Streamlit.

Despite the app title referring to animal “type,” the notebook's target is `animal_name`, not broad categories such as mammal, bird, or reptile. The model uses structured traits rather than photographs; this is not an image-recognition system.

## Dataset and features

The included file, [`archive/zoo_data-1.csv`](archive/zoo_data-1.csv), contains **101 records**, **100 distinct animal names**, and **17 columns**: the target plus 16 input features. The repository does not currently document the dataset's original source or redistribution license.

| Feature | Meaning | Input |
| --- | --- | --- |
| `hair` | Has hair | 0 / 1 |
| `feathers` | Has feathers | 0 / 1 |
| `eggs` | Lays eggs | 0 / 1 |
| `milk` | Produces milk | 0 / 1 |
| `airborne` | Can fly | 0 / 1 |
| `aquatic` | Aquatic animal | 0 / 1 |
| `predator` | Predatory animal | 0 / 1 |
| `toothed` | Has teeth | 0 / 1 |
| `backbone` | Has a backbone | 0 / 1 |
| `breathes` | Breathes air | 0 / 1 |
| `venomous` | Venomous | 0 / 1 |
| `fins` | Has fins | 0 / 1 |
| `legs` | Number of legs | App accepts integers from 0 to 8 |
| `tail` | Has a tail | 0 / 1 |
| `domestic` | Domestic animal | 0 / 1 |
| `catsize` | Cat-sized or larger indicator | 0 / 1 |

For binary features, `0` means no and `1` means yes. The feature descriptions reflect the column conventions; an authoritative dataset dictionary is not included.

## Repository structure

```text
ML-Zoo-Animal-Classification/
|-- README.md                              # Project documentation
|-- app.py                                 # Streamlit prediction interface
|-- Zoo-Animal-Classification-ML.ipynb       # Exploration, training, evaluation
|-- model.pkl                              # Serialized KNN estimator
|-- requirements.txt                       # Existing core dependencies
|-- archive/
|   `-- zoo_data-1.csv                      # Animal traits and names
`-- docs/images/
    `-- zoo-cover.png                      # Illustrative wildlife cover
```

## Run the application locally

Install Python with `venv` support, then clone the repository:

```bash
git clone https://github.com/vams2krish/ML-Zoo-Animal-Classification.git
cd ML-Zoo-Animal-Classification
python -m venv .venv
```

Activate the environment:

```powershell
# Windows PowerShell
.venv\Scripts\Activate.ps1
```

```bash
# macOS / Linux
source .venv/bin/activate
```

Install the packages used by the app and notebook:

```bash
python -m pip install streamlit pandas numpy matplotlib seaborn scikit-learn jupyter
python -m streamlit run app.py
```

Run the command from the repository root so the app can find `model.pkl`. Open the local URL printed by Streamlit, set the trait sliders, and click **submit** to display a predicted animal name. The current implementation calculates a prediction on each Streamlit rerun; the button controls when it is displayed.

The existing `requirements.txt` lists unpinned core dependencies and omits Streamlit, Seaborn, and Jupyter; the install command above includes these. There is no tested Python-version matrix. The notebook metadata records Python 3.11.5, but the environment used to export the model is not fully documented. Serialized estimators can be version-sensitive. Only load pickle files from sources you trust.

## Explore and train in the notebook

```bash
python -m jupyter notebook Zoo-Animal-Classification-ML.ipynb
```

Before running the notebook, replace the machine-specific `filePath` assignment in the data-loading cell with:

```python
filePath = "archive/zoo_data-1.csv"
```

The notebook currently follows these steps:

1. Load the CSV and inspect its contents, summary statistics, and missing values.
2. Plot animal-name counts and feature correlations.
3. Separate `animal_name` from the 16 input features.
4. Split records into 80% training and 20% test data with `random_state=42`.
5. Apply `StandardScaler` and fit `KNeighborsClassifier(n_neighbors=5)`.
6. Generate predictions, a confusion matrix, and a classification report.
7. Predict an example record and save the estimator as `model.pkl`.

Running the export cell overwrites `model.pkl` in the working directory.

## Evaluation and current limitations

The **stored notebook output** reports accuracy of **0.00 on 21 test records**. This is an existing recorded result, not a newly reproduced benchmark. No claim of production readiness or accurate animal identification is made.

Several implementation choices explain why further work is needed:

- **Sparse target labels:** 100 distinct names across 101 rows leave almost no repeated examples per class. Many test labels have no training examples, so a classifier cannot learn to recognize those names from this split.
- **Test preprocessing:** the notebook calls `fit_transform` separately on the training and test sets. It should fit the scaler on training data and use `transform` for test data. The current test transformation also replaces the scaler's fitted state before the example prediction.
- **Training/inference mismatch:** the model is trained on scaled features, but the Streamlit app passes raw slider values. Only the KNN estimator is saved; the scaler is not exported with it.
- **Portability:** the notebook uses a machine-specific absolute dataset path, and dependencies are not pinned.
- **Input validation:** sliders allow biologically inconsistent combinations and leg counts that may be outside the dataset's observed examples.

## Suggested next steps

1. Choose a target supported by enough repeated examples; if broad animal groups are desired, add a verified class label and document its source.
2. Use a scikit-learn `Pipeline` containing `StandardScaler` and KNN, and save the entire pipeline for consistent inference.
3. Evaluate with an appropriate split or cross-validation strategy after resolving the target-label problem.
4. Pin and test dependencies, make the dataset path portable, and add clear app error handling.
5. Document dataset provenance and licensing, then add validated example predictions and an actual interface screenshot.

## Technology

**Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Jupyter · Streamlit · pickle**

## Author and licensing

Maintained by [Vamshi Krishna Adam](https://github.com/vams2krish).

No software license is currently included in this repository. Public visibility alone does not grant an open-source license. Dataset licensing must also be confirmed separately.
