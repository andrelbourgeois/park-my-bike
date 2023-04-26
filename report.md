# PARK MY BIKE
Author: André Bourgeois

Project: [Github Repository](https://github.com/andrelbourgeois/park-my-bike) and [Edge Impulse Repository](https://studio.edgeimpulse.com/public/201800/latest)

## Introduction
This project details a study undertaken to understand whether a camera placed at a bicycle bay could be used to remotely inform cyclists of available parking spaces at that bicycle bay. In order to accomplish this, a deep learning model was developed and trained to detect bicycles using [Edge Impulse](https://www.edgeimpulse.com/). The idea being, that if the number of bicycles can be accurately obtained at a specific bicycle bay and there is previous knowledge regarding the number of parking spaces at that bicycle bay, then the difference between these numbers is the number of available parking spaces. This number can then be pushed - along with the device's location - to a popular mapping application for public use. The aim of this project, however, is not the design of this entire system, but instead the design and deployment of this system’s deep learning capability. Figure 1 provides a more tangible idea of what the whole system might look like in practice.

![park-my-bike-diagram](https://user-images.githubusercontent.com/33913141/232341366-51ec127c-757a-477a-b7ca-77f8858b443d.png)

Figure 1 - Example of Park My Bike Deployment

The motivation for this project is twofold. As a cyclist myself, my friends and I understand the difficulty of locating bicycle parking in a large city. There are many available spaces throughout the city, but they can be difficult to find and are often filled in popular areas. Having the ability to both locate traditional bicycle bays and know whether they have available parking spaces would significantly reduce the friction associated with cycling to new destinations in London.
Additionally, a similar system is already in use by London's self-service bike-sharing scheme, Santander Cycles. Although this scheme is enabled through a system of docking stations throughout the city instead of computer vision, the end result is the same - live data pushed to your favourite wayfinding app that shows cyclists the location and availability of bicycles or parking at the Santander Cycle stations throughout the city. This feature is shown in Figure 2.

![santander-ex](https://user-images.githubusercontent.com/33913141/232324525-efa49797-fa02-4039-96cb-835080c791ce.png)

Figure 2 - Example of Santander Bicycle Sharing Stations on Google Maps

## Research Question
Can a camera deployed near a bicycle bay accurately detect the number of bicycles parked at that bicycle bay, and therefore, help in determining the number of available parking spaces?

## Application Overview
The purpose of this application is to identify bicycles. The image input relies on the edge device's camera - in this case, the OV7675 CMOS VGA Camera Module connected to the Arduino Nano 33 BLE Sense. While images are being captured through a real-time video feed, they are each being processed and split into a grid where the equivalent of image classification is ran across all cells in the grid independently in parallel. (Moreau, 2022) A depiction of this processing stage can be seen in Figure 4. Following this processing, the model searches the image for features similar to those it was trained on - bicycles - and makes a decision as to whether or not there are any bikes in the image. With the FOMO model, an affirmative decision is depicted as a centroid marker on the image, because instead of predictin bounding boxes, FOMO predicts the objects centre (Dickson et al., 2022). An example of an image with a bounding box next to the same image with a centroid marker can be seen in Figure 5.

![application-overview](https://user-images.githubusercontent.com/33913141/234704224-94128693-fd25-4e0f-9a65-26ac27104619.png)

Figure 3 - Application Diagram

![example-image-proc](https://user-images.githubusercontent.com/33913141/234701976-f93cd07f-6dae-4cc7-a8ab-ab9fbe141bd7.png)

Figure 4 - Example of FOMO Image Processing; 320x320 Image Split Into a 40x40 Grid (Moreau, 2022)

![box-vs-cent](https://user-images.githubusercontent.com/33913141/234717829-0de49c8a-d4c6-4827-9785-05001ed3d35e.png)

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

In total, the combined dataset that was used contained 1441 images, similar to those shown in the figures directly above.

In order to label each image, I manually drew bounding boxes around any bicycles using the Edge Impulse platform. See Figure 8 for examples.

![label-ex1](https://user-images.githubusercontent.com/33913141/234707499-47352fff-d8ae-479a-a370-168cf782c9f8.png)
&nbsp;&nbsp;&nbsp;
![label-ex2](https://user-images.githubusercontent.com/33913141/234707532-adf97aa0-1b5a-49d7-8980-12a19b246e92.png)
&nbsp;&nbsp;&nbsp;
![label-ex3](https://user-images.githubusercontent.com/33913141/234707555-f20e5f8f-4b67-478e-89be-784cebb60a7c.png)

Figure 8 - Example Image Labelling with Bounding Boxes

Decisions regarding the processing of these images are as follows:
- Greyscaling; The FOMO model used currently only accepts greyscaled images and greyscaling simplifies the alogrithm and reduces computational requirements (Kanan & Cottrell, 2012) by reducing colour variable down to a single number between 0 - 255.
- Squaring; The FOMO model used currently only accepts square images and it was a common standard dimension within all of the research I conducted as many model topologies require square input images (Isahit, 2022). 
- Tiling; Allowed me to artificially increase the size of the dataset by augmenting these images.

## Model
This is a Deep Learning project! What model architecture did you use? Did you try different ones? Why did you choose the ones you did?
In this project, I tested 3 model architectures which represented a selection of all model architectures on the Edge Impulse platform.

- FOMO MobileNetV2 0.35 (referred to as FOMO)
- MobileNetV2 SSD FPN-Lite 320x320 (referred to as FPN-Lite)
- YOLOv5 (referred to as YOLO)

I spent most of my time testing the FOMO and FPN-Lite models as those gave the best results during my first few tests. The FPN-Lite model even going on to achieve the highest accuracy throughout all of my experimentation (84.19%). However, I soon realized that in order to deploy onto a constrained device such as the Arduino Nano 33 BLE Sense, the only model I could deploy from Edge Impulse was one built with FOMO.

For this project, I tested several different models including YOLOv5, FOMO, and MobileNet SSD.
YOLO vs FOMO vs 

Tip: probably ~200 words and a diagram is usually good to describe your model!

## Experiments
What experiments did you run to test your project? What parameters did you change? How did you measure performance? Did you write any scripts to evaluate performance? Did you use any tools to evaluate performance? Do you have graphs of results?

![experiments](https://user-images.githubusercontent.com/33913141/233605076-8015e691-b550-467e-97f8-49312fb2e6e5.png)

Figure x - Chart of Notable Experiments and Results

i realized a lot of the processing could be done on edge impulse, blah blah

Tip: probably ~300 words and graphs and tables are usually good to convey your results!

## Results and Observations
Synthesis the main results and observations you made from building the project. Did it work perfectly? Why not? What worked and what didn't? Why? What would you do next if you had more time?

Tip: probably ~300 words and remember images and diagrams bring results to life!

## To Do
- research on different models, why did i use the models i used
- research on data processing and cleaning, why did i make things grey, why did i scale them, tiling, etc
- research on the changes i made, did the changes fix overfitting? reducing resolution help?
- add info about edge impulse
- add references

## Bibliography

Moreau, L. (2022) Announcing Fomo (faster objects, more objects), Edge Impulse. Available at: https://www.edgeimpulse.com/blog/announcing-fomo-faster-objects-more-objects#:~:text=FOMO%20can%20be%20thought%20of,all%20of%20a%20similar%20size. (Accessed: April 26, 2023).

Dickson, B. et al. (2022) Fomo is a tinyml neural network for real-time object detection, TechTalks. Available at: https://bdtechtalks.com/2022/04/18/fomo-tinyml-object-detection/ (Accessed: April 26, 2023). 

Kanan, C. and Cottrell, G.W. (2012) Color-to-grayscale: Does the method matter in image recognition?, PLOS ONE. Public Library of Science. Available at: https://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0029740#:~:text=The%20main%20reason%20why%20grayscale,algorithm%20and%20reduces%20computational%20requirements. (Accessed: April 26, 2023).

Isahit (2022) What is the purpose of image preprocessing in deep learning?, Isahit. isahit. Available at: https://www.isahit.com/blog/what-is-the-purpose-of-image-preprocessing-in-deep-learning (Accessed: April 26, 2023). 

Tip: we use https://www.citethisforme.com to make this task even easier.

## Declaration of Authorship
I, André Bourgeois, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

André Bourgeois

21/04/2023
