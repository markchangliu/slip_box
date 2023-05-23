# Reference

**Title**: Deep Residual Learning for Image Recognition

**Research Team**: Microsoft Research

**Publish Date**: Dec 10, 2015

# Abstract

* Present a residual learning framework to ease the training of deep neural network. The residual networks are easier to optimize, and can gain accuracy from considerably increased depth.

* Reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions.

# Introduction

* The network depth is of crutial importance to promote performances.
  
* Learning better networks is not as easy as stacking more layers because of 2 arised problems:

  * **Vanishing/exploding gradients**: This has been largely addressed by normalized initialization and intermediate batch normalization layers.

  * **Degradation**: With the network depth increasing, accuracy gets saturated and then degrades rapidly. Such degradation is not caused by overfitting, since adding more layers leads to higher training error, as shown in Figure 1.

* Degradation indicates that not all systems are similarly easy to optimize. 

  * Consider a shallower architecture and its deeper counterpart that adds more layers onto it. There exists a solution by construction to the deeper model: the added layers are identidy mapping, and the other layers are copied from the learned shallower model. This constructed solution should produce no higher training error than its shallower counterpart. However, Figure 1 shows that our current solvers on hand are unable to find solutions that are comparably good or better than the constructed solution.

* The authors' team addresses the degradation problem by introducing a deep residual framework. 

  * The framework's structure is shown in Figure 2. This framework explicitly let stacked layers fit a residual mapping. More precisely, denoting the desired underlying mapping as $H(x)$, the authors let the stacked layers fit the mapping of $F(x):=H(x)-x$. 

* Based on comprehensive experiments, the research team show that

  * The deep residual nets are easier to optimize than the original, unreferenced "plain" nets. That is, the deep residual nets exhibit lower training error than the counterpart "plain" nets.

  * The deep residual nets can easily gain accuracy improvements from greatly increased depth.

<br>

<p style="text-align: center"><img src="./img/arXiv_1512_03385/Figure1.png" width="700"></p>
<p style="text-align: center"> Figure 1. Training error (left) and test error (right) on CIFAR-10 with 20-layer and 56-layer "plain" networks. The 56-layer network actually has even higher training and test error. </p>

<br>

<p style="text-align: center"><img src="./img/arXiv_1512_03385/Figure2.png" width="300"></p>
<p style="text-align: center">Figure 2. The deep residual framework.</p>

# Related Work

* **Residual Representations**: Some existing residual representations suggest that a good reformulation or preconditioning can simplify the optimization. These methods include VLAD, Fisher Vector, Multigrid method, hierarchical basis preconditioning. 

* **Short Cinnections**: Concurrent with ResNet, "highway networks" present short connections with gating functions. It has 3 drawbacks compared with ResNet:

  * The gates are data-dependent and have parameters, while ResNet's identity mapping is parameter-free.

  * The gated shortcut may "close" (approaching zero), while ResNet's identity shortcut is never closed.

  * Highway networks have not demonstrated accuracy gains with extremely increased depth.

# Residual Learning

The reseatch team argue that the residual function $F(x):=H(x)-x$ is easier to be optimized than the underlying original function $H(x)$. This argument supported by Figure 3, on which we can see that the residual function in general have small response stds, suggesting that identity mapings provide reasonable preconditioning.

<p style="text-align: center"><img src="./img/arXiv_1512_03385/Figure3.png" width="600"></p>
<p style="text-align: center">Figure 3. Standard deviation (std) of layer responses on CIFAR-10. The responses are the outputs of each 3x3 layer, after BN and before nonlinearlity. <b>Top:</b> the layers are shown in their original order. <b>Bottom:</b> the responses are ranked in descending order.</p>

# ResNet Architecture

The research team introduces 5 members for the ResNet family, which are ResNet-18, 34, 50, 101, and 152. The members share a stem and head architecture, but use different building blocks in stages. The architectures of blocks, stages, stem, head, and the entire ResNet networks are illustrated below.

## ResNet-18/34 Building Block

A building block of ResNet-18/34 is composed of two 3x3 convolutional layers and follow two design rules:

* For the same output feature map size, the layers have the same number of filters and a stride of 1. The identity shortcut is used directly is this case.

* If the feature map size is halved, the layers have number of filters doubled and a stride of 2. A projection shortcut done by 1x1 convolutions is used to match dimensions.

<p style="text-align: center"><img src="./img/arXiv_1512_03385/ResNet_18_34_building_block.png" width="600"></p>
<p style="text-align: center">Figure 4. ResNet-18/34 building block architecture. <b>Left:</b> a block with feature map size and the number of channels unchanged. <b>Right:</b> a block with feature map size halved and the number of channels doubled.</p>

## ResNet-50/101/152 Building Block

For ResNet-50/101/152, their building blocks present a bottleneck transformation, which is done by 1x1, 3x3, and 1x1 convolutions. The 1x1 layers are responsible for reducing and then increasing dimensions, leaving the 3x3 layer a bottleneck with smaller input/output dimensions.

Except the internal bottleneck transformation, the building block as a whole processes feature maps in the same way as the ones of ResNet-18/34. That is, the output feature maps' dimensions are either unchanged, or have height/width halved and channel number doubled. In the former case, a 1x1 convolution layer is also applied on the shortcut connection to match output feature dimensions.

<p style="text-align: center"><img src="./img/arXiv_1512_03385/ResNet_50_101_152_building_block.png" width="600"></p>
<p style="text-align: center">Figure 5. ResNet-50/101/152 building block architecture. <b>Left</b>: a block with feature map size and the number of channels unchanged. <b>Right</b>: a block with feature map size halved and the number of channels doubled.</p>

## ResStage, ResStem, ResHead

To begin with, ResStem is where input images are initially processed. It is composed of a 7x7 convolutional layer and a max pooling layer. Both of them have strides of 2 and the convolutional layer has 64 number of filters, which leads to an output of size (H/4, W/4, 64).

ResStem is followed by several ResStages, which is the core part of ResNet network that residual mappings are applied on. A ResStage is formed by stacking $d$ building blocks, where the 1st block performs feature downsampling if necessary, and the remaining ones maintain a constant feature size. The detailed settings for ResNet-18/34/50/101/152, including the values of $d$s, strides, input and output channel numbers, are listed in Figure 6.

ResHead is the last part of a ResNet network that performs classifications. It is composed of an adaptive average pooling layer, a fully connected layer, and a softmax layer. The pooling reshapes feature maps' widths and heights to a size of (1, 1), and the fully connected layer further transforms the number of channels into the number of classes. Finally, the classification confidence values are computed across the channel dimension by the softmax layer.

<p style="text-align: center"><img src="./img/arXiv_1512_03385/ResStage_ResStem_ResHead.png" width="600"></p>
<p style="text-align: center">Figure 5. The architectures of ResStage <b>(left)</b>, ResStem <b>(middle)</b>, and ResHead <b>(right)</b>.</p>

## ResNet

The diagrams below illustrate the entire architectures and parameter settings of all ResNet family members. To recap, all ResNet members use common architectures of ResStem and ResHead, while the building block of ResStages are different. ResNet-18/34's building blocks use regular convolutional transformation, while ResNet-50/101/152 uses bottleneck transformation instead.

<p style="text-align: center"><img src="./img/arXiv_1512_03385/ResNet.png" width="600"></p>
<p style="text-align: center">Figure 6. The architectures and parameter settings of all ResNet family members.</p>

# Implementation Details

* Resize the image with its shorter side randomly sampled in [256, 480] for scale augmentation.

* A 224x224 random crop is sampled from the an image or its horizontal flip, with per-pixel mean subtracted. 

* The standard color augmentation is used.

* Batch normalizations are adopted right after each convolution and before activation.

* Train all plain/residual nets from scratch.

* SGD mini-batch = 256, learning rate starts from 0.1 and is divided by 10 when the error plateau, weight decay = 0.0001, momentum = 0.9

* Weights are initialized as in [Delving deep into rectifiers:
Surpassing human-level performance on imagenet classification](https://arxiv.org/abs/1502.01852)

* Models are trained for up to $60*10^4$ iterations.

* Dropout is not used.

* In testing, techniques including 10-crop testing, fully-concolutional form are adopted. The team averaged the scores at multiple scales.

# Experimental Observations and Analysis

The comparision of training & validation errors btw 18-layer and 34-layer plain nets and ResNets reveal the following points:

* Plain nets exhibit degradation problems, since plain-34 has higher training error than plain-18, as illustrated in Figure 7. The

