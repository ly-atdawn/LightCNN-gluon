# Light CNN for Deep Face Recognition.
* gluon implementation of LightCNN in the paper "A Light CNN for Deep Face Representation with Noisy Labels" [[Paper]](https://arxiv.org/abs/1511.02683)
* Borrowed code and ideas from yxu0611's Tensorflow-implementation-of-LCNN: https://github.com/yxu0611/Tensorflow-implementation-of-LCNN.

## Install Required Packages
First ensure that you have installed the following required packages:
* gluon. gluon is the interface of mxnet. The version of mxnet is mxnet0.12.0, maybe other version is ok.
* Opencv ([instructions](https://github.com/opencv/opencv)). Here is opencv-2.4.9.

See requirements.txt for details.

## Datasets
* In the implementation of the LightCNN, MsCelebV1-Faces-Aligned subset as the training data and validating data. Utilize the 10K, 70K [download](https://drive.google.com/open?id=1-jC6E2_gtuVLJxtITzSSX4ACKlOiGSat) to generate the subset of the MsCelebV1-Faces-Aligned. Then, use the mxnet tools im2rec.py to get the Record file, the image size is 144x144. When training LightCNN, all the training data are converted to gray-scale images and randomly croped to 128x128.

## Training a Model
* Run the following script to train and validate the model.
```shell
python train_lightcnn.py
```
You could change some arguments in the train_lightcnn.py, like num_epochs, gpus. If want to get the good results and save training time, there are some points should note:
 - For weights initialization,  use "xavier" initialization for "weight" of every layer, the "bias" will initializate to constant value "0.1".
 - For learning rate,  use "AdamOptimizer" as optimization method, and initial learning rate is 0.00001. So in mxnet, you can set the initial learning rate is 0.0001, lr_step_epochs is '0, 100', in this way, the initial learning rate is 0.00001 in epoch 0.

 ## Extract features of LFW
 * Run the following script to extract every image's feature, moreover save the features and image path to MATLAB data.
 ```shell
 cd extract_features
 python extract_features.py
 ```
 The script will load the trained LightCNN model to generate fc layer output, i.e. the feature of a image. You could change the arguments in test.sh depend on your machine config. You could download the aligned LFW datasets and lfw_patch_part.txt from here [download](https://drive.google.com/open?id=1Xq6wITloANkIv66eysMw3ZwW1fv5-adM).

 ## Face Verification
 * Utilize MATLAB to run the lfw_eval.m to verificate a pair of images. You must generate the features of the aligned LFW datasets in advance. See **lfw_verification-matlab/lfw_eval.m** for details. 

## Downloading trained model
* Pretrained model: [download](https://drive.google.com/open?id=1tWUKIP2zg_rHCT_vxA4YZV7Z0_1rFBqm). Training about 35 epoches use 10k people, the loss has converged. But the accuracy on the aligned LFW datasets is not high, about 94%.

* Some improved solutions:
 1. Change the optimizer method, like: when loss doesn't decrease, manually decrease it to 10 time smaller.
 2. Improved by manaully aligning the images which are mis-algined in LFW datasets.
 3. Improved by using metric learning method for similarity caculation, not just cos value.
