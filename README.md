# Laboratory-Work-4-Activity-Improving-CNN-Performance Using Regularization

https://colab.research.google.com/drive/1NRu7SArP7O_AFbvwd8z6huRh8VGUSZRJ?usp=drive_link


# A. Model Evaluation Analysis

1. What were the weakest-performing classes based on the confusion matrix?
   
# Model Evaluation – Weakest-Performing Classes
| Rank | Class | Correct Predictions | Est. Total Samples | Accuracy | Main Misclassifications | Severity |
|------|-------|--------------------|--------------------|----------|------------------------|----------|
| 1 | RoyalFern | 26 | ~50 | **52.0%** | 8 → ButtonFern, 5 → LadyFern | 🔴 Weakest |
| 2 | BrakenFern | 21 | ~32 | **65.6%** | 7 → BostonFern | 🔴 Weakest |
| 3 | AustralianTreeFern | 32 | ~55 | **58.2%** | 4 → ButtonFern, 4 → SilverLaceFern | 🟠 Weak |
| 4 | StaghornFern | 32 | ~45 | **71.1%** | Scattered across multiple classes | 🟠 Weak |
| 5 | JapanesePaintedFern | 38 | ~48 | **79.2%** | 3 → SilverLaceFern | 🟡 Moderate |
| 6 | BostonFern | 36 | ~46 | **78.3%** | 5 → BrakenFern | 🟡 Moderate |
| 7 | MaidenhairFern | 45 | ~56 | **80.4%** | Scattered | 🟡 Moderate |

