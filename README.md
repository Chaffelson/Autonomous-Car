# Autonomous Car

## Overview

Autonomous Car is an Open Source Self Driving Vehicle. This car is powered by 3 Cameras, LiDAR and Logitech Game Controller connected to the Jetson TX2 Board. ROS runs on the TX2 and controls the car's movement. Keras is used on the TX2 or on CDSW in the cloud for building, training and deploying a CNN model based on cloning driving behavior. Hadoop HDFS is used to store all the car data and save the trained CNN model. When the CNN model is deployed, ROS receives a constant speed of 15mph while streaming in center camera images and receiving a predicted steering angle for each image. Hence, the car drives autonomously on the custom track.

![james_with_car.jpg](documentation/assets/images/readme/james_with_car.jpg)

![controller.jpg](documentation/assets/images/readme/controller.jpg)

In this tutorial, you will build an Edge to AI application featuring CDF and CDSW.

## Big Data Technologies used to develop the Application:

- Nvidia Jetson TX2
    - [Apache MiNiFi](https://nifi.apache.org/minifi/) C++ Agent
- CDF
    - [CEM](https://docs.hortonworks.com/HDPDocuments/CEM/CEM-1.0.0/index.html): Interactive UI for building MiNiFi dataflows
    - CFM: [Apache NiFi](https://nifi.apache.org/)
- CDH
    - [Apache Hadoop](https://hadoop.apache.org/) - HDFS
    - [CDSW](https://www.cloudera.com/products/data-science-and-engineering/data-science-workbench.html): IDE for Machine Learning

## Bill of Materials

- [Nvidia Jetson TX2 Development Kit](https://www.amazon.com/NVIDIA-945-82771-0000-000-Jetson-TX2-Development/dp/B06XPFH939)
- [Racecar/J Robot Base Kit](https://racecarj.com/products/racecar-j-robot-base-kit)

## Tutorial Series

[Autonomous Car Tutorial](https://github.com/Chaffelson/Autonomous-Car/blob/master/tutorial-0.md)
