# WQD7008 Parallel and Distributed Computing Project

## Project Title
Image Processing with Hadoop in Raspberry Pi Cluster

## Team Members
- WQD180041 - MUHAMMAD HAFIZ BIN KHAIRUDIN
- WQD190039 - JONATHAN KOW YEE SENG
- WQD170100 - SYAIFUL ANUAR BIN ABD LATIF
- WQD180061 - XUXIANG
- WQD180027 - LIM SHIEN LONG

## 1. Introduction


## 2. The objective of the project (Hafiz)
- Build a cluster of machines
- Delegate work for the machines using a distributed storage and processing framework
- Image processing with distributed framework
- Orchestrate the communication between the machines using a central server
- Build edge computing node that transfers data to cluster

## 3. Scenario
<suggestions - where and why apply image classification/processing with edge node>
common/commercially available example (may not want to use this but just as example) - home security, identify/classify person at door is authorized for entry.

idea1 - automated farming, chillie farms with automated robots that capture images of each chillie and classify as ready/not ready for picking. 
idea2 - remote environment monitoring, ie: monitor changing terrain to identify soil movement
idea3 - 

## 3. Methodology
![alt text](https://github.com/shienlong/parallel/blob/master/Archi04.PNG)


Phase 1: Data Acquisition

- what type of data, format of data
- where obtain data from

https://projects.raspberrypi.org/en/projects/getting-started-with-picamera
https://www.raspberrypi.org/products/camera-module-v2/

We are simulating a camera that captures images and stores it in a folder (/home/pi/Pictures/NewImg). The camera will be Raspberry Pi Camera Module which uses the Sony IMX219. The sensor on the Sony IMX219 is a 8-megapixel camera which performs very well in low light. The module is connected to a Raspberry Pi (Edge Pi) via ribbon cable. 

Phase 2: Edge Pi

- what is happening in this phase...
- The Edge Pi uses Node-RED to watch for changes in the folder that stores images captured by camera module. 
- When there is change in folder, Node-RED takes the message payload and using SSH transfer the newly added image to RP master (/home/pi/Desktop) 
- This flow (enabled by Node-RED) automates the process to detect new images and transfer via ssh to master. 

Phase 3: HDFS (consist of namenode & datanode)

- what is happening in this phase...

Phase 4: Visualization

- what is the output


## 4. Explanation of all the components being involved in the project. Adjustment of why these components are being used.

### Wireless Router
- Serve as a router and wireless access point
- Provide private wireless network
- Provide internet access to all the machines
- Why:
  - The main advantage of using private wireless network in this setup is we can maintain the internal IPs of the machines at all time and avoid dealing with connectivity issues
  - Do not have to fiddle with wiring Ethernet cables 

### Virtual Network Computing
- A GUI software that uses Remote Frame Buffer Protocol (RFB) to control other computers by transmitting all the mouse and keyboard events from the origin (VNC Server) to the target (VNC Client)
- Why:
  - Make it easier to interact with display-less computers like Raspberry Pi
  - Monitoring connectivity and system logs in each Raspberry Pi

### Laptop (Windows 10 machine)
- Why:
  - Act as the VNC server
  - Act as the display unit to access VNC GUI and Node-RED interface

### Raspberry Pi 4 (Model B)
- Low-cost compute unit
- Wifi support
- Come with Raspbian OS (Linux) easier to run applications like Hadoop and Node-RED
- 4GB SDRAM
- Why:
  - Running Hadoop framework (both namenode/datanode)
  - Act as a VNC client
  - Edge computing node

### Node-RED
- Open source tool developed by IBM Emerging Technology
- It's a flow-based programming tool to wiring together hardwares devices, APIs and conected services
- Why:
  - Comes with a very friendly browser-based flow editing
  - Rich ecosystem of flows that are shared by the community
  - Light-weight runtime (built on Node.js), so it's ideal for Raspberry Pi 
  
- http://192.168.0.198:1880
- http://192.168.0.198:1880/ui

Node-RED flow programmed to detect new image and copy to master via SSH
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_ImageSSH.PNG)

Node-RED flow to monitor EdgePi CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_TempFlow.PNG)

Dashboard to show CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_Dashboard_Temp.PNG)


## 5. Results (visualization) and discussion about findings. here you should also include a roadmap of each objective that has been addressed.


## 6. Conclusion

## 7. References
