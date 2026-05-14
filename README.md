# AS-clustering
Repo for materials related to a clustering project for my dissertation
# AS Network Clustering with Deep Learning

This project explores how deep learning can be used to cluster Autonomous Systems (ASes) and better understand the structure of the internet. Rather than relying on manually labeled data or traditional distance-based clustering methods alone, it uses an Autoencoder combined with a Gaussian Mixture Model (DEC-GM) to uncover natural groupings in BGP and routing data.

The repository includes the full workflow: data preprocessing, model training, clustering evaluation, visualization tools, and route analysis utilities. It also comes with a cleaned dataset of roughly 78,000 ASNs and pretrained model weights so the results can be reproduced without retraining the network from scratch.

---

# Repository Contents

- **as_cluster.ipynb**  
  Main notebook containing the entire pipeline:
  - data preprocessing
  - model training in PyTorch
  - Optuna hyperparameter optimization
  - clustering evaluation
  - traceroute and route analysis tools

- **final_dataset_cleaned.csv**  
  Dataset containing 13 engineered routing and BGP-related features, including:
  - provider/customer relationships
  - IP address space
  - peering ratios
  - topology-related metrics

- **optuna_models/**  
  Pretrained `.pth` model weights that allow the training stage to be skipped.

---

# Architecture and Methodology

The project moves beyond standard clustering approaches such as K-Means by using a DEC-GM architecture implemented in PyTorch.

The model first compresses high-dimensional routing data into an 8-dimensional latent representation using an autoencoder. A Gaussian Mixture layer is then applied in the latent space to assign clusters probabilistically by minimizing Negative Log-Likelihood (NLL).

Hyperparameter tuning is handled with Optuna, which optimizes the balance between:
- reconstruction quality
- cluster separation
- entropy regularization

This produces clusters that are both distinct and structurally meaningful while avoiding unstable or fragmented partitions.

---

# Traceroute and Route Analysis

The notebook includes an `analyze_route` utility for working with real traceroute data.

The function:
1. parses traceroute text files
2. maps each hop to its corresponding ASN
3. determines the DEC-GM cluster assignment
4. outputs prediction entropy for each hop

This makes it possible to observe how traffic traverses different network tiers and organizational structures across the internet.

---

# Visualizations

Several visualization tools are included to help interpret both the latent space and the clustering results:

- **PCA Projection**  
  A 2D baseline representation of the AS ecosystem before clustering.

- **Raw DEC-GM Clusters**  
  Initial probabilistic cluster assignments generated directly from the neural network.

- **Cleaned Operational Tiers**  
  Final tier structure after automatically merging very small edge-case clusters.

- **Algorithm Comparison**  
  Side-by-side comparison of DEC-GM against:
  - K-Means
  - BIRCH
  - standard Gaussian Mixture Models

---

# Setup and Usage

Built with:
- Python 3.13
- PyTorch
- pandas
- scikit-learn
- matplotlib
- Optuna
- HDBSCAN

Run the notebook from top to bottom to execute the full pipeline:

```bash
jupyter notebook as_cluster.ipynb
