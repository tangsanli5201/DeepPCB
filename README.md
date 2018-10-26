# DeepPCB
DeepPCB: a dataset contains 1,500 image pairs, each of which consists of a defect-free template image and an aligned tested image with annotations including positions of 6 most common types of PCB defects: open, short, mousebite, spur, pin hole and spurious copper.

## Dataset Description
### Image Collection
All the images in this dataset is obtained from a linear scan CCD in resolution around 48 pixels per 1 millimetre. The defect-free template image is manually checked and cleaned from a sampled image in the above manner. The original size of the template and tested image is around 16k x 16k pixels. Then they are clipped into sub-images with size of 640 x 640 and aligned through template matching techniques. Next, a threshold is carefully selected to employ binarization to avoid illumination disturbance. Notice that pre-processing algorithms can be various according to the specific PCB defect detection algorithms, however, the image registration and thresholding techniques are common process for high-accuracy PCB defect localization and classification. An example pair in DeepPCB dataset is illustrated in the following figure, where the top one is the defect-free template image and the bottom one is the defective tested image with the ground truth annotations.
<div align=center><img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/test.jpg" width="375" style="margin:20">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/template.jpg" width="375" style="margin:20"> 
 </div>
<div align=center>
 an example of the tested image 
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 the corresponding template image
 </div>

### Image Annotation
We use the axis-aligned bounding box with a class ID for each defect in the tested images. As illustrated in the above, we annotate six common types of PCB defects: open, short, mousebite, spur, pin hole and spurious copper. Since there is only a few defect in the real tested image, we manually argument some artificial defects on each tested image according to the PCB defect patterns, which leads to around 3 to 12 defects in each 640 x 640 image. The number of PCB defects is shown in the following figure. We separate 1,000 images as training set and the remains as testing set.
<div align=center>
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/CountPCB.png" width="560"> 
 </div>
 
### Benchmarks
The average precision rate and F-score are used for evaluation. A detection is correct only if the intersection of unit (IoU) between the detected bounding box and any of the ground truth box with the same class is larger than 0.5. F-score is calculated as: F-score=2*P*R/(P+R), where P and R is the precision and recall rate. Notice that F-score is threshold-sensitive, which means you may adjust your score threshold to obtain a better result. Although F-score is not as fair as the mAP criteria but more practical since a threshold should always be given when deploying the model and not all of the algorithms have a score evaluation for the target. Thus, F-score and mAP are both under consideration in the benchmarks. 

The evaluation script for mAP and F-score are borrowed from [Icdar2015 evaluation scripts](http://rrc.cvc.uab.es/?ch=4&com=mymethods&task=1) with small modification. (You may first register an account.) Here, we give the modified evaluation scripts and the ground truth _gt.zip_ file of the test set in _evaluation/_ directory. You can evaluate your own method by following instructions:
* run your algorithm and save the detect result for each image named as *image_name.txt*. You should follow the format of *evaluation/gt.zip* except that each defect description should be:
			*x1,y1,x2,y2,confidence,type*, where *(x1,y1)* and *(x2,y2)* is the top left and the bottom right corner of the bounding box of the defect. *confidence* is a float number to show how confident you believe such detection result. *type* is a string and should be one of the following: *open,short,mousebite,spur,copper,pin-hole*. Notice there is no space except the comma.
* zip your *txt* file to *res.zip*. 
* run the evaluation scrip python script.py -s=res.zip -g=gt.zip

## Approach
This section with the source code will be public after the acceptance of the paper.

### Experiment results
Here we show some results of our model based on deep neural network. Our model achieves mAP of 98.6%, 98.2% F-score @ 62FPS. More statistic analysis will be public after the acceptance of the paper. The green bounding box is the predicted location of the PCB defect with the confidence on the top of each.

Result pair 1:
<div align=center><img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_test1.jpg" width="375" style="margin:20">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_temp1.jpg" width="375" style="margin:20"> 
 </div>
Result pair 2:
<div align=center><img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_test2.jpg" width="375" style="margin:20">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_temp2.jpg" width="375" style="margin:20"> 
 </div>
 Result pair 3:
<div align=center><img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_test3.jpg" width="375" style="margin:20">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_temp3.jpg" width="375" style="margin:20"> 
 </div>
 Result pair 4:
<div align=center><img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_test4.jpg" width="375" style="margin:20">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://github.com/tangsanli5201/DeepPCB/blob/master/fig/result/result_temp4.jpg" width="375" style="margin:20"> 
 </div>

#### Notification
This work is contributed by the paper **_An Efficient and Acurrate Detector for Online PCB Defect Detection_**.
