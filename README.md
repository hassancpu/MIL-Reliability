# 🚀 MIL-Reliability

**Beyond accuracy: Quantifying the reliability of multiple instance learning for whole slide image classification**

This repository contains the official implementation, experiments, and evaluation code for our work on **MIL reliability assessment**.

<p align="center">
  <img src="https://github.com/tueimage/MIL-Reliability/raw/main/figures/framework.png" width="1000"/>
</p>
<p align="center"><em>Figure 1: Overall framework for evaluating the reliability of MIL models.</em></p>

---

## 📌 Table of Contents
1. [Overview](#overview)  
2. [Data Preparation](#data-preparation)  
3. [Training](#training)  
4. [Evaluation](#evaluation)  
5. [Reliability Computation](#reliability-computation)  
6. [Results](#results)  
7. [Citation](#citation)  
8. [License](#license)  
---

<a name="overview"></a>
## 🧠 Overview

This project provides a unified pipeline to **quantitatively evaluate the reliability of multiple instance learning (MIL) models**.  It is designed for WSI classification tasks and supports several MIL architectures (e.g., **ABMIL**, **CLAM**, etc.). Reliability in this study is defined as the consistent focus of MIL models on diagnostically relevant ROIs within WSIs, a prerequisite for trustworthy and clinically useful predictions. To quantify this alignment, we selected three complementary metrics that together capture different aspects of spatial concordance between predicted patch scores and ground truth annotations:
- **Mutual Information (MI)**  
- **Spearman’s Correlation (Spearman’s)**  
- **Area Under the Precision-Recall Curve (AUPRC)**

---

<a name="data-preparation"></a>
## 📂 Data Preparation

We follow the preprocessing pipeline of **[CLAM](https://github.com/mahmoodlab/CLAM)**. Pre-extracted patch features should be organized similarly to CLAM’s directory structure. Please refer to the original CLAM documentation or the accompanying paper for detailed guidance.

Download the extracted features from Zenodo: 

- **BRCA:** [Download](https://zenodo.org/records/10563985/files/BRCA.zip?download=1)
- **Camelyon16:** [Download](https://zenodo.org/records/18596568/files/Camelyon16.zip?download=1)
- **CATCH:** [Download](https://zenodo.org/records/18596568/files/CATCH.zip?download=1)
  
Organize your dataset directory accordingly for training.

---

<a name="training"></a>
## 🏋️ Training

Train MIL models using:

```
python train.py \
  --data_root_dir feat-directory ... \
  --lr 1e-4 --reg 1e-5 --seed 2021 \
  --k 5 --k_end 5 \
  --split_dir task_camelyon16 \
  --model_type abmil \
  --task task_1_tumor_vs_normal \
  --csv_path ./dataset_csv/camelyon16.csv \
  --exp_code ABMIL
```

<a name="evaluation"></a>
## 🔍 Evaluation

After training, compute and store patch-level attention / prediction scores:

```
python eval.py \
  --drop_out \
  --k 5 --k_start 0 --k_end -1 \
  --models_exp_code ABMIL_s2021 \
  --save_exp_code ABMIL_eval \
  --task task_1_tumor_vs_normal \
  --model_type abmil \
  --results_dir results \
  --data_root_dir ...
```

<a name="reliability-computation"></a>
## 📊 Reliability Computation

Compute reliability metrics across folds:

```
python reliability.py \
  --model_name ABMIL \
  --att_path ... \
  --anno_path ...
```

<a name="results"></a>
## 📈 Results

Below are reliability comparisons across MIL models based on AUPRC, Spearman correlation, and Mutual Information.

<p align="center"> <img src="https://github.com/tueimage/MIL-Reliability/raw/main/figures/bar_plot_AUPRC.png" width="350" /> <img src="https://github.com/tueimage/MIL-Reliability/raw/main/figures/bar_plot_Spearmans.png" width="350" /> <img src="https://github.com/tueimage/MIL-Reliability/raw/main/figures/bar_plot_MI.png" width="350" /> </p> <p align="center"><em>Figure 2: Reliability evaluation of different MIL architectures.</em></p>


<a name="citation"></a>
## 📚 Citation

If you find this repository useful, please consider citing:

```bibtex
@article{keshvarikhojasteh2025beyond,
  title={Beyond accuracy: Quantifying the reliability of multiple instance learning for whole slide image classification},
  author={Keshvarikhojasteh, Hassan and Aubreville, Marc and Bertram, Christof A and Pluim, Josien PW and Veta, Mitko},
  journal={PloS one},
  volume={20},
  number={12},
  pages={e0337261},
  year={2025},
  publisher={Public Library of Science San Francisco, CA USA}
}
```

<a name="license"></a> 
## ⚖ License 

This repository is licensed under MIT License.
