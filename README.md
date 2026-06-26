## ✅ Accuracy Check

Your ensemble achieved outstanding performance:

- **LightGBM OOF Accuracy**: 0.9680  
- **XGBoost OOF Accuracy**: 0.9670  
- **CatBoost OOF Accuracy**: 0.9637  
- **Weighted Ensemble (weights ≈ 1/3 each)**: ~0.966 (implied)

This is **much higher** than the earlier estimate of ~0.65. The improvement likely comes from advanced feature engineering, careful hyperparameter tuning, and the multi‑seed ensemble.

---

## 📝 Updated README Section (Performance)

Replace the performance section in your `README.md` with this:

```markdown
## 📊 Performance

The ensemble was evaluated using **Stratified 5‑Fold Cross‑Validation** (OOF predictions). Individual model accuracies and ensemble weights are:

| Model       | OOF Accuracy | Ensemble Weight |
| :---------- | :----------- | :--------------- |
| LightGBM    | 0.9680       | 0.3339           |
| XGBoost     | 0.9670       | 0.3336           |
| CatBoost    | 0.9637       | 0.3325           |
| **Ensemble**| **~0.966**   | —                |

The ensemble is a **weighted average** of the three models, with weights proportional to each model’s OOF accuracy. All models use **native categorical support** and were trained with **4 different random seeds** to reduce variance and improve robustness.

> **Note**: Performance may vary slightly depending on the random seed and the specific data split.
```

---

## 📁 Full Updated README

If you want the complete updated `README.md`, here it is (incorporating the new accuracy numbers):

```markdown
# Stellar Classifier Ensemble – Galaxy / Star / QSO Classification

This repository contains a complete machine learning pipeline for classifying celestial objects into **Galaxy**, **Star**, or **QSO (Quasar)** using SDSS photometric data (u, g, r, i, z magnitudes and redshift). It was developed for the [Kaggle Playground Series – Season 6, Episode 6](https://www.kaggle.com/competitions/playground-series-s6e6) competition.

The solution combines **domain‑specific feature engineering** (color indices, locus distances, redshift interactions) with a **multi‑model ensemble** of LightGBM, XGBoost, and CatBoost, using stratified K‑Fold cross‑validation and multiple random seeds.

---

## 📊 Dataset

The dataset is a synthetically generated version of the SDSS (Sloan Digital Sky Survey) stellar classification data. It includes:

- **Photometric features**: `u`, `g`, `r`, `i`, `z` (magnitudes) and `redshift`.
- **Positional features**: `alpha` (RA), `delta` (Dec).
- **Categorical features**: `spectral_type` (e.g., `G/K`, `M`, `O/B`) and `galaxy_population` (`Red_Sequence`, `Blue_Cloud`).
- **Target**: `class` – one of `GALAXY`, `STAR`, or `QSO`.

The training set contains ~577k samples, and the test set ~247k samples.

---

## 🧠 Approach

### 1. Exploratory Data Analysis (EDA)
- Visualised class distributions, colour‑colour diagrams (`u-g` vs `g-r`), redshift distributions, and correlation matrices.
- Identified that magnitudes are highly correlated, motivating the use of color indices and domain‑inspired features.

### 2. Feature Engineering
**Core Color Indices**:
- `u-g`, `g-r`, `r-i`, `i-z` – standard SDSS colours.

**Astronomical “Locus Distances”** (domain knowledge):
- `dist_stellar` – Euclidean distance to the stellar locus (`u-g=0.8`, `g-r=0.4`)
- `dist_qso` – distance to the QSO locus (`u-g=-0.5`, `g-r=0.0`)
- `dist_galaxy` – distance to the galaxy locus (`u-g=1.2`, `g-r=0.6`)

**Redshift Enhancements**:
- `redshift_bin` – binned redshift (0–0.5, 0.5–1, 1–2, >2)
- `redshift_x_u_g` – interaction term to help separate high‑redshift QSOs.

**Advanced Features**:
- `stellar_locus_deviation` – deviation from the stellar locus line.
- `magnitude_sum`, `magnitude_range`, `u_g_squared`, `g_r_squared`, `redshift_x_mag_sum`, etc.

All features were engineered on both train and test sets consistently.

### 3. Modeling
- **Algorithms**: LightGBM, XGBoost, CatBoost (all support native categorical features).
- **Cross‑validation**: Stratified 5‑fold (preserves class proportions).
- **Ensemble**: 4 random seeds (`42, 123, 456, 789`) were used to train models on each fold, and predictions were averaged with weights proportional to each model’s out‑of‑fold (OOF) accuracy.
- **GPU Acceleration**: Used `device='cuda'` for XGBoost and `task_type='GPU'` for CatBoost to speed up training.

### 4. Performance

| Model       | OOF Accuracy | Ensemble Weight |
| :---------- | :----------- | :--------------- |
| LightGBM    | 0.9680       | 0.3339           |
| XGBoost     | 0.9670       | 0.3336           |
| CatBoost    | 0.9637       | 0.3325           |
| **Ensemble**| **~0.966**   | —                |

---

## 🚀 How to Run

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/stellar-classifier-ensemble.git
   cd stellar-classifier-ensemble
   ```

2. **Install dependencies** (see `requirements.txt`):
   ```bash
   pip install -r requirements.txt
   ```

3. **Open the notebook**:
   ```bash
   jupyter notebook Predicting_Stellar_Class.ipynb
   ```

4. **Run all cells sequentially** – the notebook will:
   - Download the Kaggle competition data (requires Kaggle API credentials).
   - Perform EDA, feature engineering, and training.
   - Generate a `submission.csv` file ready for Kaggle upload.

> **Note**: You need a Kaggle API key (place `kaggle.json` in `~/.kaggle/` or login via the notebook widget).

---

## 📁 Repository Structure

```
stellar-classifier-ensemble/
├── Predicting_Stellar_Class.ipynb   # Main notebook
├── README.md                        # This file
├── requirements.txt                 # Python dependencies
```

---

## 📦 Dependencies

- Python 3.9+
- numpy, pandas
- matplotlib, seaborn
- scikit-learn
- lightgbm, xgboost, catboost
- kagglehub (for data download)
- (Optional) CUDA‑compatible GPU for faster training.

Full list in `requirements.txt`.

---

## 📝 License

This project is open‑source under the **MIT License**. Feel free to use, modify, and distribute.

---

## 🙏 Acknowledgments

- [Kaggle](https://www.kaggle.com/) for the Playground Series dataset.
- The SDSS collaboration for the original photometric data.
- The open‑source community for the amazing libraries used.

---

## ⚠️ Limitations

- The model is **not suitable for scientific astronomy** due to moderate accuracy (~65%) and the synthetic nature of the dataset.
- Feature engineering is tailored to the SDSS photometric system; may not generalise to other surveys without adaptation.
- The ensemble is computationally heavy – consider reducing `seeds` or `NUM_ROUNDS` for faster experimentation.

---

## 🤝 Contributing

Issues and pull requests are welcome. If you find a bug or have a suggestion, please open an issue.

---

**Happy classifying!** ✨
```
