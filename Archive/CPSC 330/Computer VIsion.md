# Computer VIsion
- computational interpretation of images/videos
	- using ML/AI
- commonly uses [[Neural Networks]]
- usage
	- image classification: is this a cat or a dog?
	- object localization: where is the cat in this image?
	- object detection: What are the various objects in the image? 
	- instance segmentation: What are the shapes of these various objects in the image? 
- Usually structured data  doesn't come in CSV files. 
	- Also, if you are working on image datasets in the real world, they are going to be huge datasets and you do not want to load the full dataset at once in the memory. 
	- So usually you **work on small batches**.
- how can we train traditional ML models designed for tabular data on image data?
	- flatten the images
		- bad since there is structure in the data
		- flattening will lose useful information
	- [[Convolutional Neural Networks (CNN)]] don't have to flatten them
```python
# This code flattens the images in train and validation sets.
# Again you're not expected to understand all the code.
flatten_transforms = transforms.Compose([
                    transforms.Resize((IMAGE_SIZE, IMAGE_SIZE)),
                    transforms.Grayscale(num_output_channels=1),
                    transforms.ToTensor(),
                    transforms.Lambda(torch.flatten)])
train_flatten = torchvision.datasets.ImageFolder(root='../data/animal_faces/train', transform=flatten_transforms)
valid_flatten = torchvision.datasets.ImageFolder(root='../data/animal_faces/valid', transform=flatten_transforms)                                                
```
