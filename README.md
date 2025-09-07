# Using Transfer Learning to Achieve 99% Accuracy in Brain Tumour Detection

This report outlines how transfer learning has been applied for brain tumour detection using medical imaging.

The dataset used can be found at: https://www.kaggle.com/preetviradiya/brian-tumor-dataset

## Dataset Exploration
We begin with a preliminary exploratory analysis to inform decisions about the model to be implemented.

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/count_data.png)

The dataset contains 2,087 labelled healthy cases and 2,513 tumour cases. This results in a ratio of 1:0.84, which indicates a reasonably balanced dataset.

As an example, below is a visualisation of 14 random images from the dataset.

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/plot_images.png)

## Model Selection and Training
The dataset was split as follows: 80% for training, 10% for validation (to determine when to stop training and avoid overfitting or underfitting), and 10% for testing the final model’s performance.

As accuracy is the primary requirement over computational efficiency, we selected the pre-trained NasNetLarge model, removing its final layer. This model takes an RGB input of 331x331 pixels and has a total of 206,885,319 parameters. We then added a dense layer of 250 neurons with a Dropout of 0.3 to reduce overfitting, followed by an output layer with a single neuron and a softmax activation function.

The transfer learning approach followed these steps:

Freeze the pre-trained layers, keeping the original weights intact. This ensures that the model retains its ability to recognise general patterns during the early stages of training.

Train only the newly added layers, enabling the model to specialise in the specific task of tumour detection.

Optionally unfreeze all layers and fine-tune the entire network if required.

In our case, step 3 was unnecessary, as it did not significantly improve performance. A total of 84,916,818 parameters were trained, representing a 60% reduction compared to the full NasNet model.

Training was conducted with a batch size of 40 and the Adam optimiser, which adjusts the learning rate as epochs progress. Each epoch required approximately 80 seconds using an Nvidia GTX 1080Ti.
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/train.PNG)

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/train_loss.PNG)

## Model Evaluation

The model was evaluated on the test dataset. To visualise performance, the confusion matrix was generated (0 = healthy, 1 = tumour).
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/confusion_matrix.png)

We observe a similar number of false positives and false negatives.

Finally, the most common metrics for classification tasks were calculated:
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/metrics.PNG)

##Other Considerations

As mentioned earlier, the decision was made not to unfreeze and train all layers of the model. In addition, various oversampling techniques were tested, such as rotations, mirroring, and Gaussian noise, with the aim of improving performance. However, these were excluded, as their cost-benefit ratio was negligible.

For a more comprehensive study, further analysis of model architecture and training strategies would be recommended.
