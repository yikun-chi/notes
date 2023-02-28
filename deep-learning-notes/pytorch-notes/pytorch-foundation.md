# PyTorch Foundation

## Dataset and Dataloader Object Construction

Dataset Object:

* \_\_init\_\_: object construction
  * transform transform function transform the feature
  * target\_transform function transform the label
* \_\_len\_\_: return the length
* \_\_getitem\_\_: takes in an idex, return image and label&#x20;

Dataloader:&#x20;

* Iterate through dataset in mini-batches&#x20;
* First dimension is the batch size

{% code overflow="wrap" %}
```python
# Source: PyTorch Official Tutorial 
# https://pytorch.org/tutorials/beginner/basics/data_tutorial.html#iterating-and-visualizing-the-dataset
import os
import pandas as pd
from torchvision.io import read_image

# Custom Dataset Construction
class CustomImageDataset(Dataset):
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file)
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform

    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = read_image(img_path)i
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
        
   
# Iterate Dataset through dataloader      
from torch.utils.data import DataLoader
train_dataloader = DataLoader(training_data, batch_size=64, shuffle=True)
test_dataloader = DataLoader(test_data, batch_size=64, shuffle=True)

train_features, train_labels = next(iter(train_dataloader))
# train_features first dimension is batch size 
img = train_features[0].squeeze()
label = train_labels[0]
plt.imshow(img, cmap="gray")
plt.show()


# Lambda function example of create one-hot encoding label 
ds = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor(),
    target_transform=Lambda(lambda y: torch.zeros(10, dtype=torch.float).scatter_(0, torch.tensor(y), value=1))
)
```
{% endcode %}

## Building Neural Network&#x20;

```python
class NeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10),
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits

device = "cuda" if torch.cuda.is_available() else "cpu"

X = torch.rand(1, 28, 28, device=device)
logits = model(X)
pred_probab = nn.Softmax(dim=1)(logits)
y_pred = pred_probab.argmax(1)
print(f"Predicted class: {y_pred}")

# see parameters
print(f"Model structure: {model}\n\n")

for name, param in model.named_parameters():
    print(f"Layer: {name} | Size: {param.size()} | Values : {param[:2]} \n")
```

* inherit parent class&#x20;
* forward specify how data is passed through all the layers.&#x20;

## Differentiation through autograd&#x20;

```python
import torch

x = torch.ones(5)  # input tensor
y = torch.zeros(3)  # expected output
w = torch.randn(5, 3, requires_grad=True)
b = torch.randn(3, requires_grad=True)

# forward computation 
z = torch.matmul(x, w)+b
loss = torch.nn.functional.binary_cross_entropy_with_logits(z, y)

# backward computation 
loss.backward()
print(w.grad)
print(b.grad)



# Disable gradient tracking 
with torch.no_grad():
    z = torch.matmul(x, w)+b
print(z.requires_grad)

# Alternative method for gradient tracking 
z = torch.matmul(x, w)+b
z_det = z.detach()
```

* Gradient is only available if _requires\_grad_ property is set to true.&#x20;
* Gradient calculation is only done once using _backward_
* If several _backward_ call is needed, we need to pass _retain\_graph = True_ to the _backward_ call
* _torch.no\_grad()_ disable gradient tracking or _detach,_ we can use them to mark parameters in neural network as frozen parameters

## Training Loop

```python
# Initialize the loss function and optimizer
loss_fn = nn.CrossEntropyLoss()
# optimizer need to specify what are the parameters need to be optimized 
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)

def train_loop(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    for batch, (X, y) in enumerate(dataloader):
        # Compute prediction and loss
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        if batch % 100 == 0:
            loss, current = loss.item(), (batch + 1) * len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")
```

* Optimizer input includes the parameters needing optimized&#x20;
* At each bach, predict, calculate loss, clean optimizer, calculate gradient, and then step.&#x20;

```python
def test_loop(dataloader, model, loss_fn):
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    test_loss, correct = 0, 0

    with torch.no_grad():
        for X, y in dataloader:
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()

    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")
```

* Iterate through each set of data to calculate loss without tracking gradient.&#x20;

```python
epochs = 10
for t in range(epochs):
    print(f"Epoch {t+1}\n-------------------------------")
    train_loop(train_dataloader, model, loss_fn, optimizer)
    test_loop(test_dataloader, model, loss_fn)
print("Done!")


# Saving entire model 
torch.save(model, 'model.pth')
model = torch.load('model.pth')

# Saving only parameters
model = models.vgg16(pretrained=True)
torch.save(model.state_dict(), 'model_weights.pth')
model = models.vgg16() # we do not specify pretrained=True, i.e. do not load default weights
model.load_state_dict(torch.load('model_weights.pth'))
model.eval() # call model.eval to set dropout and batch normalization layer 
```
