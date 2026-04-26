# Laboratory-Work-4-Activity-Improving-CNN-Performance Using Regularization

https://colab.research.google.com/drive/1NRu7SArP7O_AFbvwd8z6huRh8VGUSZRJ?usp=drive_link


# A. Model Evaluation Analysis

1. What were the weakest-performing classes based on the confusion matrix?
   
Model Evaluation – Weakest-Performing Classes
| Rank | Class | Correct Predictions | Est. Total Samples | Accuracy | Main Misclassifications | Severity |
|------|-------|--------------------|--------------------|----------|------------------------|----------|
| 1 | RoyalFern | 26 | ~50 | **52.0%** | 8 → ButtonFern, 5 → LadyFern | 🔴 Weakest |
| 2 | BrakenFern | 21 | ~32 | **65.6%** | 7 → BostonFern | 🔴 Weakest |
| 3 | AustralianTreeFern | 32 | ~55 | **58.2%** | 4 → ButtonFern, 4 → SilverLaceFern | 🟠 Weak |
| 4 | StaghornFern | 32 | ~45 | **71.1%** | Scattered across multiple classes | 🟠 Weak |
| 5 | JapanesePaintedFern | 38 | ~48 | **79.2%** | 3 → SilverLaceFern | 🟡 Moderate |
| 6 | BostonFern | 36 | ~46 | **78.3%** | 5 → BrakenFern | 🟡 Moderate |
| 7 | MaidenhairFern | 45 | ~56 | **80.4%** | Scattered | 🟡 Moderate |
------


2. How did Precision, Recall, and F1-score vary across classes?
   
Precision (how few false positives): HollyFern is top of the class at 1.00 - every sample it predicted was correct. CinnamonFern (0.63) is the worst - 37% of the samples it predicted were not actually the class it was predicting.
Recall (how few false negatives): LadyFern (0.97) and TropicalBrakeFern (0.97) are best at recall - they never missed a sample. BrakenFern is at 0.50; that is, half of its real samples were given incorrect labels.
F1-Score (harmonic average): HollyFern (0.96) and StaghornFern (0.94) come out on top. RoyalFern (0.62) and BrakenFern (0.64) are the woes, just like the confusion matrix.
The overall accuracy is 84% (macro avg F1 = 0.83), which is good for a 20-class prediction. We should be worried about the lowest 3-4 classes bringing down the macro average.
------

3. What does a low recall indicate in your model?

Recall is the number of true instances of a class that were identified. If recall is low, the model is not identifying the true positives - it spots a fern that's actually BrakenFern but decides it's not a fern which belongs to BrakenFern.

The poor recall in this model suggests that the model is not recognising the real fern samples - the model is not predicting the correct class for actual instances of a class, predicting them as another fern species.

BrakenFern and RoyalFern have a recall of 0.50 - the model was correctly predicting half as another fern. The model can't see the fern.


