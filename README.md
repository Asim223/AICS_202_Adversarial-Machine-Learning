# Adversarial Machine Learning | AICS-202

**Integrated Practical Assessment | International Cybersecurity & Digital Forensics Academy (ICDFA)**

A practical study of adversarial machine learning covering baseline threat modelling, adversarial evasion attacks, training-label poisoning, dataset integrity, and adaptive defence evaluation using a locally trained machine learning classifier.

---

## Project Overview

This project investigates the security weaknesses of machine learning models through four controlled laboratory experiments. It evaluates how adversarial input perturbations affect model predictions, how training-label corruption influences model performance, and whether adversarial data augmentation can improve model robustness.

All experiments were conducted locally using the Scikit-learn handwritten digits dataset. No production systems, external services, third-party models, or personal data were accessed.

### Key Objectives

- Establish a baseline machine learning classifier and define its threat model.
- Evaluate inference-time evasion using the Fast Gradient Sign Method (FGSM).
- Investigate training-label poisoning and dataset integrity.
- Implement SHA-256 hashing and independent label auditing.
- Evaluate adversarial retraining against adaptive FGSM and PGD attacks.
- Develop a risk register and prioritised security recommendations.

## Laboratory Experiments

### Lab 01: Baseline Model and Threat Modelling

- Dataset: Scikit-learn handwritten digits.
- Model: Multinomial Logistic Regression.
- Training samples: 1,347.
- Testing samples: 450.
- Train-test split: 75/25, stratified.
- Random seed: 42.
- Baseline accuracy: **96.2%**.

Established the baseline performance, examined classification errors, and documented attacker objectives, capabilities, protected assets, and security boundaries.

### Lab 02: Adversarial Evasion Testing (FGSM)

Implemented a white-box, untargeted Fast Gradient Sign Method attack to investigate the impact of bounded perturbations on model predictions.

**Attack configuration:**
- Attack: FGSM.
- Perturbation norm: L-infinity.
- Epsilon values: 0.03, 0.07, 0.12, 0.15, 0.18.
- Input clipping: [0, 1].
- Evaluation: Unchanged held-out test dataset.

| Epsilon | Adversarial Accuracy |
|---|---:|
| 0.00 | 96.2% |
| 0.03 | 90.7% |
| 0.07 | 79.1% |
| 0.12 | 51.6% |
| 0.15 | 36.0% |
| 0.18 | 20.4% |

**Key finding:** At epsilon 0.12, model accuracy decreased by 44.7 percentage points compared with baseline performance.

### Lab 03: Training-Label Poisoning and Dataset Integrity

Investigated the effects of random training-label corruption at different percentages.

**Corruption levels:** 0%, 5%, 10%, and 20%.

Implemented:
- Controlled training-label corruption.
- SHA-256 dataset hash manifest.
- Training and testing partition integrity verification.
- Independent label auditing using five-fold cross-validation.
- Dataset provenance and version-control recommendations.
- Incident investigation and escalation workflow.

**Key findings:**
- Accuracy decreased from 96.2% to 93.3% in the primary experiment with 20% corrupted labels.
- Ten-seed mean accuracy at 20% corruption was 93.5%.
- Label audit achieved 0.72 precision and 0.99 recall at 10% corruption.
- SHA-256 verification detected training-label modifications while confirming that the held-out test partition remained unchanged.

### Lab 04: Defensive Retraining and Adaptive Assessment

Evaluated adversarial data augmentation as a potential defence against FGSM and Projected Gradient Descent (PGD) attacks.

Implemented and compared:
- Baseline classifier.
- Augmentation A: epsilon 0.10, one round.
- Augmentation B: epsilon 0.03, one round.
- Augmentation C: epsilon 0.05, one round.
- Student variant: epsilon 0.05, three rounds.

Every defended model was evaluated using attacks generated against the model being tested, ensuring adaptive evaluation.

| Model | Clean Accuracy | Adaptive FGSM | Adaptive PGD |
|---|---:|---:|---:|
| Baseline | 96.2% | 51.6% | 48.9% |
| Augmentation A | 94.0% | 28.4% | 26.0% |
| Augmentation B | 96.2% | 46.7% | 43.8% |
| Augmentation C | 95.8% | 43.3% | 40.2% |
| Student Variant | 96.7% | 47.6% | 46.7% |

**Security decision: REJECT (as a security control).**

None of the tested augmentation variants achieved the predefined robustness improvement criteria. The experiments demonstrate why adaptive attack evaluation is essential when assessing adversarial defences.

---

## Technology Stack

| Category | Technologies |
|---|---|
| Programming Language | Python 3.13.18 |
| Environment | Google Colab |
| Machine Learning | Scikit-learn 1.6.1 |
| Numerical Computing | NumPy 2.4.6 |
| Data Visualisation | Matplotlib 3.10.0 |
| Security Techniques | FGSM, PGD, Label Poisoning |
| Integrity Verification | SHA-256 |
| Evaluation | Accuracy, Precision, Recall, F1-score |
| Reproducibility | Fixed Random Seeds, CSV Evidence, JSON Manifests |

## Repository Structure

```text
AICS_202_Adversarial-Machine-Learning/
│
├── AICS202_Adversarial_ML_Assessment.ipynb
│
├── Labs/
│   ├── Lab_01/
│   ├── Lab_02/
│   ├── Lab_03/
│   └── Lab_04/
│
├── Results/
│   ├── figures/
│   ├── tables/
│   ├── evidence/
│   └── logs/
│
├── Submission/
│   ├── AICS202_Final_Report.pdf
│   ├── README.md
│   ├── evidence_index.md
│   └── results.zip
│
└── requirements.txt
```

*Note: The structure above represents the intended project organisation. Adjust filenames and folders to match the files actually committed to the repository.*

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Asim223/AICS_202_Adversarial-Machine-Learning.git
cd AICS_202_Adversarial-Machine-Learning
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

Alternatively:

```bash
pip install numpy==2.4.6 scikit-learn==1.6.1 matplotlib==3.10.0
```

### 3. Run the Notebook

Open `AICS202_Adversarial_ML_Assessment.ipynb` in Google Colab or Jupyter Notebook.

Execute all cells sequentially to reproduce the experiments, generate evaluation metrics, and produce the associated figures and evidence files.

## Key Security Findings

1. **Adversarial vulnerability:** High clean accuracy does not guarantee robustness against adversarial inputs.
2. **Dataset integrity:** Training-label modifications can be detected through cryptographic hashing and independent auditing.
3. **Adaptive evaluation:** Defences must be evaluated against attacks generated specifically for the defended model.
4. **Negative experimental results:** The tested adversarial augmentation configurations did not satisfy the predefined security improvement criteria.
5. **Security governance:** Model deployment should include dataset provenance, integrity verification, adaptive testing, monitoring, and independent approval.

## Limitations

- Experiments were conducted on a small 8 × 8 handwritten-digit dataset.
- Only a logistic-regression model was evaluated.
- Evasion testing assumed white-box attacker knowledge.
- Label poisoning used random corruption rather than targeted backdoor attacks.
- PGD evaluation used seven steps and is not an exhaustive robustness assessment.
- The findings do not constitute certification of machine learning model security.

## References and Further Reading

- [Goodfellow et al. (2015) - Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)
- [Madry et al. (2018) - Towards Deep Learning Models Resistant to Adversarial Attacks](https://arxiv.org/abs/1706.06083)
- [Carlini et al. (2019) - On Evaluating Adversarial Robustness](https://arxiv.org/abs/1902.06705)
- [MITRE ATLAS - Adversarial Threat Landscape for AI Systems](https://atlas.mitre.org/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST Adversarial Machine Learning: Taxonomy and Terminology (2025)](https://csrc.nist.gov/pubs/ai/100/2/e2025/final)
- [Scikit-learn Digits Dataset](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_digits.html)

## Academic Information

**Name:** Asim Shayan Khan

**Programme:** AI-Driven Cybersecurity and Digital Forensics Fellowship

**Institution:** International Cybersecurity & Digital Forensics Academy (ICDFA)

**Cohort:** AICDF-COHORT-2026-Q1

**Course:** AICS-202 | Adversarial Machine Learning

**Assessment:** Integrated Practical Assessment (Labs 01–04)

**Date:** 18 November 2026

---

## Disclaimer

This repository is intended strictly for educational, academic, and defensive cybersecurity research purposes. All experiments were conducted using locally owned models and an offline dataset in a controlled laboratory environment. No unauthorised systems or third-party services were targeted.

**Defensive AI | Responsible Research | Adversarial Machine Learning**
