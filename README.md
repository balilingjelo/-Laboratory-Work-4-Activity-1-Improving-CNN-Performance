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

The low recall in this model suggests that the model is not recognising the real fern samples - the model is not predicting the correct class for actual instances of a class, predicting them as another fern species.

BrakenFern and RoyalFern have a recall of 0.50 - the model was correctly predicting half as another fern. The model can't see the fern.

------

4. How does AUC score reflect model performance compared to accuracy?

The total area under the curve (AUC) was 0.9614 and the model's accuracy (ACC) was 0.84. They're good, just for different reasons.

* Accuracy (0.84) is the number of correct guesses divided by the total number of guesses. This means every mistake is weighted equally, and it's impacted by class imbalance: if there are more samples of one class, it overwhelmingly impacts the metric.

* AUC (0.9614) is a threshold-independent measure of how well it can separate classes. With 0.9614, our model places the true sample of a class higher than the non-sample 96% of the time. It is not affected by the classification threshold, and is less sensitive to class imbalance..

------

# B. Model Improvement

5. How did data augmentation affect validation accuracy?

RandomFlip, RandomRotation, RandomZoom, and RandomContrast data (or transforms) augmentation was applied as model's first layer to increase training images artificial variety each epoch. This was intended to help mitigate overfitting so the model would not learn the training images by heart (which leads to lower training- validation accuracy difference and better performance on validation). It was used alongside Dropout, BatchNormalization, and Early Stopping so the improvements were on top of each other, enhancing validation accuracy as a whole.

------

6. Why is Batch Normalization important in CNNs?

Batch Normalization normalizes the output of each layer during training, which speeds up convergence, prevents vanishing/exploding gradients, and reduces overfitting. In my model, it was placed after every Conv2D layer to keep activations stable as features passed through the network, allowing the model to train more efficiently across 20 fern classes.

------

7. What role did Dropout play in improving your model?

Dropout randomly switches off neurons to prevent over-fitting some features. In my model applied **Dropout(0.4)** after the convolutional layers and **Dropout(0.5)** before the final layer to avoid over-fitting and were able to generalise to new fern images.

------

8. How did Early Stopping prevent overfitting?

In your training logs, you can see that Early Stopping tracked val_loss and it stopped training when there was no improvement and restored the model's weight to its original state.

Looking at my epochs

| Epoch | Train Accuracy | Val Accuracy | Val Loss |
|---|---|---|---|
| 1 | 0.7336 | 0.8567 | 0.4973 |
| 4 | 0.7598 | 0.8835 | 0.4665 |
| 5 | 0.7656 | **0.8883** | **0.3870** ← best |
| 6 | 0.7723 | 0.8615 | 0.5017 |
| 9 | 0.7782 | 0.8348 | 0.5737 |
| 10 | 0.7899 | 0.8529 | 0.5658 |

**The validation loss began to grow after Epoch 5** (0.3870 → 0.5017 → 0.5737) while the training accuracy continued to improve - an indication that overfitting began. Early Stopping with `patience=3` would catch this overfitting and stop training before the model completely overfits, and would revert to best weights (from Epoch 5).

This stopped the model from capturing noise in the training data as its performance on the validation set dropped, instead of saving the final trained weights.

------

# C. Performance Comparison

Before vs. After Enhancements

| Metric | Original Model | Enhanced Model |
|--------|---------------|----------------|
| Best Val Accuracy | Lower (overfitting observed) | **0.8883** (Epoch 5) |
| Overfitting | High (train >> val accuracy) | Reduced — gap narrowed |
| Overall Accuracy | — | **0.84** |
| AUC Score | — | **0.9614** |
| Macro F1 | — | **0.83** |
| Weighted F1 | — | **0.84** |

Data Augmentation - prevented overfitting by introducing transformations (flips, rotations, zooms, and contrast changes) of the fern images, increasing the model's ability to generalize to new fern images.

BatchNormalization - normalized the training for the 3 Conv2D blocks, enabling quicker and more reliable convergence.

Dropout (0.4 and 0.5) - stopped the dense layers from overfitting the training data, thus boosting validation accuracy.

Smaller Learning Rate (0.0001) - enabled the model to make smaller adjustments to the weights, preventing overshooting the solution.

Early Stopping - stopped training at Epoch 5, where val_accuracy was highest (0.8883) and val_loss lowest (0.3870), avoiding overfitting.

------

10. Which enhancement contributed the most to performance improvement? Why?

The greatest impact came from **Data Augmentation** as it addressed the main issue - overfitting with too few training samples. It created different variations of each image via flipping, rotation, zooming, and contrast, so the model learned the common features of the images rather than memorizing the training images, which is why the model achieved a high validation accuracy of **0.8883** and AUC of **0.9614**.

------

11. Did the gap between training and validation accuracy decrease? Explain.

The gap between training and validation accuracy decreased after applying model enhancements.

| Epoch | Train Accuracy | Val Accuracy | Gap |
|-------|---------------|--------------|-----|
| 1/20  | 0.7336        | 0.8567       | +0.1231 |
| 2/20  | 0.7446        | 0.8415       | +0.0969 |
| 3/20  | 0.7510        | 0.8577       | +0.1067 |
| 4/20  | 0.7598        | 0.8835       | +0.1237 |
| 5/20  | 0.7656        | 0.8883       | +0.1227 |
| 6/20  | 0.7723        | 0.8615       | +0.0892 |
| 7/20  | 0.7832        | 0.8720       | +0.0888 |
| 8/20  | 0.7799        | 0.8787       | +0.0988 |
| 9/20  | 0.7782        | 0.8348       | +0.0566 |
| 10/20 | 0.7899        | 0.8529       | +0.0630 |

* Validation accuracy was always above training accuracy – an indication that the modifications were working

* The difference reduced from +0.1231 (+1 Epoch) to +0.0630 (+10 Epochs), implying increased stability

* Data Augmentation ensured that training accuracy was falsely low due to difficult training using augmented images

* Dropout (0.4 & 0.5) ensured the dense layers did not overfit, thus maintaining high validation accuracy

* The narrowing difference shows that the network was learning generalized features rather than overfitting training data


