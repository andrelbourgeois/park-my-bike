# PARK-MY-BIKE
This repository contains an object detection model that can be deployed to an edge device to identify bicycles at bicycle bays. The model was originally based on the MobileNetV2 SSD FPN-Lite 320x320 architecture, however, this was later changed to the MobileNetV2 0.35 architecture due to memory constraints resulting in the inability of deploying the original model onto an Arduino Nano 33 BLE Sense. The MobileNetV2 SSD FPN-Lite 320x320 works wonderfully on a smart phone, however, smart phone deployment does not fit the needs of this project. This model was trained on a combined dataset of open-source images [found here](https://images.cv/dataset/bicycle-image-classification-dataset) and custom images sourced from around London, UK.

This project is the final submission for CASA0018 (Deep Learning for Sensor Networks) in the MSc in Connected Environments at UCL's Bartlett Centre for Advanced Spatial Analysis.

## Project Directory
- My final report can be found in the folder titled report.
- The code for this project can be found in the doler titled code.
- A project demonstration can be found at this link.
- Licensing information can be found in LICENSE.md.

## Softwares Used
- [Edge Impulse](https://edgeimpulse.com/)
- [ImageMagick](https://imagemagick.org/index.php)

## Installation
x

## Usage
x

## Contributing
I welcome contributions to this project! To contribute, please follow these steps:

- Fork this repository to your own GitHub account.
- Create a new branch with your changes.
- Submit a pull request with a clear description of your changes.

## License
This project is licensed under the MIT License.

## FAQ
Q: How accurate is the model?
A: The modes were trained on a custom dataset of bicycle images and had a maximum accuracy of 85% while using the MobileNetV2 SSD FPN-Lite 320x320 architecture, however, this model was too large to deploy onto the Arduinio Nano 33 BLE Sense. Instead, the deployed model uses the FOMO (Faster Objects, More Objects) MobileNetV2 0.1 architecture which is officially supported by Edge Impulse. Thise models is designed to be <100KB in size and support a grayscale input at any resolution.

Q: Can the model detect other objects besides bicycles?
A: The model was trained specifically to detect bicycles and may not be accurate at identifying other objects.

Q: What edge devices is the model compatible with?
A: The model has been tested on Raspberry Pi 4 using a Pi noIR camera???????, but may be compatible with other edge devices.

## To Do List
- finish project
- write report
- complete readme


----------------------------------------------------------------------------------
reference...

Installation
To install the model and its dependencies, follow these steps:

Clone this repository to your local machine.
Install the required dependencies by running the following command:
bash
Copy code
pip install -r requirements.txt
Usage
To use the model, follow these steps:

Connect your edge device to a bicycle bay.
Run the following command to start the object detection script:
bash
Copy code
python detect_bicycles.py
The script will capture an image of the bicycle bay and use the object detection model to identify any bicycles present.
The script will output the number of bicycles detected and their corresponding bounding boxes.
