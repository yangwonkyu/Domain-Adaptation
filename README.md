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

![image1](/DATA/epx_image/image1.png)
![image2](/DATA/epx_image/image2.png)
![image3](/DATA/epx_image/image3.png)
