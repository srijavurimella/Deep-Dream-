# Deep-Dream-
This a computer vision project which uses convolutional neural networks for creating dream like images.
!pip install torch_dreams
import numpy as np
import matplotlib.pyplot as plt
from torch_dreams import Dreamer
import torchvision.models as models
plt.rcParams['figure.figsize'] = 5,5
from torch_dreams import Dreamer
model = models.inception_v3(pretrained=True)
dreamy_boi = Dreamer(model, device='cpu', quiet=False)
image_param = dreamy_boi.render(
    layers = [model.Mixed_5b],
)
plt.imshow(image_param)
plt.show()
from PIL import Image
import matplotlib.pyplot as plt

num_iterations = 5

for i in range(num_iterations):
    image_param = dreamy_boi.render(
        layers=[model.Mixed_5b],
        width=512,
        height=256,
        iters=100,
        lr=9e-3,
        rotate_degrees=15,
        scale_max=1.2,
        scale_min=0.5,
        translate_x=100,
        translate_y=100,
        custom_func=None,
        weight_decay=1e-2,
        grad_clip=1,
    )

    plt.imshow(image_param)
    plt.title(f"Iteration {i+1}")
    plt.show()
layers_to_use = [model.Mixed_6b.branch1x1.conv]
def make_custom_func(layer_number = 0, channel_number= 0):
    def custom_func(layer_outputs):
        loss = layer_outputs[layer_number][:, channel_number].mean()
        return -loss
    return custom_func
my_custom_func = make_custom_func(layer_number= 0, channel_number = 119)
image_param = dreamy_boi.render(
    layers = layers_to_use,
    custom_func = my_custom_func,
    iters = 200
)
plt.imshow(image_param)
plt.show()
!wget -O flowers.jpg "https://kidlingoo.com/wp-content/uploads/flowers_name_in_english-980x510.jpg"
from torch_dreams.custom_image_param import CustomImageParam
my_custom_func = make_custom_func(layer_number = 0, channel_number = 19)
layers_to_use = [model.Mixed_6b.branch1x1.conv]
import matplotlib.pyplot as plt
import cv2
from torch_dreams.custom_image_param import CustomImageParam
original_image = cv2.cvtColor(cv2.imread('flowers.jpg'), cv2.COLOR_BGR2RGB)
n = 5
for i in range(n):
    image_param = dreamy_boi.render(
        image_parameter=param,
        layers=layers_to_use,
        lr=2e-4,
        iters=20,
        custom_func=my_custom_func
    )
    fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(10, 5))
    ax[0].imshow(original_image)
    ax[0].set_title("Original Image")
    ax[1].imshow(image_param)
    ax[1].set_title(f"Rendered Image - Iteration {i+1}")
    plt.show()

