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
* **AUTHORS** : Philipp Krähenbühl, Vladlen Koltun
* **LINK** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Paper/crf.pdf

## IMPLEMENTATION STEPS :
#### 1. Converting a classifier to dense FCN:
The model which is used for the task of semantic segmentation is derived from VGG. VGG on it's own is meant for classification task. So to make the model suitable for dense prediction we remove the last fully connected layers of VGG and replace them with convolutions. We append a 1x1 convolution with channel dimension 21  to predict scores for each of the PASCAL classes (including background) at each of the coarse output locations, followed by a deconvolution layer to bilinearly upsample the coarse outputs to pixel-dense outputs.

#### 2. Transferring features of lower level layers to higher layers
We define a new fully convolutional net (FCN) for segmentation that combines layers of the feature hierarchy and refines the spatial precision of the output. While fully convolutionalized classifiers can be fine-tuned to segmentation and even score highly on the standard metric, their output is dissatisfyingly coarse. The 32 pixel stride at the final prediction layer limits the scale of detail in the upsampled output.

We address this by adding skips that combine the final prediction layer with lower layers with finer strides. This turns a line topology into a DAG with edges that skip ahead from lower layers to higher ones. As they see fewer pixels, the finer scale predictions should need fewer layers, so it makes sense to make them from shallower net outputs. Combining fine layers and coarse layers lets the model make local predictions that respect global structure.

We first divide the output stride in half by predicting from a 16 pixel stride layer. We add a 1x1 convolution layer on top of pool4 to produce additional class predictions. We fuse this output with the predictions computed on top of conv7 (convolutionalized fc7) at stride 32 by adding a 2x upsampling layer and summing both predictions.

Finally, the stride 16 predictions are upsampled back to the image.

We call this net FCN-16s.
FCN-16's have only one skip connection which transferring the information from 4th Max pooling layer. To improve the results further we introduce one more skip connection which transfer information from 3rd Max pooling layer also with the skip connection which transfers information from 4th Max pooling layer.

* **Plot of FCN-16 Architecture** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Plots/FCN-16_withshape.png
* **Plot of FCN-8 Architecture** : https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/blob/master/Plots/FCN-8with_shapes.png

#### 3. Using CRF as post processing technique :
While predicting using FCN we gave label to each pixel independently of it's surrounding pixels, this may result in coarse segmentation. CRF takes two inputs one is the original image and the other is predicted probabilities for each pixel. The CRF which was uses a highly efficient inference algorithm for fully connected CRF models in which the pairwise edge potentials are defined by a linear combination of Gaussian kernels in an arbitrary feature space. Thereby it considers the surrounding pixels also while assigning the class to particular pixel which results in better semantic segmentation results.

## INSTALLATION OF REQUIRED TOOLS
#### 1. Tensorflow
Refer to the following link https://www.tensorflow.org/install/install_sources. Tensorflow is used as backend for Keras. The link contains installation instructions with and without gpu support

#### 2. Keras
Keras is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano. It was developed with a focus on enabling fast experimentation.

* To install Keras

    sudo pip install keras

#### 3. Matplotlib
Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms.
* Refer to following link for installation instructions https://matplotlib.org/users/installing.html.
While installing it or tensorflow number of dependencies like Numpy will be installed.

#### 4.Skimage
Scikit-image is an image processing toolbox for SciPy. It is used for loading, saving and applying various transformations like color to gray and gray to color on images.

* Refer following link for installation instructions http://scikit-image.org/docs/dev/install.html

#### 5. graphviz
This package facilitates the creation and rendering of graph descriptions in the DOT language of the Graphviz graph drawing software from Python. It is required to plot the models in keras.
* To install graphviz

    sudo pip install graphviz
    
#### 6. Jupyter Notebook
The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text
* Refer following link for installation instructions https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jupyter-notebook-to-run-ipython-on-ubuntu-16-04

    
# Quick start

Run the following commands in Terminal. 

    git clone https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation.git
    cd FCN-for-Semantic-Segmentation
    jupyter notebook
    
It opens up all the notebooks which are there in the directory in the browser. 

* FCN-16.ipynb contains code related to implementation of FCN-16.

* FCN-8.ipynb contains code related to implementation of FCN-8
* Comparison_of_fcn8_and_fcn16.ipynb has code which compares results of FCN-8 and FCN-16 models.
* CRF.ipynb has code which is used to compare the results after applying CRF on FCN-8 and FCN-16 annotated images.
* All the images which are used can be found in `Testimages` Folder (https://github.com/Gurupradeep/FCN-for-Semantic-Segmentation/tree/master/TestImages)

Open respective notebooks and run the commands to reproduce the results. As we are running in jupyter notebook we can see results after executing every command.



    








