# PARK-MY-BIKE
This repository contains an object detection model that can be deployed to an edge device to identify bicycles at bicycle bays. The model was designed on Edge Impulse and originally based on the MobileNetV2 SSD FPN-Lite 320x320 model architecture, however, this was later changed to the FOMO MobileNetV2 0.35 model architecture due to memory constraints resulting in the inability of deploying the original model onto an Arduino Nano 33 BLE Sense. The MobileNetV2 SSD FPN-Lite 320x320 works wonderfully on a smart phone, however, smart phone deployment does not fit the needs of this project.

The experimentation and training for this project were conducted with a combined dataset of open-source images [found here](https://images.cv/dataset/bicycle-image-classification-dataset) and custom images sourced from around London, UK.

This project is the final submission for CASA0018 (Deep Learning for Sensor Networks) in the MSc in Connected Environments at UCL's Bartlett Centre for Advanced Spatial Analysis.

## Project Directory
- My final report can be found in [report.md](https://github.com/andrelbourgeois/park-my-bike/blob/main/report.md)
- A project demonstration can be found at this [link]()
- Licensing information can be found in LICENSE.md

## Software
- [Edge Impulse](https://edgeimpulse.com/)
- [ImageMagick](https://imagemagick.org/index.php)

## Hardware
- Arduino TinyML Kit
  - Arduino Nano 33 BLE Sense
  - OV7675 CMOS VGA Camera Module
  - TinyML Shield
  - USB A - Micro USB Cable
  
  ![tinyml-kit](https://user-images.githubusercontent.com/33913141/234853153-b601a9f7-b65b-466e-925c-9dcbc4fd51ea.png)

## License
This project is licensed under the MIT License.

## FAQ
Q: How accurate is the model?
A: The model has an accuracy of [accuracy] while under ideal conditions - good lighting, angle, etc.

Q: Can the model detect other objects besides bicycles?
A: The model was trained specifically to detect bicycles and will not be accurate at identifying other objects.

Q: What edge devices is the model compatible with?
A: The model has been tested on an Arduino Nano 33 BLE Sense using a OV7675 CMOS VGA Camera Module, but may be compatible with other edge devices.
