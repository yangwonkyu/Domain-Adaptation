# Domain Adaptation with Neural Style Transfer

|이름|역할|
|안희상|데이터 정제, Neural Style Transfer, YOLOv5 train|
|양원규|Neural Style Transfer, YOLOv5 train, 결과 분석|

Domain-Adaptation-w-Neural-Style-Transfer
Bridging the gap between the source domain(VKITTI) and the target domain (BDD100k) through neural style transfer applied synthetic data.

Introduction:
Virtual Kitti(VKITTI) offers synthetic object detection data, produced with Unity, that simulates real-world environments. It offers real-time drive images in overcast, morning, sunset, foggy, and rainy backgrounds that are absent in KITTI. On top of VKITTI, we sought to create a stylized dataset with neural style transfer which would perform better than VKITTI on a never-before-seen dataset(BDD100k). We trained two groups of datasets: 1: "KITTI + VKITTI", 2: "KITTI + VKITTI clone + NST VKITTI Clone" . Testing was done on BDD100k to compare the results.

