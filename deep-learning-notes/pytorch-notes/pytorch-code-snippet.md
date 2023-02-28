# Pytorch Code Snippet

## Visualization&#x20;

### Making Image Grid&#x20;

```python
def imshow(img):
    npimg = img.numpy() 
    plt.imshow(np.transpose(npimg, (1, 2, 0))) # Numpy is H x W x C 


# get some random training images
dataiter = iter(trainloader)
images, labels = next(dataiter)

# show images
imshow(torchvision.utils.make_grid(images))
```

## MISC

```python
# set seed
torch.manual_seed(1729)

```
