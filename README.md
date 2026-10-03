# AI Programming Foundations Project: Wine Quality Data Workflow

## Project Description

This project is a reproducible Python data workflow built with NumPy, pandas, Matplotlib and Seaborn. It loads the Wine Quality dataset, cleans it with documented functions, explores it with reusable analysis functions, and presents four labelled visualizations with written interpretations. No machine learning models are trained; the aim is to build a clean foundation for later ML, deep learning and agentic AI work.

**Dataset:** Wine Quality (red and white Portuguese Vinho Verde wines), UCI Machine Learning Repository: https://archive.ics.uci.edu/dataset/186/wine+quality

The detailed written report with academic citations is in `module_summary.pdf`.

## Repository Contents

- `data_workflow.ipynb`: the complete notebook (Setup, Ingestion, Cleaning, EDA, Visualizations, Summary)
- `winequality-red.csv` and `winequality-white.csv`: the dataset files
- `figures/`: the four figures produced by the notebook
- `module_summary.pdf`: written report with citations and references
- `requirements.txt`: package versions created with `pip freeze`

## How to Run the Project

1. Install Python 3.10 or higher and Git.
2. Clone the repository and move into the folder:

   ```
   git clone https://github.com/dohigg1987/ai-programming-foundations-project.git
   cd ai-programming-foundations-project
   ```

3. (Optional) Create and activate a virtual environment:

   ```
   python -m venv .venv
   source .venv/bin/activate        # on Windows: .venv\Scripts\activate
   ```

4. Install the dependencies:

   ```
   pip install -r requirements.txt
   ```

5. Open and run the notebook:

   ```
   jupyter notebook data_workflow.ipynb
   ```

   Then choose **Kernel > Restart & Run All**. The notebook reads the two CSV files from the same folder and writes the figures to `figures/`.

To regenerate the requirements file in your own environment, run:

```
pip freeze > requirements.txt
```

## Reflection Questions

### Where could poor data cleaning introduce bias in this dataset?

Careless cleaning could change who or what is represented. Removing duplicate rows without checking affects white wines more (19.1 percent of rows removed) than red wines (15.0 percent), which shifts the balance between the two types. Deleting outliers such as very high residual sugar or chloride values would remove real wines and make the data look more typical than it is. Dropping rows with missing values, which did not arise here, could remove one group disproportionately, and grouping quality scores into bands changes how many wines appear at the extremes. I checked that mean quality barely changed after de-duplication, and I kept the outliers.

### How would this workflow change if the next step were a machine learning model?

I would split the data into training, validation and test sets before any step that learns from the data, such as scaling or imputation, to avoid data leakage. The split would be stratified by wine type and quality band and use a fixed random seed. I would add a simple baseline model, evaluation metrics suited to imbalanced classes, and wrap the cleaning functions in a scikit-learn Pipeline so the identical steps run on new data.

### How would it prepare the data for a neural network?

I would standardise the numeric features using statistics from the training set only, encode `wine_type` as a 0 or 1 input, and decide whether the target is a score to regress or a band to classify. The data would be converted to float32 arrays and fed in batches. Correlated inputs such as alcohol and density, and the rare extreme quality scores, would need attention through regularisation or class weights. The existing ingestion and cleaning functions would become the preprocessing stage.

### Could parts of this workflow be automated by an agentic AI system?

Yes. The functions are modular, documented and return reports, so an agent could call them as tools: detect a new data file, run the quality checks, clean the data, regenerate the figures and draft a summary of what changed. Judgement calls should stay with a person, for example whether duplicate rows are errors or genuine repeated samples. Sensible safeguards would be to never overwrite the raw files, log every change, and require approval before any rows are removed.
