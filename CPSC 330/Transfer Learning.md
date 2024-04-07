# Transfer Learning
- fine-tuning a pre-trained [[Neural Networks]] for your specific need
- common in [[Computer VIsion]] and [[Natural Language Processing]]
- three common approaches
	1. Using pre-trained models **out-of-the-box**
    2. Using pre-trained models as **feature extractor** and training your own model with these features
    3. Starting with weights of pre-trained models and **fine-tuning the weights** for your task. 
## Out-of-the-box
- preprocess the data to fix the model's expected input format
- e.g. `vgg16` from `torchvision` was trained on `ImageNet` dataset
## Feature Extraction
- use the pre-trained model to extract features
- **pass our specific data** through pre-trained network to get a **feature vector for each example**
	- usually extracted from the last layer, before the classification lnayer
- train our own model using these feature vectors