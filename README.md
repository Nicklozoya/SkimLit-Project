# SkimLit: NLP for Medical Abstract Segmentation

## Overview
SkimLit is an NLP project aimed at simplifying the reading of medical abstracts by classifying sentences into sections (e.g., "OBJECTIVE", "METHODS", "RESULTS"). Inspired by the paper [*PubMed 200k RCT*](https://arxiv.org/abs/1612.05251), I replicated and adapted its approach using the PubMed 20k RCT dataset. My best model—a tribrid architecture combining token, character, and positional embeddings—achieved an accuracy of 82.95% and a top F1-score, demonstrating strong performance on this multi-class classification task.

## Problem
Medical abstracts are dense and time-consuming to skim. SkimLit automates the process of labeling abstract sections, making research more accessible to healthcare professionals and scientists.

## Approach
- **Dataset**: Preprocessed the PubMed 20k RCT dataset (180k train, 30k val, 30k test) with numbers replaced by "@" signs, structuring it into text, labels, and positional metadata (`line_number`, `total_lines`).
- **Experiments**: Built and compared 6 models:
  1. **Baseline**: TF-IDF + Naive Bayes.
  2. **Custom Token Embedding**: Conv1D with custom token embeddings.
  3. **Pretrained Token Embedding**: Universal Sentence Encoder (USE) from TensorFlow Hub.
  4. **Character Embedding**: Conv1D with custom character embeddings.
  5. **Hybrid**: Pretrained token + character embeddings with Bidirectional LSTM.
  6. **Tribrid**: Hybrid + positional embeddings (line number, total lines).
- **Best Model**: The tribrid model (Model 5) integrates USE token embeddings, character-level Bidirectional LSTM, and one-hot encoded positional features, concatenated into a dense output layer.

## Results
The tribrid model outperformed all others, balancing precision and recall effectively. Below is a comparison of all models based on validation set performance:
![Model_comparison](https://github.com/user-attachments/assets/8aa91f98-ce10-44cd-afec-e85e880d2993)


| Model                          | Accuracy | Precision | Recall | F1     |
|--------------------------------|----------|-----------|--------|--------|
| model_0_baseline              | 0.7226   | 0.7181    | 0.7226 | 0.7182 |
| model_1_custom_token_embedding| 0.7510   | 0.7490    | 0.7510 | 0.7490 |
| model_2_pretrained_token_embedding | 0.7860 | 0.7850 | 0.7860 | 0.7845 |
| model_3_custom_char_embedding | 0.7320   | 0.7300    | 0.7320 | 0.7295 |
| model_4_hybrid+char_token_embedding | 0.8100 | 0.8080 | 0.8100 | 0.8075 |
| model_5_pos_char_token_embedding | **0.8295** | 0.8200 | 0.8150 | **0.8175** |

*Note*: Model 5’s F1-score of 0.8175 highlights its superior performance across the 5 classes (BACKGROUND, OBJECTIVE, METHODS, RESULTS, CONCLUSIONS).
