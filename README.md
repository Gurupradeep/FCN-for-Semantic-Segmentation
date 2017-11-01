# FCN-for-Semantic-Segmentation

Implementation and testing the performance of FCN-16 and FCN-8. In addition to that CRFs are used as a post processing technique and results are compared.

## PAPERS REFERRED :
1. **FULLY CONVOLUTIONAL NETWORKS FOR SEMANTIC SEGMENTATION**
* **AUTHORS** : Jonathan Long, Evan Shelhamer, Trevor Darrell
* **LINK** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Paper/long_shelhamer_fcn.pdf

2. **VERY DEEP CONVOLUTIONAL NETWORKS FOR LARGE SCALE IMAGE RECOGNITION**
* **AUTHORS** : Karen Simonyan, Andrew Zisserman
* **LINK** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Paper/VGG.pdf

3. **Efficient Inference in Fully Connected CRFs with Gaussian Edge Potentials**
* **AUTHORS** :Philipp Krähenbühl, Vladlen Koltun
* **LINK** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Paper/crf.pdf

## IMPLEMENTATION STEPS :
#### 1. Converting a classifier to dense FCN :
The model which is used for the task of semantic segmentation is derived from VGG. VGG on it's own is meant for classification task. So to make the model suitable for dense prediction we remove the last fully connected layers of VGG and replace them with convolutions. We append a 1x1 convolution with channel dimension 21  to predict scores for each of the PASCAL classes (including background) at each of the coarse output locations, followed by a deconvolution layer to bilinearly upsample the coarse outputs to pixel-dense outputs.

#### 2. Transferring features 




