🛡️ Deep Learning Network Intrusion Detection System (NIDS)
An advanced, Deep Learning-based Network Intrusion Detection System designed to identify and classify malicious network traffic. This project leverages the massive CIC-DDoS 2019 dataset (sourced from Kaggle) to perform highly accurate multinomial classification across 13 distinct DDoS attack vectors alongside benign traffic.

Achieved a peak accuracy of 97.12% using a custom 1D CNN + Inception Module architecture.

📊 Dataset Overview
Source: CIC-DDoS 2019 (Kaggle)

Total Combined Rows: 1,130,650

Final Processed Rows: 423,722

Classification Types: 13 Attack Classes + 1 Benign Class

🧹 Data Preprocessing & Cleaning
Network traffic datasets are notoriously noisy. Extensive preprocessing and domain-knowledge-driven feature selection were applied to clean the 1.13+ million rows:

1. Feature Reduction (88 → 66 Features)
Using cybersecurity domain knowledge, 22 specific features were permanently dropped from the original 88-column dataset. These included:

Metadata & Identifiers: IPs, Ports, and Flow IDs (to prevent data leakage and overfitting).

Inconsistent Records: Features known to be inconsistently recorded by the CICFlowMeter tool.

Zero-Variance Flags: Columns with constant 0 values across the entire dataset.

2. Handling Infinite & Missing Values
Infinity Fixing: Columns highly prone to division-by-zero errors (Flow Packets/s and Flow Bytes/s) had their inf values critically replaced with the median of their respective finite values.

Log Transformation: Applied np.log1p() to highly skewed numeric columns to compress extreme values and normalize the distribution.

Scrubbing: All remaining NaN, Null, and duplicated rows were purged, resulting in a perfectly clean final dataset of 423,722 rows.

⚙️ Feature Engineering & Pipeline
Following an extensive Exploratory Data Analysis (EDA) and Train/Test split, the data was routed through a rigorous transformation pipeline:

1. Class Imbalance Handling
Network traffic is inherently imbalanced. To prevent the model from becoming biased toward the most frequent attacks, Weighted Random Sampling was implemented during the training phase to ensure minority attack classes were properly represented.

2. Categorical vs. Numerical Routing
Features were automatically separated based on their unique value counts. Features with a low cardinality (e.g., Protocol with primarily 2 or 3 unique states, and TCP Flags) were routed to the categorical pipeline.

3. Transformation & Dimensionality Reduction
Categorical: Applied One-Hot Encoding to prevent the model from assuming false ordinal relationships between network flags/protocols.

Numerical: Applied Standard Scaling to normalize distributions, followed immediately by Principal Component Analysis (PCA). PCA successfully compressed the 58 numerical features down to 22 principal components, retaining maximum variance while drastically reducing computational overhead.

Target Variable: Applied Label Encoding to map the 13 attack names to integers for the neural network's softmax output.

🧠 Deep Learning Models Evaluated
This project strictly focused on Deep Learning approaches. Five distinct architectures were trained, tuned, and evaluated against the heavily engineered dataset:

Multi-Layer Perceptron (MLP)

Long Short-Term Memory (LSTM)

Transformer Architecture

Standard 1D Convolutional Neural Network (1D CNN)

1D CNN + Inception Module (🏆 Best Performing)

The Winning Architecture: 1D CNN + Inception
The highest performance was achieved by augmenting a 1D CNN with an Inception module. By utilizing multiple filter sizes simultaneously in parallel convolution branches, the model was able to capture both local spatial patterns and broader, complex correlations within the PCA-reduced network traffic features, yielding a final test accuracy of 97.12%.

📈 Model Performance & Results
(Add your images here by replacing the file paths below)

Accuracy & Loss Curves
Classification Report
🚀 How to Run Locally
1. Clone the repository
Bash

git clone https://github.com/yourusername/NIDS_CNN.git
cd NIDS_CNN
2. Install dependencies
Bash

pip install -r requirements.txt
3. Usage
Due to size constraints, the raw CIC-DDoS dataset is not included. Ensure you download the raw CSVs from Kaggle, then run the preprocessing scripts in order before initiating model training.
