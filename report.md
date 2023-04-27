# PARK MY BIKE
Author: André Bourgeois

Project: [Github Repository](https://github.com/andrelbourgeois/park-my-bike) and [Edge Impulse Repository](https://studio.edgeimpulse.com/public/201800/latest)

## Introduction
This project details a study undertaken to understand whether a camera placed at a bicycle bay could be used to remotely inform cyclists of available parking spaces at that bicycle bay. In order to accomplish this, a deep learning model was developed and trained to detect bicycles using [Edge Impulse](https://www.edgeimpulse.com/). The idea being, that if the number of bicycles can be accurately obtained at a specific bicycle bay and there is previous knowledge regarding the number of parking spaces at that bicycle bay, then the difference between these numbers is the number of available parking spaces. This number can then be pushed - along with the device's location - to a popular mapping application for public use. The aim of this project, however, is not the design of this entire system, but instead the design and deployment of this system’s deep learning capability. Figure 1 provides a more tangible idea of what the whole system might look like in practice.

![park-my-bike-diagram](https://user-images.githubusercontent.com/33913141/234854034-9085e4f7-a9dc-4307-a0bf-62e0e6781c6d.png)

Figure 1 - Example of Park My Bike Deployment

The motivation for this project is twofold. As a cyclist myself, my friends and I understand the difficulty of locating bicycle parking in a large city. There are many available spaces throughout the city, but they can be difficult to find and are often filled in popular areas. Having the ability to both locate traditional bicycle bays and know whether they have available parking spaces would significantly reduce the friction associated with cycling to new destinations in London.
Additionally, a similar system is already in use by London's self-service bike-sharing scheme, Santander Cycles. Although this scheme is enabled through a system of docking stations throughout the city instead of computer vision, the end result is the same - live data pushed to your favourite wayfinding app that shows cyclists the location and availability of bicycles or parking at the Santander Cycle stations throughout the city. This feature is shown in Figure 2.

![santander-ex](https://user-images.githubusercontent.com/33913141/232324525-efa49797-fa02-4039-96cb-835080c791ce.png)

Figure 2 - Example of Santander Bicycle Sharing Stations on Google Maps

## Research Question
Can a camera deployed near a bicycle bay accurately detect the number of bicycles parked at that bicycle bay, and therefore, help in determining the number of available parking spaces?

## Application Overview
The purpose of this application is to identify bicycles. The image input relies on the edge device's camera - in this case, the OV7675 CMOS VGA Camera Module connected to the Arduino Nano 33 BLE Sense. While images are being captured through a real-time video feed, they are each being processed and split into a grid where the equivalent of image classification is ran across all cells in the grid independently in parallel. (Moreau, 2022) A depiction of this processing stage can be seen in Figure 4. Following this processing, the model searches the image for features similar to those it was trained on - bicycles - and makes a decision as to whether or not there are any bikes in the image. With the FOMO model, an affirmative decision is depicted as a centroid marker on the image, because instead of predictin bounding boxes, FOMO predicts the objects centre (Dickson et al., 2022). An example of an image with a bounding box next to the same image with a centroid marker can be seen in Figure 5.

![application-overview](https://user-images.githubusercontent.com/33913141/234854077-d049e68f-f8e9-4e5a-b7c0-1493d452a0cb.png)

Figure 3 - Application Diagram

![example-image-proc](https://user-images.githubusercontent.com/33913141/234701976-f93cd07f-6dae-4cc7-a8ab-ab9fbe141bd7.png)

Figure 4 - Example of FOMO Image Processing; 320x320 Image Split Into a 40x40 Grid (Moreau, 2022)

![box-vs-cent](https://user-images.githubusercontent.com/33913141/234854130-cdd1e873-9885-448d-8825-4fe6b0b2bfcb.png)

Figure 5 - Example of FOMO's Centroid Marker

## Data
The data utilised for this project combined open-source, online images and a dataset of custom images taken around London, UK.

The online images were retrieved in a single dataset from images.cv - a website offering open-source, labelled image datasets. This [dataset](https://images.cv/dataset/bicycle-image-classification-dataset) contained 705 images of bicycles, bike-related objects, and images that could be labelled as bicycle, such as bicycle kicks and bicycle playing cards. The website provides the ability to download this dataset in a range of sizes, with options for colour. It also provides the option to divide the images into folders for training, validation, and testing. With these features and after some pruning of images I didn't believe to be useful, I was left with 593, greyscaled images at a size of 256px x 256px. Example images can be seen in Figure 6.

![gray-2WZF8BLTOBMY](https://user-images.githubusercontent.com/33913141/232325511-8c5b96da-0467-4f46-a7d1-f5ee82f54ce9.jpg) &nbsp;&nbsp;&nbsp;
![gray-2EVEA5H88OEQ](https://user-images.githubusercontent.com/33913141/232341993-81e1c81e-aca3-4646-a54a-84552117d84b.jpg) &nbsp;&nbsp;&nbsp;
![gray-2VARMD3C8MZF](https://user-images.githubusercontent.com/33913141/232342052-bd7e1d77-40b9-400e-a0b6-73148df6c06e.jpg)

Figure 6 - Example Images from Image.cv Dataset

The custom images were taken by myself, over the course of two weeks throughout my daily commutes in London. These Images began as 3024px × 4032px coloured photos. In order to process them, I first recoloured them and cropped them into 3024px x 3024px squares. Next, I reduced their resolution to 1024px x 1024px using the [ImageMagick](https://imagemagick.org/index.php) app. Finally, I split each image into 16 separate images to increase the size of the dataset and get a good mix of images that contained bicycles and images with scenery around the bicycles. This process resulted in 848 greyscaled images at a size of 256px x 256px. Figure 7 demonstrates the custom image processing.

![tiled-example](https://user-images.githubusercontent.com/33913141/232341346-e35a9ab2-1f36-45c1-84aa-d26142a5de61.png)

Figure 7 - Example Image Processing for Custom Data

Decisions regarding the processing of these images are as follows:
- Greyscaling; The FOMO model used currently only accepts greyscaled images and greyscaling simplifies the alogrithm and reduces computational requirements (Kanan & Cottrell, 2012) by reducing colour variable down to a single number between 0 - 255.
- Squaring; The FOMO model used currently only accepts square images and it was a common standard dimension within all of the research I conducted as many model topologies require square input images (Isahit, 2022). 
- Tiling; Allowed me to artificially increase the size of the dataset by augmenting these images.

In total, the combined dataset that was used contained 1441 images, similar to those shown in the figures directly above.

In order to label each image, I manually drew bounding boxes around any bicycles using the Edge Impulse platform. See Figure 8 for examples.

![label-ex1](https://user-images.githubusercontent.com/33913141/234707499-47352fff-d8ae-479a-a370-168cf782c9f8.png)
&nbsp;&nbsp;&nbsp;
![label-ex2](https://user-images.githubusercontent.com/33913141/234707532-adf97aa0-1b5a-49d7-8980-12a19b246e92.png)
&nbsp;&nbsp;&nbsp;
![label-ex3](https://user-images.githubusercontent.com/33913141/234707555-f20e5f8f-4b67-478e-89be-784cebb60a7c.png)

Figure 8 - Example Image Labelling with Bounding Boxes

## Model
This is a Deep Learning project! What model architecture did you use? Did you try different ones? Why did you choose the ones you did?

In this project, I tested 3 model architectures which represented a selection of all model architectures on the Edge Impulse platform.

- FOMO MobileNetV2 0.1 (referred to as FOMO 0.1)
- FOMO MobileNetV2 0.35 (referred to as FOMO 0.35)
- MobileNetV2 SSD FPN-Lite 320x320 (referred to as FPN-Lite)
- YOLOv5 (referred to as YOLO)

During my initial rounds of testing, I spent most of my time testing the FOMO 0.35 and FPN-Lite models as those gave the best results during my first few comparisons between models. The FPN-Lite model even achieved the highest testing accuracy throughout all of my experimentation (84.19%).

I soon realized, however, that in order to deploy onto a constrained device such as the Arduino Nano 33 BLE Sense, the only model I could deploy from Edge Impulse was FOMO-based model. Due to this, I conducted further rounds of testing with the FOMO 0.35 and FOMO 0.1 models. Due to the inability of deployment with the other models, I've omitted most of their experimentation and results from this report and its diagrams, although highlights from these tests can be see in the Observations & Results section under Model & Experimentation in Figure x.

## Experiments

### Methodology
The methodology used was quite simple, control all but a single variable between experiments in order to understand the impact of that variable on the testing accuracy of each model. Edge Impulse allows a user to specify 3 main variables during training:

- Model
- Number of Training Cycles (Epochs)
- Learning Rate (How much the models internal parameters are updated during each step of the training proces)

Upon realizing I was only able to deploy a FOMO model from Edge Impulse onto a constrained device, I entered my final rounds of experimentation with FOMO 0.35 and FOMO 0.1. I decided to change my image resolution from 320 x 320 to 96 x 96 as it is the smallest Edge Impulse recommended size for object detection and smaller images will allow for quicker training. The increase in training speed also allowed me to implement far more training cycles than I were previously possible within Edge Impulse's 20 minute training timeframe for standard users. On both the FOMO 0.35 and FOMO 0.1 models, I tested each of 6 different training cycle amounts with 3 different learning rates, and evaluated the models based on their accuracy against the testing data. The results of these tests can be seen in Figures 9 and 10.

IMAGE

Figure 9 - Chart of Tests, FOMO MobileNetV2 0.35

IMAGE

Figure 10 - Chart of Tests, FOMO MobileNetV2 0.1 


## Results and Observations
Synthesis the main results and observations you made from building the project. Did it work perfectly? Why not? What worked and what didn't? Why? What would you do next if you had more time?

### Data Collection
More images collected from different angles, times of day, weather conditions, and scenery may increase the usability of the application. Images taken from the specific angle in which the camera will be looking at bicycles would be most beneficial.

### Image Processing
Much of the image processing I conducted prior to uploading my images to Edge Impulse can be handled by the platform itself, such as greyscaling and cropping or resizing images. Knowing this will drastically improve my future image processing practices. Additionally, this ability allowed me to quickly alter image characteristics between experiments, allowing me to quckly test 256x256, 320x320, and 96x96 resolutions without needing to reupload any data.

### Model & Experimentation
Throughout the majority of my initial tests, the training accuracy of my model was 23.72%. I quickly realized that this number correlated exactly with the percentage of testing images that did not contain any bicycles. Therefore, for the majority of these tests, my model was unable to detect any bicycles.

After switching from FOMO to FPN-Lite, there was a huge improvement in the accuracy of bicycle detection. I assume this was due to the sheer size of the FPN-Lite model, which consequently also made it unsuitable for deployment onto an Arduino Nano 33 BLE Sense. Some Highlights of these tests can be seen in Figure x.

![exp1](https://user-images.githubusercontent.com/33913141/234832560-f120b9e6-9baa-45cf-84e9-aef10d4f779a.png)

Figure x - Highlights of Initial Experimentation, FOMO MobileNetV2 0.35 and MobileNetV2 SSD FPN-Lite 320x320

Upon returning to the FOMO model, and conducting some research, I decided to resize my images in my impulse design to 96x96 as this is another size recommended by Edge Impulse for FOMO. This also resulted in a significant improvement in my model's accuracy. I believe this was due to an improvement in the speed of training which allowed me to run more epochs.

Figure x - Graph of Results from Final Experimentation, FOMO MobileNetV2 0.35 and FOMO MobileNetV2 0.1

### Deployment
I deployed my final model - [insert model specifications] - onto an Arduin Nano 33 BLE Sense, with an OV7675 CMOS VGA Camera Module as an input device. The system was deployed with the TinyML Sheild included in Arduino's TinyML Kit. The full build can be seen in Figure - x.

![dep1](https://user-images.githubusercontent.com/33913141/234854412-76a4a325-e05d-4a26-bf13-666e3564640f.png)

Figure x - Final Build

xx
images of edge device and deployment video stream?
Images of deployment, and detection?


## To Do
- research on the changes i made, did the changes fix overfitting? reducing resolution help?
- add info about edge impulse

## Bibliography

Moreau, L. (2022) Announcing Fomo (faster objects, more objects), Edge Impulse. Available at: https://www.edgeimpulse.com/blog/announcing-fomo-faster-objects-more-objects#:~:text=FOMO%20can%20be%20thought%20of,all%20of%20a%20similar%20size. (Accessed: April 26, 2023).

Dickson, B. et al. (2022) Fomo is a tinyml neural network for real-time object detection, TechTalks. Available at: https://bdtechtalks.com/2022/04/18/fomo-tinyml-object-detection/ (Accessed: April 26, 2023). 

Kanan, C. and Cottrell, G.W. (2012) Color-to-grayscale: Does the method matter in image recognition?, PLOS ONE. Public Library of Science. Available at: https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0029740#:~:text=The%20main%20reason%20why%20grayscale,algorithm%20and%20reduces%20computational%20requirements. (Accessed: April 26, 2023).

Isahit (2022) What is the purpose of image preprocessing in deep learning?, Isahit. isahit. Available at: https://www.isahit.com/blog/what-is-the-purpose-of-image-preprocessing-in-deep-learning (Accessed: April 26, 2023). 

## Declaration of Authorship
I, André Bourgeois, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

André Bourgeois

21/04/2023
