---
{"dg-publish":true,"permalink":"/garden/notes-on-nn-u-net/"}
---

Link to the nnUNet paper: https://arxiv.org/pdf/1809.10486
 
The papers does not propose a new model architecture, the proposal is geared more towards the usage of hyperparameters other than the usual model architecture for the most part. It gives more importance to steps like preprocessing, training (loss functions, optimiser setting, data augmentation), inference (patch based strategy, ensembling across test-time augmentation and models) and a potential post processing (enforcing single connected components ifi applicable)

UNet cascade that is used in the framework

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Stage 1
a 3D UNet is trained on down-sampled images 
Stage 2
The segmentations from the previous step is upsampled to the original voxel dims
a second 3D UNet is trained on patches with full resolution
 
UNet Cascade  


</div></div>


### Dynamic adaptation of network topologies
Due to the various shapes of the images and the availability of GPU memory, the framework has to automatically pass along the tradeoff between batch size, network capacity etc .
#### 3D UNet case

- Uses 30 feature maps at the highest resolution layers. 
- Base confguration for input size: 128x128x128 batch size of 2
- If median shape of the dataset is less than 128^3 then that median shape is taken as the input shape
- If the size is larger, then the aspect aratio of the patch is changed to keep the overall thing more consistent.
- pool along each axis (maximum of 5times) untli the feature maps hasve a size of 8.

The main idea is that regardless of the shape of ht dataset, a constant amout of GPU memory is being used, they acheive this by changing the size of the patch, so that it is not strcitly a cube, it could be a cuboid, but the overall shape/ volume remains constant.

### Preprocessing steps
#### Cropping
All data is cropped to the region of non zero values. Need to understand how this plugs in with out ROI cropping step

#### Resampling
All samples are resampled to the mean voxel spacing. This is also something that could affect our dataset drastically.
Third order spline interpolation is used for image data and nearest neighbor interpolation for the segmentation mask.

Check for activating Cascade UNet:
if the median shape of resampled data has more than 4 times the voxels that can be processed as input patch by the 3D Unet (with a batch size of 2), it activates Cascade.
 what happens when this condition is met:
 - dataset is resampled to a lowert reolution, increase voxel sapcinig by a factor of 2 until the 4 times thnigy condition is met.
 - if dataset is anistropic, the higher resolutoin axis iis firt downsampled until they match the resolution of the low res axis, and then all of them are downsampled together.

#### Normalization
Clipping iin the [0.5, 0.95] percentile, and then applying a z-score normalisation.

### Training Procedure
5 fold CV
loss function: Dice + CE
calculate loss for each patient and then average over the batch
adam optimiser with lr 3e-4



## List of things to read further on:
- Test time augmentation
- patch based strategy ( i am assuming this is somethng similar to sliding window) 
- instance normalisation
- anisotropic datast 
- How does limited field of view affect model performance, and is this something that our model suffers from?
- Third order spline interpolation

