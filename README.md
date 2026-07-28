# FedSemPro: A Semantic Prototype-Based Framework for Early Detection and Robust Aggregation Against Label-Flipping Attacks in Federated Bangla NLP

This repository contains the implementation of **FedSemPro**, a federated learning framework for detecting malicious client  under label-flipping attacks. The framework supports both **IID** and **non-IID** data distributions and allows experiments with different numbers of honest and malicious clients.

The main notebook is:

```text
5client9700-30i-iid.ipynb
5client9700_30nonIID.ipynb
```

## Main Features


- Federated learning with multiple clients
- Honest and malicious client simulation
- Label-flipping attack experiments
- IID and non-IID data distribution
- LoRA-based parameter-efficient fine-tuning
- Semantic prototype-based malicious-client detection
- Trust-based client filtering and aggregation
- Experiments with different malicious-client participation levels

## Dataset Files

The repository uses processed Bangla sentiment datasets with the following columns:

```text
review
polarity
```

The sentiment labels are:

```text
positive
negative
```

Recommended dataset directory:

```text
data/
├── 7000Correct.xlsx
├── 2000LabelFlip.xlsx
├── 4850Correct.xlsx
├── 4850LabelFlip.xlsx
└── trustfl.xlsx
```

The exact filenames may be changed, but the corresponding paths in the notebook must also be updated.

## Experimental Configurations

The framework supports two main types of experiments:

1. **Attack-level experiments using different correct and label-flipped dataset sizes**
2. **Malicious-client participation experiments using different numbers of clients**

Both experiment types can be evaluated under **IID** and **non-IID** settings.

---

## 1. Five-Client Setup

For the five-client experiments, use:

```text
Total clients: 5
Honest clients: 3
Malicious clients: 2
```

Client assignment:

| Client ID | Client Type |
|---|---|
| Client 0 | Honest |
| Client 1 | Honest |
| Client 2 | Honest |
| Client 3 | Malicious |
| Client 4 | Malicious |

The same five-client configuration can be used for both IID and non-IID experiments.

### Lower Attack Setting

Use the following datasets:

```text
Correct dataset: 7000Correct.xlsx
Label-flipped dataset: 2000LabelFlip.xlsx
```

Run this configuration separately for:

```text
5-client IID experiment
5-client non-IID experiment
```

### Balanced 50% Data Setting

Use equal amounts of correct and label-flipped data:

```text
Correct dataset: 4850Correct.xlsx
Label-flipped dataset: 4850LabelFlip.xlsx
```

Run this configuration separately for:

```text
5-client IID experiment
5-client non-IID experiment
```

> **Important:** In a five-client setup with three honest and two malicious clients, the malicious-client participation rate is 40% because 2 out of 5 clients are malicious. Therefore, the lower attack percentage should be treated as a dataset or attack-setting label rather than a 30% malicious-client participation rate.

---

## 2. Malicious-Client Participation Experiments

To study the effect of increasing malicious-client participation, use the following client configurations.

### Six-Client Setup: 50% Malicious Participation

```text
Total clients: 6
Honest clients: 3
Malicious clients: 3
Malicious participation: 50%
```

Suggested client assignment:

| Client ID | Client Type |
|---|---|
| Client 0 | Honest |
| Client 1 | Honest |
| Client 2 | Honest |
| Client 3 | Malicious |
| Client 4 | Malicious |
| Client 5 | Malicious |

Run both:

```text
6-client IID experiment
6-client non-IID experiment
```

### Seven-Client Setup: Approximately 57% Malicious Participation

```text
Total clients: 7
Honest clients: 3
Malicious clients: 4
Malicious participation: 4/7 = 57.14%
```

Suggested client assignment:

| Client ID | Client Type |
|---|---|
| Client 0 | Honest |
| Client 1 | Honest |
| Client 2 | Honest |
| Client 3 | Malicious |
| Client 4 | Malicious |
| Client 5 | Malicious |
| Client 6 | Malicious |

Run both:

```text
7-client IID experiment
7-client non-IID experiment
```

### Eight-Client Setup: 75% Malicious Participation

```text
Total clients: 8
Honest clients: 2
Malicious clients: 6
Malicious participation: 6/8 = 75%
```

Suggested client assignment:

| Client ID | Client Type |
|---|---|
| Client 0 | Honest |
| Client 1 | Honest |
| Client 2 | Malicious |
| Client 3 | Malicious |
| Client 4 | Malicious |
| Client 5 | Malicious |
| Client 6 | Malicious |
| Client 7 | Malicious |

Run both:

```text
8-client IID experiment
8-client non-IID experiment
```

---

## Configuration Summary

| Experiment | Total Clients | Honest | Malicious | Malicious Participation | Data Distribution |
|---|---:|---:|---:|---:|---|
| Five-client lower attack setting | 5 | 3 | 2 | 40% client participation | IID and non-IID |
| Five-client balanced data setting | 5 | 3 | 2 | 40% client participation | IID and non-IID |
| Six-client setting | 6 | 3 | 3 | 50% | IID and non-IID |
| Seven-client setting | 7 | 3 | 4 | 57.14% | IID and non-IID |
| Eight-client setting | 8 | 2 | 6 | 75% | IID and non-IID |

## IID Data Distribution

In the IID setting, each client should receive approximately the same class distribution.

Example:

```text
Each honest client receives a similar proportion of positive and negative reviews.
Each malicious client receives a similar proportion of label-flipped positive and negative reviews.
```

The data should be shuffled before it is divided among clients.

## Non-IID Data Distribution

In the non-IID setting, the class distribution should vary across clients.

For example:

```text
One client may receive mostly positive reviews.
Another client may receive mostly negative reviews.
Other clients may receive different class proportions.
```

The same non-IID partitioning strategy should be used consistently when comparing different defence methods.

## Updating the Notebook

Before running an experiment, update the main configuration variables.

Example:

```python
NUM_CLIENTS = 5
HONEST_CLIENTS = [0, 1, 2]
MALICIOUS_CLIENTS = [3, 4]
```

For six clients:

```python
NUM_CLIENTS = 6
HONEST_CLIENTS = [0, 1, 2]
MALICIOUS_CLIENTS = [3, 4, 5]
```

For seven clients:

```python
NUM_CLIENTS = 7
HONEST_CLIENTS = [0, 1, 2]
MALICIOUS_CLIENTS = [3, 4, 5, 6]
```

For eight clients:

```python
NUM_CLIENTS = 8
HONEST_CLIENTS = [0, 1]
MALICIOUS_CLIENTS = [2, 3, 4, 5, 6, 7]
```

Also update the dataset paths according to the selected experiment.

Example for the lower attack setting:

```python
df_clean = pd.read_excel("data/7000Correct.xlsx")
df_malicious = pd.read_excel("data/2000LabelFlip.xlsx")
```

Example for the balanced data setting:

```python
df_clean = pd.read_excel("data/4850Correct.xlsx")
df_malicious = pd.read_excel("data/4850LabelFlip.xlsx")
```

## Running the Experiments

For every configuration, run the following two experiments separately:

```text
1. IID experiment
2. Non-IID experiment
```

Recommended experiment list:

```text
5-client lower attack IID
5-client lower attack non-IID

5-client balanced data IID
5-client balanced data non-IID

6-client 50% malicious IID
6-client 50% malicious non-IID

7-client 57% malicious IID
7-client 57% malicious non-IID

8-client 75% malicious IID
8-client 75% malicious non-IID
```

Use the same random seed, model configuration, number of rounds, local epochs, trust dataset, and evaluation procedure for fair comparison.

## Recommended Repository Structure

```text
.
├── README.md
├── notebooks/
│   ├── 5client_iid.ipynb
│   ├── 5client_non_iid.ipynb
│   ├── 6client_iid.ipynb
│   ├── 6client_non_iid.ipynb
│   ├── 7client_iid.ipynb
│   ├── 7client_non_iid.ipynb
│   ├── 8client_iid.ipynb
│   └── 8client_non_iid.ipynb
├── data/
│   ├── 7000Correct.xlsx
│   ├── 2000LabelFlip.xlsx
│   ├── 4850Correct.xlsx
│   ├── 4850LabelFlip.xlsx
│   └── trustfl.xlsx
└── results/
```

## Reproducibility Notes
-after importing library restart the experiment
- Keep the processed datasets unchanged when reproducing the journal experiments.
- Use the same random seed for all comparative experiments.
- Keep the global validation and test sets fixed.
- Use the same trust dataset across all experiments.
- Do not mix IID and non-IID results.
- Save every experiment using a unique output filename.
- Record the exact number of honest and malicious clients for each run.
- Record the correct and label-flipped dataset sizes used in each run.
- Clearly distinguish the data attack setting from the malicious-client participation rate.



