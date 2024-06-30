Relation of input and output dimensions of the convolutional layers of Pytorch![[Pasted image 20240306230015.png]]![[Pasted image 20240306122108.png]]This is the model summary of the LeMet model.
The following part discusses convolutional layers and models in Pytorch.
``` Python

class LeNet(nn.Module):
    def __init__(self):
        super(LeNet, self).__init__()
        self.conv1 = nn.Conv2d(1, 6, kernel_size=5)
        self.relu1 = nn.ReLU()
        self.avgpool1 = nn.AvgPool2d(2, stride=2)
        self.conv2 = nn.Conv2d(6, 16, kernel_size=5)
        self.relu2 = nn.ReLU()
        self.avgpool2 = nn.AvgPool2d(2, stride=2)
        self.conv3 = nn.Conv2d(16, 120, kernel_size=5)
        self.relu3 = nn.ReLU()
        self.fc2 = nn.Linear(120, 84)
        self.relu4 = nn.ReLU()
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.avgpool1(self.relu1(self.conv1(x)))
        x = self.avgpool2(self.relu2(self.conv2(x)))
        x = self.relu3(self.conv3(x)).reshape(x.size()[0], -1)
        x = self.relu4(self.fc1(x))
        x = self.fc2(x)
        return x
```