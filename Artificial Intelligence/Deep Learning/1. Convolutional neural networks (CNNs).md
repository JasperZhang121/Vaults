
It is a type of neural network that are especially well-suited for image recognition and other problems where the input has a spatial structure. They use specific architectures and connection patterns to learn features at different scales and locations in the input.

- **Convolution operation:**

In a convolutional layer, the convolution operation is used to <mark style="background: #FFB8EBA6;">apply a set of filters to the input image</mark>. This operation can be represented mathematically as follows:

$$\begin{equation} S(i,j) = (I * K)(i,j) = \sum_{m}\sum_{n}I(m,n)K(i-m,j-n) \end{equation}$$

where $S(i,j)$ is the output of the convolution operation at position $(i,j)$, I is the input image, K is the filter/kernel, and * denotes the convolution operation.

Similar to the process of sliding a small square horizontally and vertically, performing calculations to create a new square while also implementing zero padding to prevent the square from decreasing in size as shown in the image below:

![[convolutional_Process.jpg]]

- **Non-linearity:**

Rectified Linear Unit (ReLU) activation:

ReLU is a popular activation function used in CNNs. It is defined as follows:

$$\begin{equation} f(x) = \max(0,x) \end{equation}$$

where x is the input to the activation function.

Softmax activation:

Softmax is commonly used as the activation function for the output layer in classification problems. It maps the output of the last hidden layer to a probability distribution over the classes. Mathematically, softmax can be defined as follows:

$$\begin{equation} P(y=j|x) = \frac{\exp(z_j)}{\sum_{k=1}^{K}\exp(z_k)} \end{equation}$$

where $P(y=j|x)$ is the probability of the j-th class given the input x, z_j is the j-th element of the output vector z of the last hidden layer, and K is the number of classes.

-   **Pooling operation:**

The pooling operation is used to downsample the feature maps obtained from the convolutional layer. The most common pooling operation is <mark style="background: #BBFABBA6;">max pooling</mark>, which takes the maximum value in each pooling region. Mathematically, max pooling can be defined as follows:

$$\begin{equation} M(i,j) = \max_{(m,n)\in R_{i,j}}S(m,n) \end{equation}$$

where M(i,j) is the output of the pooling operation at position (i,j), R_{i,j} is the pooling region centered at (i,j), and S is the input feature map.

-   **Fully Connected Layers:**

After the feature maps are extracted through convolution, passed through activation functions, and downsampled through pooling, the output is flattened and passed through one or more fully connected layers to generate the final output. These layers are similar to the ones used in traditional neural networks and act as a classifier that uses the learned features to classify or identify the input image. The output of each fully connected layer is calculated as follows:

$$z = Wx + b$$

where $W$ is the weight matrix, $x$ is the input vector, $b$ is the bias vector, and $z$ is the output vector.

The output of the last fully connected layer is passed through a softmax activation function to produce a probability distribution over the classes. Mathematically, softmax can be defined as follows:

$$P(y=j|x) = \frac{\exp(z_j)}{\sum_{k=1}^{K}\exp(z_k)}$$

-   **Backpropagation:**

Once the output is generated, the network's weights and biases are updated using backpropagation to minimize the difference between predicted and actual outputs. This involves calculating the gradient of the loss function with respect to the model's parameters, and <mark style="background: #FFB86CA6;">updating these parameters in the opposite direction of the gradient</mark>. This process is repeated over several iterations until the model converges. The gradients of the loss function with respect to the output of the last layer can be calculated as follows:

$$\delta_{i}^{(L)} = y_{i} - t_{i}$$

where $\delta_{i}^{(L)}$ is the error for the i-th output neuron, $y_{i}$ is the predicted output, and $t_{i}$ is the true output.

Backpropagation is an <mark style="background: #ABF7F7A6;">iterative algorithm that works by propagating the error back through the layers of the network</mark>. The error is calculated as the difference between the predicted and actual outputs, and is backpropagated through the layers to update the weights and biases. This process is computationally expensive and can be improved through techniques such as momentum and adaptive learning rates.

The process of backpropagation enables the network to learn from its mistakes and improve its performance over time. The goal of backpropagation is to minimize the loss function, which measures the difference between the predicted and actual outputs. Once the loss function is minimized, the network is considered to have converged, and it can be used for making predictions on new data.

---

### Short Summary on Convolutional Neural Networks:

1.  Training Algorithm:

-   Same as general machine learning algorithms, define a loss function to measure the difference between predicted and actual results.
-   Find the values of W and b that minimize the loss function. The algorithm used in CNN is <mark style="background: #FFB8EBA6;">SGD (Stochastic Gradient Descent)</mark>.

2.  Advantages and Disadvantages:

-   Advantages:
    -   Shared convolutional kernel for processing high-dimensional data without pressure.
    -   No need to manually select features. With trained weights, good feature classification results can be obtained.
-   Disadvantages:
    -   Tuning is required, requiring a large sample size. Training is best done on a GPU.
    -   The physical meaning is unclear. In other words, we do not know what features each convolutional layer extracts, and neural networks themselves are a difficult to explain "black box" model.

3.  Typical CNN:

-   LeNet, the earliest CNN used for digit recognition.
-   AlexNet, CNN that far exceeded second place in the 2012 ILSVRC competition. Deeper than LeNet, using multiple small convolutional layers stacked instead of a single large convolutional layer.
-   ZF Net, the champion of the 2013 ILSVRC competition.
-   GoogLeNet, the champion of the 2014 ILSVRC competition.
-   VGGNet, the model in the 2014 ILSVRC competition. The image recognition performance is slightly worse than GoogLeNet, but it has excellent performance in many image transformation learning problems (such as object detection).

4.  Fine-tuning:

-   Fine-tuning is using the weights or partial weights of a pre-trained model that has been used for other objectives as the initial values for training. Why not <mark style="background: #FFF3A3A6;">randomly</mark> select a few numbers as the initial weight values? 
	- First, training a convolutional neural network from scratch is prone to problems. 
	- Second, fine-tuning can quickly converge to a relatively ideal state, saving time and effort.
-   The specific method of fine-tuning is as follows:
    -   Reuse the weights of the same layer, define a new layer, and set the initial weights to random values.
    -   Increase the learning rate of the newly defined layer and decrease the learning rate of the reused layer.

5.  Common Frameworks:

-   Caffe: A mainstream CV toolkit originating from Berkeley, supporting C++, Python, and Matlab. The Model Zoo contains many pre-trained models for use.
-   PyTorch: A convolutional neural network toolkit used by Facebook. It is very intuitive to use through local interfaces of temporal convolution. Defining new network layers is simple.
-   TensorFlow: A deep learning framework developed by Google. TensorBoard visualization is very convenient. Data and model parallelization are good, and the speed is fast.

6.  In All:

-   Convolutional networks are essentially a mapping from input to output. They can learn a large number of mappings between inputs and outputs without any precise mathematical expressions between inputs and outputs, as long as they are trained with known patterns.
-   A very important feature of CNN is that it is <mark style="background: #ABF7F7A6;">top-heavy</mark> (weights get smaller as inputs get closer, and there are more weights as outputs get closer), showing an inverted triangle shape, which avoids the problem of gradient loss during backpropagation in BP neural networks.
-   Convolutional neural networks are mainly used to recognize two-dimensional graphics with displacement, scaling, and other forms of distortion invariance.

---

### Example in Tensorflow

```python
"""
Define a CNN with two convolutional layers, two max pooling layers, and two fully connected layers. The output layer has a single neuron with sigmoid activation, making it suitable for binary classification tasks. The model is compiled with binary crossentropy loss and Adam optimizer and trained on training data with batch size of 32 and for 10 epochs, while validating on validation data.
"""

from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Flatten, Dense

# Initialize the model
model = Sequential()

# Add a convolutional layer with 32 filters, a 3x3 kernel size, and ReLU activation
model.add(Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)))

# Add a max pooling layer with a pool size of 2x2
model.add(MaxPooling2D(pool_size=(2, 2)))

# Add another convolutional layer with 64 filters, a 3x3 kernel size, and ReLU activation
model.add(Conv2D(64, (3, 3), activation='relu'))

# Add another max pooling layer with a pool size of 2x2
model.add(MaxPooling2D(pool_size=(2, 2)))

# Flatten the output of the previous layer
model.add(Flatten())

# Add a fully connected layer with 128 neurons and ReLU activation
model.add(Dense(128, activation='relu'))

# Add an output layer with a single neuron and sigmoid activation
model.add(Dense(1, activation='sigmoid'))

# Compile the model with binary crossentropy loss and Adam optimizer
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# Train the model on training data and validate on validation data
model.fit(x_train, y_train, batch_size=32, epochs=10, validation_data=(x_val, y_val))
```

### Example in Pytroch:

```python
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

# Set random seed for reproducibility
torch.manual_seed(0)

# Define the CNN architecture
class SimpleCNN(nn.Module):
    def __init__(self):
        super(SimpleCNN, self).__init__()
        self.conv1 = nn.Conv2d(3, 16, kernel_size=3, stride=1, padding=1)
        self.relu = nn.ReLU()
        self.pool = nn.MaxPool2d(kernel_size=2, stride=2)
        self.fc = nn.Linear(16 * 16 * 16, 10)
    
    def forward(self, x):
        x = self.conv1(x)
        x = self.relu(x)
        x = self.pool(x)
        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x

# Create an instance of the CNN
model = SimpleCNN()

# Define the loss function and optimizer
criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

# Load the data
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
])
trainset = torchvision.datasets.CIFAR10(root='./data', train=True, download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=4, shuffle=True, num_workers=2)

# Train the model
for epoch in range(5):
    running_loss = 0.0
    for i, data in enumerate(trainloader, 0):
        inputs, labels = data
        optimizer.zero_grad()
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()
        running_loss += loss.item()
        if i % 2000 == 1999:
            print(f'Epoch: {epoch+1}, Batch: {i+1}, Loss: {running_loss/2000:.3f}')
            running_loss = 0.0

print('Training finished.')

# Save the trained model
torch.save(model.state_dict(), 'cnn_model.pth')
```

