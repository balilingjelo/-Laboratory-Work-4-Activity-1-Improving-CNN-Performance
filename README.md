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

EpochTrain AccuracyVal AccuracyVal Loss10.73360.85670.497340.75980.88350.466550.76560.88830.3870 ← best60.77230.86150.501790.77820.83480.5737100.78990.85290.5658
