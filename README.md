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
As an information medium, images are not only without language restrictions, but also rich in content. The number of images in various application fields has grown very rapidly. From astronomical observation to remote sensing on the ground, from e-commerce to social networking sites, terabyte-level images are being generated every day. Compared with text files, image files occupy a large space and have a slow processing speed. Therefore, improving the processing efficiency of images has important theoretical significance and practical value for specific applications.

Hadoop handles text files by default, and image files are stored and processed in a different way from ordinary text files. Hadoop does not have its own image processing interface. Moreover, the Hadoop framework is very suitable for the operation of large files. Therefore, this article uses a third-party developed image processing interface HIPI (Hadoop Image Processing Interface), which combines the source images and stores them on HDFS for MapReduce processing.

## 2. Previous case study
According to our research, image classification using Raspberry Pi is not new as there are plenty of previous researches regarding it. To kick start was a research done by (Kochláň, Hodoň, Čechovič, Kapitulík, & Jurečka, 2014) where researchers for this paper conducted a research an image classification using a Raspberry Pi to monitor traffic flow. At the end of the researcher, researchers managed to obtain two findings, which is the traffic volume and vehicle class. Throughout this system, researchers managed to achieve an accuracy level of 95.7% and 63.2%, for traffic volume and vehicle class respectively. Unfortunately, the system was not able to identify the vehicle speed which due to the non-functional microwave speed detector.

The other research paper that caught our attention was a research done by (Dürr, Pauchard, Browarnik, Axthelm, & Loeser, 2015) which previous researchers developed face recognition model using Raspberry Pi, model B, with the help of Convolutional Neural Network (CNN) classifier. Data source are images taken by researchers using Raspberry Pi camera which then being converted to 8-bit gray-scale before been downscaled to a 640x480 pixel resolution. Next researchers applied CNN to those images to detect faces. Other than that, previous researchers even  compared the classification accuracy level with Fisherface classifier but CNN turned out to perform better, 87.5% and 96.9% respectively.

As an alternative to CNN there was a research done by (Mehta & Tomar, 2016) which implemented several types of algorithms for different purposes. Viola-Jones algorithm was used for face detection, combination of Local Binary Pattern (LBP) and Histogram Oriented Gradient (HOG) was used as face extraction, lastly Support Vector Machine (SVM) was used to recognize faces. Previous researchers acquire data based on the video taken from the camera that was connected to the controller board which then transferred to Matlab via wireless connection for further processing. From the video, a frame will be selected, which is in Red, Green, Blue (RGB) format that will then converted to greyscale as preparation for the next process. Viola-Jones algorithm was used for face detection before certain features were extracted from the cropped detected faces. Images undergone contrast improvement, background removal, and also conversion images to black and white during the pre-processing phase. Next, LBP and HOG were used to extract six features from the images before extracted images being compared with stored images in the database with the help of SVM. Researchers managed to obtained an accuracy level of 92% at the end of the project.

According to previous research did by (Aljasem, Heeney, Gritti, & Raimondi, 2016), previous researchers implemented Raspberry Pi to classify images in real-time, aim to help blind individual. Data was acquired from cameras attached to Raspberry Pi which then being transferred to the local machine to manually classified into three categories; walkable, non-walkable, and average. Histogram of Oriented Gradients (HOG) was used upon these images, that had been classified, as an extraction process. Feature extracted images were converted into CSV files which then transferred to Weka, data mining tool, before Support Vector Machine (SVM) – best classification algorithm compared Naïve Bayesian classifier and Adaboost – being used by previous researchers to classify the images. Weka gave an indication between 0 to 1, where 0 is non-walkable and 1 is walkable, where input data resulted from the classification made by SVM. Indication from Weka – between 0 to 1 – then acted as a data input to the motor where motor is activated when Weka gave an output that is greater than 0.5.

Based on previous research conducted by (Arsh, Bhatt, & Kumar, 2016), in which previous researchers managed to provide solution to increase the performance of image processing – detecting center of Organic-Light-Emitting-Diode – which is complex that demand for high computation power. At the end of the research, previous researchers have found out that more time was consumed to create different instances of mapper and to allocate in different nodes as number of rows processed by every mapper were small, compared to parallelized processing. Other than that, total number of mapper instances created, and logical splits decreased, as number of rows/columns per mapper increased. However, after certain value processing time increases as per mapper task increase beyond it as parallel processing more than compensate the process of instance creation. Due to this, previous researcher had concluded that splitting process is very crucial and need to be done in a manner where total work was divided equally. 

The other research paper that caught our attention was a research done by (Kaur, Sachdeva, & Singh, 2017) in which previous researchers performed image processing on a Multinode Hadoop Cluster. Research used images from sources like cameras, the Internet, and also smartphone as an input data the were then placed in Hadoop Image Bundle (HIB). Researchers used Hadoop framework along with HIPI during the extraction process which only took a minute and thirteen seconds to process, that is so much faster when compared to traditional technologies. Previous also mentioned that with the help of batch processing in Hadoop, heavy jobs and time-consuming jobs that are repetitive can be done faster without any interaction. 


## 3. The objective of the project
- Build a cluster of machines
- Build an image processing flow
- Delegate work for the machines using a distributed storage and processing framework
- Orchestrate the communication between the machines using a central server
- Build edge computing node that transfers data to cluster

## 4. Scenario
The project is suitable to be deployed to an array of industies which requires monitoring or collection of information in large facilities or a geographically expansive area. 

An example is you own a large factory producing and packaging consumer products. The process starts with raw material processing at one end of the factory feeding machines and conveyer systems throughout the length of factory floor with some systems up to 2 storey in height. Periodic maintenance is key to ensuring optimum working condition for maximum output. 

However, the rising labour cost and high attrition rate due to working environment is affecting maintenance schedule and effectiveness. Traditional monitoring systems are cumbersome and expensive as they need to be wired throughout the whole factory. Therefore a system where various edge sensors that are able to relay critical information wirelessly to a storage and processing node would be an elegant and cost effective solution. Given the number of machines, processes and monitoring required, a huge amount of data is generated, therefore having an off the shelf distributed storage and processing solution (ie: Raspberry Pi Hadoop Cluster), could be a very attractive proposition.  

## 5. Methodology
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
  - Use the `hibimport` tool to format raw images to a format that's compatible with the HIPI workflow
  - The images would be represented as HipiImageBundle (HIB) format. A HIB is a collection of images represented as a single file on the HDFS.
  - With successful import using `hibimport`, you'll see two files created on HDFS, `*.hib` and `*.hib.dat`
  - HIB would then run through the MapReduce/HIPI flow to process the images 
  - These are the shell commands run on the Pi@Master:

```sh
# run in the root of the HIPI directory
# -> import images into the HIB format

$ tools/hibImport.sh ~/home/pi/Desktop images.hib
Input image directory: /home/pi/Desktop
Output HIB: images.hib
Overwrite HIB if it exists: false
HIPI: Using default blockSize of [134217728].
HIPI: Using default replication factor of [1].
 ** added: 1.jpg
 ** added: 2.jpg
 ** added: 3.jpg
Created: images.hib and images.hib.dat
```

```sh
# run in the root of the HIPI directory
# -> verify the content of the HIB file using the hibInfo tool

$ tools/hibInfo.sh images.hib --show-meta
Input HIB: images.hib
Display meta data: true
Display EXIF data: false
IMAGE INDEX: 0
   640 x 480
   format: 1
   meta: {source=/Users/hipiuser/SampleImages/1.jpg}
IMAGE INDEX: 1
   3210 x 2500
   format: 1
   meta: {source=/Users/hipiuser/SampleImages/2.jpg}
IMAGE INDEX: 2
   3810 x 2540
   format: 1
   meta: {source=/Users/hipiuser/SampleImages/3.jpg}
Found [3] images.
```

```java
// AveragePixelColor.java
// this program will be the entrypoint for our processing logic

import org.hipi.image.FloatImage;
import org.hipi.image.HipiImageHeader;
import org.hipi.imagebundle.mapreduce.HibInputFormat;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class AveragePixelColor extends Configured implements Tool {
  
  public static class AveragePixelColorMapper extends Mapper<HipiImageHeader, FloatImage, IntWritable, FloatImage> {
    public void map(HipiImageHeader key, FloatImage value, Context context) 
      throws IOException, InterruptedException {
    }
  }
  
  public static class AveragePixelColorReducer extends Reducer<IntWritable, FloatImage, IntWritable, Text> {
    public void reduce(IntWritable key, Iterable<FloatImage> values, Context context) 
      throws IOException, InterruptedException {
    }
  }
  
  public int run(String[] args) throws Exception {
    // Check input arguments
    if (args.length != 2) {
      System.out.println("Usage: AveragePixelColor <input HIB> <output directory>");
      System.exit(0);
    }
    
    // Initialize and configure MapReduce job
    Job job = Job.getInstance();
    // Set input format class which parses the input HIB and spawns map tasks
    job.setInputFormatClass(HibInputFormat.class);
    // Set the driver, mapper, and reducer classes which express the computation
    job.setJarByClass(AveragePixelColor.class);
    job.setMapperClass(AveragePixelColorMapper.class);
    job.setReducerClass(AveragePixelColorReducer.class);
    // Set the types for the key/value pairs passed to/from map and reduce layers
    job.setMapOutputKeyClass(IntWritable.class);
    job.setMapOutputValueClass(FloatImage.class);
    job.setOutputKeyClass(IntWritable.class);
    job.setOutputValueClass(Text.class);
    
    // Set the input and output paths on the HDFS
    FileInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));

    // Execute the MapReduce job and block until it complets
    boolean success = job.waitForCompletion(true);
    
    // Return success or failure
    return success ? 0 : 1;
  }
  
  public static void main(String[] args) throws Exception {
    ToolRunner.run(new AveragePixelColor(), args);
    System.exit(0);
  }
  
} // AveragePixelColor
```

```java
// this program will run in the Map stage

public static class AveragePixelColorMapper extends Mapper<HipiImageHeader, FloatImage, IntWritable, FloatImage> {
    
    public void map(HipiImageHeader key, FloatImage value, Context context) 
        throws IOException, InterruptedException {

      // Verify that image was properly decoded, is of sufficient size, and has three color channels (RGB)
      if (value != null && value.getWidth() > 1 && value.getHeight() > 1 && value.getNumBands() == 3) {

        // Get dimensions of image
        int w = value.getWidth();
        int h = value.getHeight();

        // Get pointer to image data
        float[] valData = value.getData();

        // Initialize 3 element array to hold RGB pixel average
        float[] avgData = {0,0,0};

        // Traverse image pixel data in raster-scan order and update running average
        for (int j = 0; j < h; j++) {
          for (int i = 0; i < w; i++) {
            avgData[0] += valData[(j*w+i)*3+0]; // R
            avgData[1] += valData[(j*w+i)*3+1]; // G
            avgData[2] += valData[(j*w+i)*3+2]; // B
          }
        }

        // Create a FloatImage to store the average value
        FloatImage avg = new FloatImage(1, 1, 3, avgData);

        // Divide by number of pixels in image
        avg.scale(1.0f/(float)(w*h));

        // Emit record to reducer
        context.write(new IntWritable(1), avg);

      } // If (value != null...
      
    } // map()

  } // AveragePixelColorMapper
```

```java
// this program will run in the Reducer stage

public static class AveragePixelColorReducer extends Reducer<IntWritable, FloatImage, IntWritable, Text> {

    public void reduce(IntWritable key, Iterable<FloatImage> values, Context context)
        throws IOException, InterruptedException {

      // Create FloatImage object to hold final result
      FloatImage avg = new FloatImage(1, 1, 3);

      // Initialize a counter and iterate over IntWritable/FloatImage records from mapper
      int total = 0;
      for (FloatImage val : values) {
        avg.add(val);
        total++;
      }

      if (total > 0) {
        // Normalize sum to obtain average
        avg.scale(1.0f / total);
        // Assemble final output as string
	float[] avgData = avg.getData();
        String result = String.format("Average pixel value: %f %f %f", avgData[0], avgData[1], avgData[2]);
        // Emit output of job which will be written to HDFS
        context.write(key, new Text(result));
      }

    } // reduce()

  } // AveragePixelColorReducer
```

```sh
$ gradle jar
:core:compileJava UP-TO-DATE
:core:processResources UP-TO-DATE
:core:classes UP-TO-DATE
:core:jar UP-TO-DATE
:AveragePixelColor:compileJava
:AveragePixelColor:processResources UP-TO-DATE
:AveragePixelColor:classes
:AveragePixelColor:jar

BUILD SUCCESSFUL

Total time: 0.855 secs
```

```sh
$ hadoop jar AveragePixelColor.jar images.hib processed_images_output
```

**Phase 4: Result**

Two files would be created on a successful processing:

```sh
$ hadoop fs -ls processed_images_output
Found 2 items
-rw-r--r--   1 user group        0 2015-03-13 09:52 processed_images_output/_SUCCESS
-rw-r--r--   1 user group       50 2015-03-13 09:52 processed_images_output/part-r-00000
```

Whenever a MapReduce program successfully finishes, it creates the file _SUCCESS in the output directory along with a part-r-XXXXX file for each reduce task. The average pixel value can be retrieved using the cat command:

```sh
$ hadoop fs -cat processed_images_output/part-r-00000
1 Average pixel value: 0.321921 0.224995 0.150284
```

## 6. Explanation of all the components being involved in the project. Adjustment of why these components are being used.

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

**About:**

A framework for distributed storage and processing of big data using the MapReduce programming model. It is designed to scale up from a single servers to thousands of machines, each offering local computation and storage. The framework has many modules such as Common, HDFS, YARN, MapReduce, Ozone, and Submarine.

**Installation Steps:**
(refer to Jonathan's step)

**Why**

- Open source
- Store and process huge amounts of data quickly (parallelisation)
- Distributed computing power that scale linearly with the number of nodes
- Built-in fault tolerant features such as redundant copies and tasks re-delegation in case of node failures
- HDFS is a flexible type of data storage that allow the framework to handle all kinds of data
- Framework that can easily scale by simply adding more nodes

### Hadoop Image Processing Interface (HIPI)
![alt text](http://hipi.cs.virginia.edu/images/hipi_pipeline.png?raw=true)

**About:**

A framework that takes image files as input, converts it to a HIPI Image Bundle (.hib) data type. There are two files after conversion, the file ending with ".hib.dat" is the data file containing images and 2nd file with ".hib" which is the index along with information of the offsets of each image in the data file. 

Next, the HIPI framwork would then take converted files and behind the scenes will perform distribution of the image bundle across the map nodes. The images will then go through a culling stage before being fed as input to map tasks (Apache Hadoop). 

**Installation Steps:**

```sh
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

**Why:**

- Open source library
- High-level library to interact with images processing on top of Hadoop
- Help developers to focus on the processing logic instead of data handling on HDFS
- Highly efficient and high-throughput implementation
- Many integrations with other popular libraries like OpenCV

## 5. Result, Discussion, and Roadmap

**Results:**

- The system built is able to achieve parallel images processing using a distributed framework
- The data pipeline is able to transfer data from raw images in a folder to the compute node

**Discussion:**

- Challenges when dealing with binaries (e.g images file format) within HDFS
  - At first, we tried to achieve image processing by just using Hadoop
  - The file block chunking in HDFS making it difficult to get the original image for processing after split
  - HIPI makes this process seamless by guaranteeing that the Reducer would combine back all the images into its original input
- Challenges when trying to integrate machine learning algorithms not available in Java (or natively supported in Hadoop)
  - We tried to use Tensorflow image classifier, but the integration with other languages/docker images within Hadoop would require an integration with Hadoop Streaming feature (would allow us to communicate with Map/Reducer using stdin/stdout)
- Challenges when dealing with cluster setup
  - Node Discovery: we have to use DHCP and static IPs to identify all members of the cluster
  - Node Authentication: we have to use SSH authorized_keys to allow SSH session into each machine
- Performance limitation when using wireless network for the cluster setup
  - For a massive dataset processing, using an ethernet cable would improve the data throughput
- Challenges when integrating with Node-RED
  - Network Discovery: any interaction with the Pi nodes would require the Node-RED server to identify the IP address
  - Learning curve when dealing with many available options in the Node-RED workflow builder (e.g when to use what)
  - Port-forwarding and dynamic DNS hosting to allow collaboration on the Node-RED server

**Roadmap:**

| Objectives                                                                          	| Achieved                                                                               	| Future Work                                                                                                                                                                                                                                              	|
|-------------------------------------------------------------------------------------	|----------------------------------------------------------------------------------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Build a cluster of machines                                                         	| - A desktop and 3 Raspberry Pi's are connected in one cluster within a private network 	| - Deploy the flow into a public cloud                                                                                                                                                                                                                    	|
| Build an image processing flow                                                      	| - We use the HIPI framework to do an image processing task                             	| - Extend the MapReduce processing to include non-Java machine libraries (e.g Python's Caffe) using Apache Streaming<br>- Explore Apache Submarine for a distributed machine learning framework<br>- Explore image classification flow (e.g Apache Spark) 	|
| Delegate work for the machines using a distributed storage and processing framework 	| - We use Apache Hadoop to distribute the work between the Pi's                         	| - Integrate Hadoop YARN for cluster resource management                                                                                                                                                                                                  	|
| Orchestrate the communication between the machines using a central server           	| - We rely on Node-RED and VNC to communicate data and input between machines           	| - Integrate process result feedback cycle into the Node-RED server<br>- Publish the computed result to a web service deployed in the internet                                                                                                            	|
| Build edge computing node that transfers data to cluster                            	| - Data transfer from edge computing to a single cluster                                     	| - Data transfer between multiple edge computing clusters                                                                                                                                                                                                 	|


## 7. Conclusion
Working through the objectives of the project presented to us the ability of edge computing and parallel distributed computing technology to solve issues in industries not limited to high performance computing or high throughput computing problems. 

The advancement of semiconductor technology in nanometer range has enabled production of small form factor, low power consumption and high processing ability embedded into a microcontroller system on chip (SoC). This allowed proliferation of edge node applications (environmental sensors, image capture and video capture systems) at a very competitive cost model which previously would require huge capital investments. This is demonstrated via our usage of Raspberry Pi (Rm150-RM270) to run an edge node image transfer system. 

The edge nodes are geographically spread out (ie: over several thousand sq-ft of factory or farm land) makes it difficult to wire all these sensors. We demonstrated that with very low cost use of wireless routers, it is possible to connect networks of edge nodes without having wires. The only shortfall is the need for AC power supply, however with the availability of efficient solar array solutions and increase of low power embedded processors with smart power modes, it is possible to run these networks without AC power available. 

With the expansion of these edge nodes introduces high and fast streaming data which may not need to be analyzed immediately but would need to be processed and stored. A small medium industry (SME) player with less than 100 employees would likely not have the capital to invest in traditional server system, let alone be able to extract the data for analysis. However with the availability of IaaS such as IBM Watson, a SME would be able to connect their factory or farm sensor data stream to be stored in IBM cloud or other cloud infrastructure and be able to analyze their data at very low cost using the analysis and today AI tools made available. We demonstrate the distributed processing and storage of data using Raspberry Pi using Apache Hadoop as file storage system and HIPI as image processing framework. 

This project has been a very interesting experience where we got to experience the power of edge computing and parallel distributed computing within a single project. In the future, it would be interesting to expand the processing of the images stored in Hadoop to perform classification. 

## 8. References
Aljasem, D. K., Heeney, M., Gritti, A. P., & Raimondi, F. (2016). On-the-Fly Image Classification to Help Blind People. Paper presented at the 2016 12th International Conference on Intelligent Environments (IE).

Arsh, S., Bhatt, A., & Kumar, P. (2016). Distributed image processing using Hadoop and HIPI. Paper presented at the 2016 International Conference on Advances in Computing, Communications and Informatics (ICACCI).

Dürr, O., Pauchard, Y., Browarnik, D., Axthelm, R., & Loeser, M. (2015). Deep Learning on a Raspberry Pi for Real Time Face Recognition. Paper presented at the Eurographics (Posters).

Kaur, J., Sachdeva, K., & Singh, G. (2017). Image processing on multinode hadoop cluster. Paper presented at the 2017 International Conference on Electrical, Electronics, Communication, Computer, and Optimization Techniques (ICEECCOT).

Kochláň, M., Hodoň, M., Čechovič, L., Kapitulík, J., & Jurečka, M. (2014). WSN for traffic monitoring using Raspberry Pi board. Paper presented at the 2014 Federated Conference on Computer Science and Information Systems.

Mehta, P., & Tomar, P. (2016). An efficient attendance management sytem based on face recognition using Matlab and Raspberry Pi 2. International Journal of Engineering Technology Science and Research IJETSR, 3(5), 71-78. 

Sweeney, Chris & Liu, Liu & Arietta, Sean & Lawrence, Jason. (2019). HIPI: A Hadoop Image Processing Interface for Image-based MapReduce Tasks. 

Markovic, Dusan & Vujicic, Dejan & Dragana, Mitrović & Ranđić, Siniša. (2018). Image Processing on Raspberry Pi Cluster. 2. 83 - 90. 10.7251/IJEEC1802083M. 


