 # Deconv


 ## Install Dependencies

 We recommend using pip to install the required dependencies:

 ```
	
  
 ```

 This code also requires at least one CUDA-enabled GPU from NVIDIA to run properly. You can test to see if you have this availble by running:

 ```
 nvidia-smi
 ```

 in your terminal.
 
 ## Settings Overview
 We have included a few settings you can add into the run command.

 The basic run command (for non-imagenet dataset) is:

 ```
 python main.py --[keyword1] [argument1] --[keyword2] [argument2]  ...
 ```

 The major keywords to note are:

 * deconv - set to True or False if you want to test deconv (True) or BN (False)
 * arch - use a given architecture (resnet50, vgg11, vgg13, vgg19, densenet121)
 * wd - sets the weight decay to a given value
 * batch-size - sets the batch size
 * method - sets the method for deconv.  3 for channel-only and 4 for full
 * epochs - the number of epochs to run
 * dataset - the dataset to use (cifar10, cifar100) (for imagenet you need the other main file)
 * lr - sets the learning rate

 ## Running the examples from the paper

 We ran all of our experiments on two to four Nvidia Geoforce 1080Ti GPUs (for
 CIFAR-10 and CIFAR-100), but your needs may vary. 

 Depending on how many GPUs you have, you will need to change the CUDA_VISIBLE_DEVICES parameter before 'python main.py'.

run:

```
nvidia-smi
```
and see how many GPUS you have.  They will be given a number from 0 -> n, where n is the number of GPUs.  Wherever we have listed CUDA_VISIBLE_DEVICES, put 0,->n to the right of the equal sign.  For example, if you see three GPUs, numbered 0,1,2, then you would put

```
CUDA_VISIBLE_DEVICES=0,1,2
```
Putting less devices will still work, but it will be slower. To be safe, you can always just copy the code we have given you here, which sets CUDA_VISIBLE_DEVICES to be 0, the first GPU. 
 
 ## 1. For logistic regression (--loss L2 for L2 linear regression): 


python main.py --lr .1 --optimizer SGD --arch simple_v1 --epochs 1 --dataset cifar10  --batch-size 512 --msg True --method 1 --loss CE
 

 ## 2. For a simple cnn: 


method 1: vanilla network + sgd, method 2: batch norm + sgd, method 3: channel deconv with 32 groups 


```
python main.py --lr .1 --optimizer SGD --arch mlp --epochs 20 --dataset mnist  --batch-size 512 
```


## 3. Train a resnet on CIFAR-10 (with/without deconv):

batch normalization:
```
CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch resnet --epochs 100 --dataset cifar10  --batch-size 512 --msg True --deconv False --num-groups-final 0 
```
deconv:
```
CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch resnet --epochs 100 --dataset cifar10  --num-groups-final 512 --batch-size 512 --msg True --deconv True 
```

 ## 4. Train a resnet50 on CIFAR-100 (with/without deconv):

batch norm.:
```
CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch resnet50 --epochs 100 --dataset cifar100  --batch-size 128 --msg True --deconv False --num-groups-final 0 
```
deconv.:
```
CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch resnet50 --epochs 100 --dataset cifar100  --batch-size 128 --msg True --deconv True --num-groups-final 0 
```
## 5. Train a vgg[11/13/19] network on CIFAR-10/100 (with/without deconv)

For vgg13 or vgg19, replace vgg11 with either vgg13 or vgg19. 

for batch norm:
```
CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch vgg11 --epochs 100 --dataset cifar100  --batch-size 128 --msg True --deconv False --num-groups-final 0
```
deconv:

```

CUDA_VISIBLE_DEVICES=0 python main.py --lr .1 --optimizer SGD --arch vgg11 --epochs 100 --dataset cifar100  --batch-size 128 --msg True --deconv True --num-groups-final 0

```
# 7. Training with Channel Deconv Only

To train a network with channel deconv only, use method '3' in the --method argument.

example:



   
 # 6. imagenet dataset:


1. original resnet18 (90 epochs, use --epochs xx to change)

python main_imagenet.py -a resnet18 -j 32 imagenet/ILSVRC/Data/CLS-LOC 

2. deconv resnet18

python main_imagenet.py -a resnet18d -j 32 imagenet/ILSVRC/Data/CLS-LOC --deconv True

3. deconv vgg11
 

python main_imagenet.py -a vgg11d -j 32 /vulcan/scratch/cxy/Data/imagenet/ILSVRC/Data/CLS-LOC --deconv True --lr 0.01 >vgg11d.imagenet.log &


 


