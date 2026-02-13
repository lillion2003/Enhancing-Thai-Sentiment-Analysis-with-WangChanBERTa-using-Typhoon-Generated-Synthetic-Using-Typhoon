# Enhancing Thai Sentiment Analysis with WangChanBERTa using Typhoon-Generated Synthetic Data

**Author**: Github: lillion2003

This content presents the process of synthesizing data using LLM (Llama 3.1-Typhoon 2) and compares different data synthesis approaches to determine which method most effectively improves sentiment classification performance when fine-tuning the WangChanBERTa model.

## Table of Contents

- [Problem](#problem)
- [Objective](#objective)
- [Data](#data)
- [Methods (Principles and Analysis Methods)](#methods-principles-and-analysis-methods)
- [Tools](#tools)
- [Experiment 1: Determining Optimal Prompt Method Synthesis Ratio](#experiment-1-determining-optimal-prompt-method-synthesis-ratio)
- [Experiment 2: Comparing Data Synthesis Approaches](#experiment-2-comparing-data-synthesis-approaches)
- [Results](#results)
- [Key Findings](#key-findings)
- [Discussion](#discussion)
- [Recommendations & Next Steps](#recommendations--next-steps)
- [Project Structure](#project-structure)

### Problem

Sentiment analysis models play a crucial role in understanding customer opinions and feedback, enabling businesses to make data-driven decisions, improve service quality, and enhance customer satisfaction. However, in sentiment analysis tasks, restaurant review datasets often suffer from class imbalance, where negative reviews are significantly underrepresented. This imbalance can reduce the effectiveness of model training, as the classifier may become biased toward the majority class, leading to poor prediction performance on minority classes.

Recent studies suggest that large language models (LLM) can be used to generate synthetic textual data to address such limitations. Therefore, this study investigates multiple text synthesis approaches for generating additional restaurant reviews. The objective is to compare different synthetic data generation strategies and evaluate which approach most effectively enhances the performance of a sentiment classification model in the context of Thai language analysis.

### Objective

This research aims to investigate the effectiveness of synthetic data augmentation for improving sentiment classification performance on Thai restaurant reviews. Specifically, the study seeks to:

1. Determine the optimal synthesis ratio for the Prompt-based method (ranging from 1x to 4x the original data size)
2. Compare the performance of different synthesis approaches (Rewriting, Prompt, and Mixed methods) to identify which strategy yields the best classification results when fine-tuning WangChanBERTa

The experiments utilize state-of-the-art LLMs (Llama 3.1-Typhoon 2) for data synthesis due to their strong text generation capabilities, while leveraging WangChanBERTa as the classification model given its proven effectiveness in Thai language processing tasks.

### Data

The data comes from the Wongnai Restaurant Review Dataset ( HuggingFace).
The dataset contains review text and star ratings from 19,958 restaurant reviews.

### Methods

This project uses the following methods:

**1. Data Preparation (Text Synthesis)** → See [Data Synthesis using Typhoon](Data%20Synthesis%20using%20Typhoon/) folder

Removes 3-star reviews (neutral sentiment) and keeps only clear positive (4-5 stars) and negative (1-2 stars) reviews.

**Synthesis Models:** Llama 3.1-Typhoon 2 are used for generating synthetic reviews. These models were selected because:

- Modern LLM demonstrate exceptional text generation capabilities, producing coherent and contextually appropriate content
- Typhoon 2 is specifically optimized for Thai language tasks, making it particularly suitable for generating Thai restaurant reviews

**Rewriting Method:** The model receives a prompt containing an example review and generates a new version by rewriting the original text while preserving the same sentiment. This method applies a fixed synthesis ratio of 4x (four times the original data size), as adopted from relevant research on synthetic data augmentation.

**Prompt Method:** The model receives only sentiment-based instructions without any example reviews and generates entirely new reviews from scratch according to the specified sentiment. The optimal synthesis ratio for this method is determined through Experiment 1
, testing ratios from 1x to 4x to systematically investigate the implementation of the Prompt Method for data synthesis.

**Mixed Method:** The synthetic dataset is created by integrating rewritten reviews and fully prompt-generated reviews into a single augmented dataset. This method combines the 4x Rewritten data with the optimal Prompt synthesis ratio identified in [Experiment 1](#experiment-1-determining-optimal-prompt-method-synthesis-ratio).

**Classification Model:** WangChanBERTa is used as the sentiment classifier. This model was chosen for its strong performance on Thai language understanding tasks, making it well-suited for analyzing Thai restaurant reviews.

### Tools

- **Programming Language:** Python
- **Libraries:**
  - **Transformers (Hugging Face):** Used to load and fine-tune the **WangChanBERTa** model for sentiment analysis and to utilize **Llama 3.1-Typhoon 2** for synthetic data generation.
  - **PyTorch:** The deep learning framework powering both the classification and generation models.
  - **Pandas:** Used for data manipulation and preprocessing the review datasets.
  - **Scikit-learn:** Employed for data splitting (train/test/validation), calculating evaluation metrics (F1-score, Precision, Recall), and computing class weights.
  - **Datasets (Hugging Face):** Used to load the Wongnai Review (2025) dataset.
  - **Matplotlib / Seaborn:** Used for visualizing confusion matrices.

### Experiment 1: Determining Optimal Prompt Method Synthesis Ratio

→ See [Find best Synthesis per Real ratio for Prompt-based](WangchanBERTa%20Fine-Tuning/Find%20best%20Synthesis%20per%20Real%20ratio%20for%20Prompt-based/) folder

This experiment identifies the optimal synthesis ratio for the Prompt-based method by testing different augmentation levels:

- Test synthesis ratios: 1x, 2x, 3x, and 4x the original dataset size
- Each ratio is evaluated by fine-tuning WangChanBERTa and measuring classification performance
- The best-performing ratio is selected for use in [Experiment 2](#experiment-2-comparing-data-synthesis-approaches)

### Experiment 2: Comparing Data Synthesis Approaches

→ See [Comparison Performance of each method](WangchanBERTa%20Fine-Tuning/Comparison%20Perfomance%20of%20each%20method/) folder

This experiment compares four different data configurations for training the sentiment classification model:

- **Raw Data Only:** Training is performed using only the original review dataset.

- **Rewritten Augmentation:** Training is performed using the original reviews combined with synthetic reviews generated through the rewriting method (4x synthesis ratio based on prior research).

- **Prompt Augmentation:** Training is performed using the original reviews combined with synthetic reviews generated through the prompt-based method (using the optimal ratio identified in Experiment 1).

- **Mixed Augmentation:** Training is performed using the original reviews combined with synthetic reviews generated from both the rewriting and prompt-based methods (4x Rewritten data + optimal Prompt ratio from Experiment 1).

### Results

See detailed performance metrics and comparison in [Results Comparison](Results%20Comparison.md).

### Limitation

The Rewriting Augmentation method uses a fixed 4x synthesis ratio based on previous studies. However, prior research does not clearly state what ratio should be used for the Prompt Method. Therefore, the Prompt Method was tested to find the best ratio before comparison. This may make the comparison less fair, since only the Prompt Method was optimized. However, this study mainly focuses on comparing the overall performance of each method, with special attention to the Prompt Method, as this study aims to examine how effectively a large language model (LLM) can synthesize Thai reviews using prompts alone, without being provided with any example reviews.

### Key Findings

- Only the **Prompt Method** and **Mixed Method** successfully improve model performance (Macro Average F1-Score) compared to using raw data only.

- The **Mixed Method** achieves the best overall performance, with Macro Average F1-Score increasing from 88.11% to 90.99%, and negative class F1-Score improving from 77.66% to 83.01%.

### Discussion

**Prompt Method:** The Prompt-based approach generates entirely new reviews without relying on existing examples, producing diverse text that resembles real reviews while introducing novel patterns and expressions. This diversity helps the WangChanBERTa model learn new linguistic features and generalize better to unseen data. Notably, this method significantly improves the prediction performance for the negative class (F1-score: 77.66% → 79.77%), addressing the class imbalance problem effectively.

**Rewritten Method:** In contrast, the Rewritten Method performs slightly worse than the baseline (Macro F1-score: 88.00% vs. 88.11%). Rewritten reviews tend to preserve similar structures and vocabulary to the original texts, providing limited additional diversity for the model to learn new patterns. As a result, this method does not improve performance compared to the baseline, particularly for the negative class (F1-score: 77.45% vs. 77.66% at baseline).

**Mixed Method:** The Mixed Method produces the most interesting results, achieving the highest performance (Macro F1-score: 90.98%). This approach appears to strike an optimal balance between diversity (from Prompt-generated reviews) and similarity to the original data distribution (from Rewritten reviews). This balanced approach enables WangChanBERTa to achieve the highest F1-score in experiment 2, with particularly strong improvements in negative class prediction (F1-score: 77.66% → 83.01%).

### Recommendations & Next Steps

- The trained model can be deployed to classify new restaurant reviews into positive or negative sentiment, and the same approach can be extended to other domains with similar sentiment analysis tasks.
- Future work should explore identifying the optimal synthesis ratio for the Rewritten Method to achieve a more comprehensive comparison of augmentation effectiveness across methods.


### Project Structure

```
├── README.md                                    # Project overview and documentation
├── Results Compairison.md                       # Detailed performance metrics and comparison
│
├── Data Synthesis using Typhoon/                # Synthetic data generation notebooks
│   ├── rewritten method.ipynb                   # Rewriting-based synthesis (4x ratio)
│   ├── new sample method.ipynb                  # Prompt-based synthesis (no examples)
│   └── mixed method.ipynb                       # Combined synthesis approach
│
├── WangchanBERTa Fine-Tuning/                   # Model training experiments
│   ├── Find best Synthesis per Real ratio for Prompt-based/
│   │   ├── fine tune newsample1X.ipynb          # Experiment 1: 1x ratio
│   │   ├── fine tune newsample2X.ipynb          # Experiment 1: 2x ratio
│   │   ├── fine tune newsample3X.ipynb          # Experiment 1: 3x ratio
│   │   └── fine tune newsample4X.ipynb          # Experiment 1: 4x ratio
│   │
│   └── Comparison Perfomance of each method/
│       ├── fine tune raw.ipynb                  # Experiment 2: Raw data baseline
│       ├── fine tune rewrite.ipynb              # Experiment 2: Rewritten augmentation
│       ├── fine tune newsample3X.ipynb          # Experiment 2: Prompt augmentation (best ratio)
│       └── fine tune mixed.ipynb                # Experiment 2: Mixed augmentation
│
├── dataset/                                     # Original and processed datasets
└── training set with synthetic data/            # Generated synthetic training data
```
