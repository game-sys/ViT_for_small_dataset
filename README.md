
# How to train ViTs with small datasets?

## Literature Review

Please check the sheet link for the literature review already done and method proposed by researchers

https://docs.google.com/spreadsheets/d/1N2FHDpmy4WF_foRt4AFme9t88mO28W0dJv8siDDh2b8/edit?usp=sharing

## Overview

This project focuses on training ViT on small datasets

## Datasets

We will use the below-described datasets to do our initial experiments

| Dataset           | Train Images | Test Images | Classes | Approx. Train Images Per Class |
|-------------------|--------------|-------------|---------|--------------------------------|
| Cifar-10          | 50,000       | 10,000      | 10      | 5,000                          |
| Cifar-100         | 50,000       | 10,000      | 100     | 500                            |
| Oxford Flowers102 | 2,040        | 6,149       | 102     | 20                             |
| Food 101          | 25,250       | 75,750      | 101     | 750                            |
| Pet37             | 3,680        | 3,669       | 37      | 100                            |
| Sun397            | 80,129       | 19,850      | 397     | 100-461 (varies among classes) |


## Model Complexity

The below table describes the model complexity and resources required for the model 

| Sno. | Model           | Parameters (Millions) | Model Size (MB) | GFLOPs | Image Size | Inference Latency (ms) | GPU Memory Consumption for batch size 32 (MB) | Time to train 1 batch size of 32 (ms) |
|------|-----------------|-----------------------|-----------------|--------|------------|------------------------|--------------------------------------------|-------------------------------------|
| 1    | ResNet-18       | 11.68                 | 46.8            | 1.8    | 224        | 2.57                   | 2194                                       | 41                                  |
| 2    | ResNet-50       | 25.55                 | 102.5           | 4.1    | 224        | 5.47                   | 4624                                       | 123                                 |
| 3    | ResNet-101      | 44.54                 | 178.8           | 7.8    | 224        | 10.6                   | 6084                                       | 200                                 |
| 4    | ResNet-152      | 60.19                 | 241.7           | 11.6   | 224        | 14.4                   | 7866                                       | 285                                 |
| 5    | EfficientNet-B0 | 5.28                  | 21.5            | 0.41   | 224        | 7.51                   | 4254                                       | 74                                  |
| 6    | EfficientNet-B1 | 7.79                  | 31.6            | 0.61   | 224        | 10.27                  | 5398                                       | 105                                 |
| 7    | EfficientNet-B2 | 9.1                   | 36.9            | 0.7    | 224        | 13.49                  | 5570                                       | 111                                 |
| 8    | EfficientNet-B3 | 12.23                 | 49.5            | 1      | 224        | 11.39                  | 6876                                       | 143                                 |
| 9    | EfficientNet-B4 | 19.34                 | 78.2            | 1.5    | 224        | 14.34                  | 8760                                       | 195                                 |
| 17   | ViT-B/32        | 88.22                 | 353             | 2.9    | 224        | 5.8                    | 3714                                       | 112                                 |
| 19   | ViT-L/32        | 306.53                | 1226            | 10.2   | 224        | 6.3                    | 8774                                       | 350                                 |
| 20   | MobileNet-V2    | 3.5                   | 14.3            | 0.3    | 224        | 4.3                    | 3822                                       | 55                                  |
| 21   | MobileNet-V3-S  | 2.54                  | 10.3            | 0.06   | 224        | 4.1                    | 1718                                       | 21                                  |
| 22   | MobileNet-V3-L  | 5.48                  | 22.2            | 0.2    | 224        | 5                      | 2866                                       | 42                                  |




## Initial Experiments

Below are the initial experiments 

### Trained using ImageNet weights 

| Sno. | Model            | Flower-102 | Food-101 | Pets-37 | Sun397 |
| ---- | ---------------- | ---------- | -------- | ------- | ------ |
| 1    | ResNet18          | 90.09      | 80.76    | 88.57   | 86.58  |
| 2    | ResNet50          | 95.10      | 88.06    | 93.04   | 97.31  |
| 3    | ResNet101         | 96.16      | 89.27    | 93.13   | 99.30  |
| 4    | ViT/B/32          | 90.53      | 81.1     | 88.22   | -      |
| 5    | ViT/L/32          | 91.23      | 81.49    | 89.04   | -      |



### Initially training the ViT without ImageNet weights

| Sno. | Model         | Flower-102 | Food-101 | Pets-37 | Sun397 |
| ---- | ------------- | ---------- | -------- | ------- | ------ |
| 1    | ResNet18      | 47.22      | 70.40    | 41.23   | 77.76  |
| 2    | ResNet50      | 42.73      | 74.10    | 30.90   | 84.90  |
| 3    | ResNet101     | 43.63      | 76.59    | 32.70   | 88.95  |
| 4    | ViT/Ti/16     | 39.09      | 60.23    | 22.62   | 96.09  |
| 5    | ViT/S/16      | 51.50      | 60.83    | 22.75   | 99.97  |
| 6    | ViT/Ti/32     | 31.58      | 50.09    | 14.47   | 71.12  |
| 7    | ViT/S/32      | 36.28      | 50.74    | 16.27   | 96.86  |
| 8    | ViT/B/32      | 37.85      | 45.25    | 13.81   | 99.92  |
| 9    | ViT/L/32      | 38.83      | 38.83    | 9.1     | 99.96  |




## Future Experiments:

### Component Analysis Strategy

In this experiment, I aim to analyze the impact of different components within the Vision Transformer (ViT) architecture when training from scratch on small datasets. Understanding the role of individual components can help us optimize our model for specific tasks and datasets.

#### The components we will analyze are as follows:

1- Patch Embedding: This component is responsible for dividing the input image into patches and converting them into embedded representations.

2- Multi-Head Self-Attention: Multi-Head Self-Attention is a crucial component for capturing contextual information in images.

3- MLP (Multi-Layer Perceptron): The MLP component typically follows the self-attention layers and is responsible for processing and transforming the information captured by the self-attention mechanism.

4- Classification Layer: The classification layer is the final component that produces the model's output predictions.

#### Approach 1: Keeping All Components with Random Weights Except One with ImageNet:
In this approach, we initialize all components of the ViT with Random weights, except for one specific component, which is initialized with ImageNet weights.

#### Approach 2: Keeping All Components with ImageNet Weights Except One with Random:
In this approach, we initialize all components with ImageNet weights, except for one specific component, which is initialized with random weights.

#### Approach 3: Utilizing Pretrained Weights:
In this approach, I will use the trained weights instead of ImageNet. However, this time I will fix these weights (no training on certain components). Then apply Approach 1 and 2.

