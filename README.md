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


## 2. The objective of the project
- Build a cluster of machines
- Build an image processing flow
- Delegate work for the machines using a distributed storage and processing framework
- Orchestrate the communication between the machines using a central server
- Build edge computing node that transfers data to cluster

## 3. Scenario
<suggestions - where and why apply image classification/processing with edge node>
common/commercially available example (may not want to use this but just as example) - home security, identify/classify person at door is authorized for entry.

idea1 - automated farming, chillie farms with automated robots that capture images of each chillie and classify as ready/not ready for picking. 
idea2 - remote environment monitoring, ie: monitor changing terrain to identify soil movement
idea3 - 

## 3. Methodology
![alt text](https://github.com/shienlong/parallel/blob/master/Archi04.PNG?raw=true)

**Phase 1: Data Acquisition**

- what type of data, format of data
- where obtain data from

https://projects.raspberrypi.org/en/projects/getting-started-with-picamera
https://www.raspberrypi.org/products/camera-module-v2/

We are simulating a camera that captures images and stores it in a folder (/home/pi/Pictures/NewImg). The camera will be Raspberry Pi Camera Module which uses the Sony IMX219. The sensor on the Sony IMX219 is a 8-megapixel camera which performs very well in low light. The module is connected to a Raspberry Pi (Edge Pi) via ribbon cable. 

**Phase 2: Data Transfer**

- The Edge Pi uses Node-RED to watch for changes in the folder that stores images captured by the camera module.
- When there is change in folder, Node-RED takes the message payload and using SSH transfer the newly added image to Pi@Master (/home/pi/Desktop) 
- This flow (enabled by Node-RED) automates the process of detecting new images in the source folder and transfer the delta via SSH protocol to the Pi@Master. 

**Phase 3: Data Processing**

- Two core components of Apache Hadoop are Hadoop Distributed File System (HDFS) and MapReduce
- Because we're dealing with images, we're using an open source image processing library: Hadoop Image Processing Interface (HIPI) which is designed to be used with Hadoop
- Once the data is ready, Pi@Master would then start doing the processing:
  - The images would be represented as HipiImageBundle (HIB) format. A HIB is a collection of images represented as a single file on the HDFS.
  - HIB would then run through the MapReduce flow...
  - ...

**Phase 4: Result**

- what is the output


## 4. Explanation of all the components being involved in the project. Adjustment of why these components are being used.

### Wireless Router
![alt text](https://i.imgur.com/QoA1Uq0.jpg?raw=true)

- Serve as a router and wireless access point
- Provide private wireless network
- Provide internet access to all the machines
- Why:
  - The main advantage of using private wireless network in this setup is we can maintain the static internal IPs of the machines at all time and avoid dealing with connectivity issues
  - Do not have to fiddle with wiring Ethernet cables

### Virtual Network Computing
![alt text](https://images.ctfassets.net/tvfg2m04ppj4/6ojbkPv0fLwYRLd6CQJynD/595d0f15453ca6a603f029bd51a5690d/main_image.png?w=800)

- A GUI software that uses Remote Frame Buffer Protocol (RFB) to control other computers by transmitting all the mouse and keyboard events from the origin (VNC Server) to the target (VNC Client)
- Why:
  - Make it easier to interact with display-less computers like Raspberry Pi
  - Monitoring connectivity and system logs in each Raspberry Pi

### Laptop (Windows 10 machine)
![alt text](https://i.gadgets360cdn.com/large/windows_10_screenshot_laptop_1532433667290.jpg?raw=true)

- Why:
  - Act as the VNC server
  - Act as the display unit to access VNC GUI and Node-RED Flow interface

### Raspberry Pi 4 (Model B)
![alt text](https://whooptous.com/wp-content/uploads/2019/06/Raspberry-Pi-4-model-B.png?raw=true)

- Low-cost compute unit
- Wifi support
- Come with Raspbian OS (Linux) easier to run applications like Hadoop and Node-RED
- 4GB SDRAM
- Why:
  - Running Hadoop framework (both namenode/datanode)
  - Act as a VNC client
  - Edge computing node

### Node-RED
![alt text](https://upload.wikimedia.org/wikipedia/commons/2/2b/Node-red-icon.png?raw=true)

**About**

- Open source tool developed by IBM Emerging Technology
- It's a flow-based programming tool to wiring together hardwares devices, APIs and conected services

**Why:**

  - Comes with a very friendly browser-based flow editing
  - Rich ecosystem of flows that are shared by the community
  - Light-weight runtime (built on Node.js), so it's ideal for Raspberry Pi 
  
- http://192.168.0.198:1880
- http://192.168.0.198:1880/ui

### Node-RED flow programmed to detect new image and copy to master via SSH
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_ImageSSH.PNG?raw=true)

The "Watch NewImg Folder" node watches for new image to be added into "NewImg" folder, then an execute node will execute a "scp" secure copy command via EdgePi's terminal to transfer the image which is in form of message payload buffer. The "scp" command is "scp...", it performs a secure copy of the new image file and transfer via SSH to the "TempImg" folder in Pi@Master. 

Node-RED flow to monitor EdgePi CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_TempFlow.PNG?raw=true)

Dashboard to show CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_Dashboard_Temp.PNG?raw=true)

### Apache Hadoop
![alt text](https://static1.tothenew.com/blog/wp-content/uploads/2016/11/hadoop.png?raw=true)

- 

### Hadoop Image Processing Interface (HIPI)
![alt text](http://hipi.cs.virginia.edu/images/hipi_pipeline.png?raw=true)

## 5. Results (visualization) and discussion about findings. here you should also include a roadmap of each objective that has been addressed.


## 6. Conclusion

## 7. References
