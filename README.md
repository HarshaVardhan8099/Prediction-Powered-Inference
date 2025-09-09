# Prediction-Powered-Inference
Estimating the fraction of the Amazon rainforest lost between 2000 and 2015

## Credits

This project builds on the framework introduced in the paper:

**Prediction-Powered Inference**  
Anastasios N. Angelopoulos, Stephen Bates, Clara Fannjiang, Michael I. Jordan, and Tijana Žrnić  
*arXiv preprint arXiv:2301.09633, 2023*  
📄 [Read the paper](https://arxiv.org/abs/2301.09633)

Specifically, the following files were adapted or reused from the authors' official repository:
- `forest.ipynb`
- `utils.py`
- `forest.npz`

All credit for these resources goes to the original authors.  

## Setup

This project builds on the [Prediction-Powered Inference (PPI)](https://github.com/aangelopoulos/ppi_py) repository.

### Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/aangelopoulos/ppi_py.git
pip install -r requirements.txt
```

Download utils.py from the PPI repo and place it in your working directory:

## Analysis of Amazon Deforestation: Notebook Overview(forest.ipynb)  

This notebook estimates the fraction of the Amazon rainforest lost between 2000 and 2015.  
It compares two statistical approaches: the traditional **Classical** method and the more modern **Prediction-Powered Inference (PPI)** method.  

---

### 1. Import Necessary Packages  
The environment is set up with the following libraries:  
- **numpy**: numerical computing and array operations  
- **pandas**: data manipulation and DataFrame storage  
- **ppi_py**: custom library containing PPI (`ppi_mean_ci`) and Classical (`classical_mean_ci`) inference functions  
- **tqdm**: progress bars for simulations  
- **scipy.optimize.brentq**: root-finding for the power experiment  
- **utils**: helper functions (e.g., `make_plots`) for visualization  

---

### 2. Import the Forest Dataset  
- **Y (Gold-Standard Labels):** verified deforestation data from costly field surveys  
- **Yhat (Predicted Labels):** satellite-based machine learning predictions  

**Core question:** Can the large, inexpensive prediction dataset improve inference from the small, expensive labeled dataset?  

---

### 3. Problem Setup  
Defined parameters:  
- **α = 0.05** → significance level (5% chance of false rejection)  
- **n_total** → total number of labeled samples available  
- **ns** → range of labeled sample sizes (50–500)  
- **num_trials** → repetitions for robust results  
- **true_theta** → mean of `Y_total`, i.e., the true deforestation fraction  

---

### 4. Construct Confidence Intervals  
The notebook compares three methods:  

1. **PPI** → combines labeled `Y` and both labeled/unlabeled `Yhat`, correcting bias and producing narrower intervals  
2. **Classical** → uses only labeled `Y`, ignores predictions  
3. **Imputation** → uses only `Yhat_total` predictions (fast but biased)  

All results (method, sample size, confidence interval bounds, trial) are stored in a DataFrame.  

---

### 5. Plot Results  
Plots compare confidence interval widths:  
- **PPI intervals** → consistently narrower than Classical, showing greater efficiency  
- **Classical intervals** → wider, less precise  
- **Imputation intervals** → biased, often fail to cover the truth  

---

### 6. Power Experiment  
Goal: Find the smallest labeled sample size \(n\) required for **80% power** to reject the null hypothesis (\(H_0\): no deforestation).  

- **Helper functions:**  
  - `_to_invert_ppi(n)`  
  - `_to_invert_classical(n)`  
- **brentq root-finding:** finds the \(n\) where rejection rate = 80%  

**Results:**  
- PPI → requires **22 labeled samples**  
- Classical → requires **32 labeled samples**  

---

### ✅ Summary  
By incorporating predictions from the large, inexpensive dataset, **Prediction-Powered Inference (PPI)** achieves the same level of statistical confidence with fewer labeled data points than the Classical method.  
This makes PPI a more **efficient** and **cost-effective** approach for monitoring Amazon deforestation.  


# Limitations of Prediction-Powered Inference (PPI)

1. **Dependence on Model Accuracy**  
   - PPI relies heavily on the predictive model used to augment inference.  
   - If the model is poorly calibrated or biased, the inference results can be misleading.

2. **Assumption of Exchangeability**  
   - PPI often assumes that labeled and unlabeled data come from the same distribution.  
   - Violations (e.g., domain shift) can reduce validity.

3. **Label Scarcity Issues**  
   - While PPI is designed for limited labeled data, having too few labels can lead to unstable corrections and wide confidence intervals.

4. **Complex Error Propagation**  
   - Errors in model predictions propagate through the inference process, making interpretation more challenging.  

5. **Computational Complexity**  
   - Requires both fitting a predictive model and performing statistical corrections, which may be computationally intensive on large datasets.

6. **Sensitivity to Model Misspecification**  
   - If the predictive model fails to capture important relationships, the efficiency gains of PPI diminish significantly.

7. **Limited Applicability**  
   - Best suited for supervised learning contexts with a mix of labeled and unlabeled data.  
   - Not always generalizable to unsupervised or reinforcement learning settings.

8. **Interpretability Challenges**  
   - The statistical corrections can make inference results harder to explain to non-technical stakeholders.


