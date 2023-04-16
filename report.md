# PARK MY BIKE
Author: André Bourgeois
Project: Github Repository and Edge Impulse Repository (Add links)

## Introduction
This project details a study undertaken to understand whether a camera placed at a bicycle bay could be used to remotely inform cyclists of available parking spaces at that bicycle bay. In order to accomplish this, a deep learning model was developed and trained to detect bicycles. The idea being, that if the number of bicycles can be accurately obtained at a specific bicycle bay and there is previous knowledge regarding the number of parking spaces at that bicycle bay, then the difference between these numbers is the number of available parking spaces. This number can then be pushed - along with the device's location - to a popular mapping application for public use. The aim of this project, however, is not the design of this entire system, but instead the design and deployment of this system’s deep learning capability. Figure 1, below, provides a more tangible idea of what this system might look like in practice.

![park-my-bike-diagram](https://user-images.githubusercontent.com/33913141/232341366-51ec127c-757a-477a-b7ca-77f8858b443d.png)

Figure 1 - Example of Park My Bike Deployment

The motivation for this project is twofold. As a cyclist myself, my friends and I understand the difficulty with locating bicycle parking in a large city. There are many available spaces throughout the city, but they can be difficult to find and are often filled at popular locations. Having the ability to both locate traditional bicycle bays and know whether they have available parking spaces would significantly reduce the friction associated with cycling to new destinations in London.
Additionally, a similar system is already in use by London's self-service bike-sharing scheme, Santander Cycles. Although this scheme is enabled through a system of docking stations throughout the city instead of computer vision, the end result is the same - live data pushed to your favourite wayfinding app that shows a cyclist the location and availability of bicycles or parking at the Santander Cycle stations throughout the city. This feature is shown, below, in Figure 2.

![santander-ex](https://user-images.githubusercontent.com/33913141/232324525-efa49797-fa02-4039-96cb-835080c791ce.png)

Figure 2 - Example of Santander Bicycle Sharing Stations on Google Maps

## Research Question
Can a camera deployed near a bicycle bay accurately detect the number of bicycles parked at that bicycle bay, and therefore, help in determining the number of available parking spaces?

## Application Overview
Thinking back to the various application diagrams you have seen through the module - how would you describe an overview of the building blocks of your project - how do they connect, what do the component parts include.

Tip: probably ~200 words and a diagram is usually good to convey your design!

## Data
The data utilised for this project combined open-source, online images and a dataset of custom images taken around London, UK.

The online images were retrieved in a single dataset from images.cv - a website offering open-source, labelled image datasets. This [dataset](https://images.cv/dataset/bicycle-image-classification-dataset) contained 705 images of bicycles, bike-related objects, and images that could be labelled as bicycle, such as bicycle kicks and bicycle playing cards. The website provides the ability to download this dataset in a range of sizes, with options for colour. It also provides the option to divide the images into folders for training, validation, and testing. With these features and after some pruning of images I didn't believe to be useful, I was left with 593, greyscaled images at a size of 256px x 256px. Example images can be seen below in Figure x.

![gray-2WZF8BLTOBMY](https://user-images.githubusercontent.com/33913141/232325511-8c5b96da-0467-4f46-a7d1-f5ee82f54ce9.jpg)
![gray-2EVEA5H88OEQ](https://user-images.githubusercontent.com/33913141/232341993-81e1c81e-aca3-4646-a54a-84552117d84b.jpg)
![gray-2VARMD3C8MZF](https://user-images.githubusercontent.com/33913141/232342052-bd7e1d77-40b9-400e-a0b6-73148df6c06e.jpg)

Figure x - Example Images from Image.cv Dataset

The custom images were taken by myself, over the course of two weeks throughout my daily commutes in London. These Images began as 3024px × 4032px coloured photos. In order to process them, I first recoloured them and cropped them into 3024px x 3024px squares. Next, I reduced their resolution to 1024px x 1024px using the [ImageMagick](https://imagemagick.org/index.php) app. Finally, I split each image into 16 separate images to increase the size of the dataset and get a good mix of images that contained bicycles and images with scenery around the bicycles. This process resulted in 848 greyscaled images at a size of 256px x 256px. Figure x, below, demonstrates the image processing.

![tiled-example](https://user-images.githubusercontent.com/33913141/232341346-e35a9ab2-1f36-45c1-84aa-d26142a5de61.png)

Figure x - Example Image Processing for Custom Data

In total, the combined dataset that was used contained 1441 images, similar to those shown in the previous figures.

## Model
This is a Deep Learning project! What model architecture did you use? Did you try different ones? Why did you choose the ones you did?

Tip: probably ~200 words and a diagram is usually good to describe your model!

## Experiments
What experiments did you run to test your project? What parameters did you change? How did you measure performance? Did you write any scripts to evaluate performance? Did you use any tools to evaluate performance? Do you have graphs of results?

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
If you added any references then add them in here using this format:

Last name, First initial. (Year published). Title. Edition. (Only include the edition if it is not the first edition) City published: Publisher, Page(s). http://google.com

Last name, First initial. (Year published). Title. Edition. (Only include the edition if it is not the first edition) City published: Publisher, Page(s). http://google.com

Tip: we use https://www.citethisforme.com to make this task even easier.

## Declaration of Authorship
I, André Bourgeois, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

André Bourgeois

21/04/2023
