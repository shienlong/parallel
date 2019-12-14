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
The project is suitable to be deployed to an array of industies which requires monitoring or collection of information in large facilities or a geographically expansive area. 

An example is you own a large factory producing and packaging consumer products. The process starts with raw material processing at one end of the factory feeding machines and conveyer systems throughout the length of factory floor with some systems up to 2 storey in height. Periodic maintenance is key to ensuring optimum working condition for maximum output. 

However, the rising labour cost and high attrition rate due to working environment is affecting maintenance schedule and effectiveness. Traditional monitoring systems are cumbersome and expensive as they need to be wired throughout the whole factory. Therefore a system where various edge sensors that are able to relay critical information wirelessly to a storage and processing node would be an elegant and cost effective solution. Given the number of machines, processes and monitoring required, a huge amount of data is generated, therefore having an off the shelf distributed storage and processing solution (ie: Raspberry Pi Hadoop Cluster), could be a very attractive proposition.  

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
  - HIB would then run through the MapReduce flow
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
**Installation Steps:**

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

To make sure the Node-RED service can autostart on Raspberry Pi boot, add this command as well:

```
sudo systemctl enable nodered.service
```

**About**

- Open source tool developed by IBM Emerging Technology
- It's a flow-based programming tool to wiring together hardwares devices, APIs and conected services

**Why:**

  - Comes with a very friendly browser-based flow editing
  - Rich ecosystem of flows that are shared by the community
  - Light-weight runtime (built on Node.js), so it's ideal for Raspberry Pi 
  
- http://192.168.0.198:1880
- http://192.168.0.198:1880/ui

#### Node-RED flow programmed to detect new image and copy to master via SSH
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_ImageSSH02.PNG?raw=true)

The "Watch NewImg Folder" node watches for new image to be added into "NewImg" folder, then an execute node will execute a "scp" secure copy command via EdgePi's terminal to transfer the image which is in form of message payload buffer. The "scp" command is "scp...", it performs a secure copy of the new image file and transfer via SSH to the "TempImg" folder in Pi@Master.

A "file" node in parallel takes the output message payload and converts it to object buffer. That buffer is converted to base64 encoding and passed to dashboard to display image. 

The dashboard UI can be viewed via:
http://192.168.0.198:1880/ui

or with internet connection, using DNS nameserver (a username and password is required to secure EdgePi as this method is using portforwarding):
http://nodered7008.duckdns.org:1880/ui

#### Node-RED flow to monitor EdgePi CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_TempFlow02.PNG?raw=true)

The EdgePi deployed into the environment will be subjected to temperature variation of the environment. A separate flow is created to obtain EdgePi CPU and GPU temperature info and display on the dashboard for monitoring. 

The flow starts with "timestamp" node where a repeat loop with interval of 1s is enabled. Then "execute" nodes are created to input commands for CPU and GPU respectively. 

*CPU command, "cat /sys/class/thermal/thermal_zone0/temp"
*GPU command, "vcgencmd measure_temp"

The outputs of the execute nodes are then passed through functions to extract temperature info. The info is the updated to the dashboard. 

#### Dashboard to show EdgePi CPU and GPU temperature
![alt text](https://github.com/shienlong/parallel/blob/master/NodeRed_Dashboard_Temp02.PNG?raw=true)

The simple dashboard displays the temperature gauge of GPU and CPU as well as the new image that was detected in the folder. 

### Apache Hadoop
![alt text](https://static1.tothenew.com/blog/wp-content/uploads/2016/11/hadoop.png?raw=true)

**Installation Steps:**
(refer to Jonathan's step)

**Why**
-

### Hadoop Image Processing Interface (HIPI)
![alt text](http://hipi.cs.virginia.edu/images/hipi_pipeline.png?raw=true)

A framework that takes image files as input, converts it to a HIPI Image Bundle (.hib) data type. There are two files after conversion, the file ending with ".hib.dat" is the data file containing images and 2nd file with ".hib" which is the index along with information of the offsets of each image in the data file. 

Next, the HIPI framwork would then take converted files and behind the scenes will perform distribution of the image bundle across the map nodes. The images will then go through a culling stage before being fed as input to map tasks (Apache Hadoop). 

**Installation Steps:**

```
$ git clone git@github.com:uvagfx/hipi.git
$ cd hipi
$ gradle
:core:compileJava
:core:processResources
:core:classes
:core:jar
:tools:downloader:compileJava
:tools:downloader:processResources
:tools:downloader:classes
:tools:downloader:jar
:tools:dumpHib:compileJava
:tools:dumpHib:processResources
:tools:dumpHib:classes
:tools:dumpHib:jar
...
:install

Finished building the HIPI library along with all tools and examples.

BUILD SUCCESSFUL

Total time: 2.058 secs
```
**About:**

**Why:**

## 5. Results (visualization) and discussion about findings. here you should also include a roadmap of each objective that has been addressed. (Hafiz)

Results:



Discussion:

Roadmap: (Maybe in table format)

- Build a cluster of machines
  - A desktop and 3 Raspberry Pi's are connected in one cluster
- Build an image processing flow
  - We use the HIPI framework to do a basic image processing
- Delegate work for the machines using a distributed storage and processing framework
  - We use Apache Hadoop to distribute the work between the Pi's  
- Orchestrate the communication between the machines using a central server
- Build edge computing node that transfers data to cluster


## 6. Conclusion

## 7. References
