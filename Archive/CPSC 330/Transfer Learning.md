# Transfer Learning
- fine-tuning a pre-trained [[Neural Networks]] for your specific need
- common in [[Archive/CPSC 330/Computer VIsion]] and [[Natural Language Processing]]
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
```python
def get_features(model, train_loader, valid_loader):
    """Extract output of squeezenet model"""
    with torch.no_grad():  # turn off computational graph stuff
        Z_train = torch.empty((0, 1024))  # Initialize empty tensors
        y_train = torch.empty((0))
        Z_valid = torch.empty((0, 1024))
        y_valid = torch.empty((0))
        for X, y in train_loader:
            Z_train = torch.cat((Z_train, model(X)), dim=0)
            y_train = torch.cat((y_train, y))
        for X, y in valid_loader:
            Z_valid = torch.cat((Z_valid, model(X)), dim=0)
            y_valid = torch.cat((y_valid, y))
    return Z_train.detach(), y_train.detach(), Z_valid.detach(), y_valid.detach()
densenet = models.densenet121(weights="DenseNet121_Weights.IMAGENET1K_V1")
densenet.classifier = nn.Identity()  # remove that last "classification" layer
Z_train, y_train, Z_valid, y_valid = get_features(
    densenet, dataloaders["train"], dataloaders["valid"]
)
pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000))
pipe.fit(Z_train, y_train)
pipe.score(Z_train, y_train)
pipe.score(Z_valid, y_valid)
```
