# Deformable Convolutional Networks


The major contributors of this repository include [Yuwen Xiong](https://github.com/Orpine), [Haozhi Qi](https://github.com/Oh233), [Guodong Zhang](https://github.com/gd-zhang), [Yi Li](https://github.com/liyi14), [Jifeng Dai](https://github.com/daijifeng001), [Bin Xiao](https://github.com/leoxiaobin) and  [Yichen Wei](https://github.com/YichenWei).

## Disclaimer

This is an official implementation for [Deformable Convolutional Networks](https://arxiv.org/abs/1703.06211) (Deformable ConvNets). It is worth noticing that:

  * The original implementation is based on our internal Caffe version on Windows. There are slight differences in the final accuracy and running time due to the plenty details in platform switch.
  * The code is tested on official [MXNet@(commit 62ecb60)](https://github.com/dmlc/mxnet/tree/62ecb60) with the extra operators for Deformable ConvNets.
  * We trained our model based on the ImageNet pre-trained [ResNet-v1-101](https://github.com/KaimingHe/deep-residual-networks) using a [model converter](https://github.com/dmlc/mxnet/tree/430ea7bfbbda67d993996d81c7fd44d3a20ef846/tools/caffe_converter). The converted model produces slightly lower accuracy (Top-1 Error on ImageNet val: 24.0% v.s. 23.6%).
  * By now it only contains Deformable ConvNets with R-FCN. Deformable ConvNets with DeepLab will be released soon.
  * This repository used code from [MXNet rcnn example](https://github.com/dmlc/mxnet/tree/master/example/rcnn) and [mx-rfcn](https://github.com/giorking/mx-rfcn).

## Introduction


**Deformable ConvNets** is initially described in an [arxiv tech report](https://arxiv.org/abs/1703.06211).

**R-FCN** is initially described in a [NIPS 2016 paper](https://arxiv.org/abs/1605.06409).


<img src='demo/deformable_conv_demo1.png' width='800'>
<img src='demo/deformable_conv_demo2.png' width='800'>
<img src='demo/deformable_psroipooling_demo.png' width='800'>


## License

© Microsoft, 2017. Licensed under an Apache-2.0 license.

## Citing Deformable ConvNets

If you find Deformable ConvNets useful in your research, please consider citing:
```
@article{dai17dcn,
    Author = {Jifeng Dai, Haozhi Qi, Yuwen Xiong, Yi Li, Guodong Zhang, Han Hu, Yichen Wei},
    Title = {Deformable Convolutional Networks},
    Journal = {arXiv preprint arXiv:1703.06211},
    Year = {2017}
}
@inproceedings{dai16rfcn,
    Author = {Jifeng Dai, Yi Li, Kaiming He, Jian Sun},
    Title = {{R-FCN}: Object Detection via Region-based Fully Convolutional Networks},
    Conference = {NIPS},
    Year = {2016}
}
```

## Main Results

|                                 | training data     | testing data | mAP@0.5 | mAP@0.7 | time   |
|---------------------------------|-------------------|--------------|---------|---------|--------|
| R-FCN, ResNet-v1-101            | VOC 07+12 trainval| VOC 07 test  | 79.6    | 63.1    | 0.16s |
| Deformable R-FCN, ResNet-v1-101 | VOC 07+12 trainval| VOC 07 test  | 82.3    | 67.8    | 0.19s |



|                                 | <sub>training data</sub> | <sub>testing data</sub>  | <sub>mAP</sub>  | <sub>mAP@0.5</sub> | <sub>mAP@0.75</sub>| <sub>mAP@S</sub> | <sub>mAP@M</sub> | <sub>mAP@L</sub> |
|---------------------------------|---------------|---------------|------|---------|---------|-------|-------|-------|
| <sub>R-FCN, ResNet-v1-101 </sub>           | <sub>coco trainval</sub> | <sub>coco test-dev</sub> | 32.1 | 54.3    |   33.8  | 12.8  | 34.9  | 46.1  | 
| <sub>Deformable R-FCN, ResNet-v1-101</sub> | <sub>coco trainval</sub> | <sub>coco test-dev</sub> | 35.7 | 56.8    | 38.3    | 15.2  | 38.8  | 51.5  |


*Running time is counted on a single Maxwell Titan X GPU (mini-batch size is 1 in inference).*

## Requirements: Software

1. Python packages might missing: cython, opencv-python >= 3.2.0, easydict. If `pip` is set up on your system, those packages should be able to be fetched and installed by running
	```
	pip install Cython
	pip install opencv-python==3.2.0.6
	pip install easydict==1.6
	```
2. For Windows users, Visual Studio 2015 is needed to compile cython module.


## Requirements: Hardware

Any NVIDIA GPUs with at least 4GB memory should be OK.

## Installation

1. Clone the Deformable ConvNets repository
~~~
git clone https://github.com/msracver/Deformable-ConvNets.git
~~~
2. For Windows users, run ``cmd .\init.bat``. For Linux user, run `sh ./init.sh`. The scripts will build cython module automatically and create some folders.
3. Copy operators in `./rfcn/operator_cxx` to `$(YOUR_MXNET_FOLDER)/src/operator/contrib` and recompile MXNet.
4. Please install MXNet following the official guide of MXNet. For advanced users, you may put your Python packge into `./external/mxnet/$(YOUR_MXNET_PACKAGE)`, and modify `MXNET_VERSION` in `./experiments/rfcn/cfgs/*.yaml` to `$(YOUR_MXNET_PACKAGE)`. Thus you can switch among different versions of MXNet quickly.


## Demo

1. To use the demo with our trained model (on COCO trainval), please download the model manually from [OneDrive](https://1drv.ms/u/s!AoN7vygOjLIQqmE7XqFVLbeZDfVN), and put it under folder `model/`.

	Make sure it looks like this:
	```
	./model/rfcn_dcn_coco-0000.params
	./model/rfcn_coco-0000.params
	```
2. To run the demo, run
	```
	python ./rfcn/demo.py
	```
	By default it will run Deformable R-FCN and gives several prediction results, to run R-FCN, use
	```
	python ./rfcn/demo.py --rfcn_only
	```
	


We will release the visualizaiton tool which visualizes the deformation effects soon.


## Preparation for Training & Testing

1. Please download COCO and VOC 2007+2012 dataset, and make sure it looks like this:

	```
	./data/coco/
	./data/VOCdevkit/VOC2007/
	./data/VOCdevkit/VOC2012/
	```

2. Please download ImageNet-pretrained ResNet-v1-101 model manually from [OneDrive](https://1drv.ms/u/s!Am-5JzdW2XHzhqMEtxf1Ciym8uZ8sg), and put it under folder `./model`. Make sure it looks like this:
	```
	./model/pretrained_model/resnet_v1_101-0000.params
	```

## Usage

1. All of our experiment settings (GPU #, dataset, etc.) are kept in yaml files at folder `./experiments/rfcn/cfgs`.
2. Four config files have been provided so far, namely, R-FCN for COCO/VOC and Deformable R-FCN for COCO/VOC, respectively. We use 8 and 4 GPUs to train models on COCO and on VOC, respectively.
3. To perform experiments, run the python scripts with the corresponding config file as input. For example, to train and test deformable convnets on COCO with ResNet-v1-101, use the following command
    ```
    python experiments\rfcn\rfcn_end2end_train_test.py --cfg experiments\rfcn\cfgs\resnet_v1_101_coco_trainval_rfcn_dcn_end2end_ohem.yaml
    ```
    A cache folder would be created automatically to save the model and the log under `output/rfcn_dcn_coco/`.
4. Please find more details in config files and in our code.

## Misc.

MXNet build without CuDNN is recommended.

Code has been tested under:

- Ubuntu 14.04 with a Maxwell Titan X GPU and Intel Xeon CPU E5-2620 v2 @ 2.10GHz
- Windows Server 2012 R2 with 8 K40 GPUs and Intel Xeon CPU E5-2650 v2 @ 2.60GHz
- Windows Server 2012 R2 with 4 Pascal Titan X GPUs and Intel Xeon CPU E5-2650 v4 @ 2.30GHz

