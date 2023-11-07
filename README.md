# Domain Adaptation with Neural Style Transfer

|이름|역할|
|----|---|
|안희상|데이터 정제, Neural Style Transfer, YOLOv5 train|
|양원규|Neural Style Transfer, YOLOv5 train, 결과 분석|

## Domain-Adaptation-w-Neural-Style-Transfer
----
Bridging the gap between the source domain(VKITTI) and the target domain (BDD100k) through neural style transfer applied synthetic data.

### Introduction:
----
Virtual Kitti(VKITTI) offers synthetic object detection data, produced with Unity, that simulates real-world environments. It offers real-time drive images in overcast, morning, sunset, foggy, and rainy backgrounds that are absent in KITTI. On top of VKITTI, we sought to create a stylized dataset with neural style transfer which would perform better than VKITTI on a never-before-seen dataset(BDD100k). We trained two groups of datasets: 1: "KITTI + VKITTI", 2: "KITTI + VKITTI clone + NST VKITTI Clone" . Testing was done on BDD100k to compare the results.

#### Problem at hand:

> YOLOv5 trained on KITTI + VKITTI performs well on KITTI, however, suffers difficulty in detecting in environments not offered by VKITTI nor KITTI.
> Scenes where the model trained with KITTI + VKITTI showed difficulty in detection

![image1](/DATA/exp_image/image1.png)
![image2](/DATA/exp_image/image2.png)
![image3](/DATA/exp_image/image3.png)


#### Cause:
BDD100k provides images taken 24 hours whereas KITTI and VKITTI Clone were taken only during the day
Difference in scenery(background/environment, lighting conditions)
Fails to perform detection especially in cases when the image is taken during nighttime or when sunlight is strong


### Method:
Train model on synthetic data stylized with different background scenes(night, rain, fog, sunset) to adapt to the environment variables prevalent in the target domain. --> Compare performance of VKITTI + KITTI trained model vs. VKITTI + VKITTI Clone + VKITTI Clone NST(our dataset) on BDD100k


#### Models used
[YOLOv5m6, Ultralytics](https://github.com/ultralytics/yolov5)

[Pytorch-Neural-Style-Transfer](https://github.com/reuissir/pytorch-neural-style-transfer)

#### Note: We modified the code of Neural Style Transfer to perform sequential neural style transfer on an entire image directory. During experiments, we used the original code to produce and evaluate a single output at a time

#### Environment
- Ryzen 3600, 2070 Super 8gb, 16 ram
- Google Colab Pro+ (A100)
  
We produced most of our dataset through Google Colab while most of our training was done on my local environment. We experimented with different augmentation techniques and two optimizers[SGD, AdamW] to bring out the best performance during train time.

### Neural Style Transfer
- Neural style transfer was conducted on VKITTI Clone. ** Hyperparameters:** content layer, style layer, content weight, style weight, total variation weight,
init_method[style, content, random(gaussian or white noise)]

  We experimented on different style weights and content weights but our primary focus was on the convolutional layers which we would extract our feature representations. In the paper, they used conv4_2 because it worked best for their artistic style transfers. On the contrary, we concentrated on keeping the edges and overall shape of the cars alive throughout the transfer. Instead of using high-level layers, we chose low-level layers where low-level features are stored.

**We discovered the existence of different style transfer types: artistic, realistic, cartoon, etc during the project. The model we used was for artistic style transfers(we wanted realistic style transfers). Yet, we preceded with our model with a hope that maybe the model will still be forced to find the right features inspite of all the diversity in the data.**
  - Content layer: conv_2 showed best results with content information still strong.
    - Differed per style image. Sometimes conv_3 worked better (ex. Fog)
  - Style layer: style information was extracted from every layer except the layer responsible for the content.
  - Different init_methods were used for various styles(fog-content, rain-style, night-content, sunset-content)
  - The content images were set as VKITTI Clone(2066 images)
  - A total of 2066 * 4(styles), 8264 images were stylized to form our NST Dataset.
    - KITTI + VKITTI clone + NST dataset was trained to be compared with the original KITTI + VKITTI dataset.

##### note: examples posted were rescaled to 250250. The ones used for training are 640640

|night|fog|rain|sunset|
|---|---|---|---|
|1|2|3|4|

