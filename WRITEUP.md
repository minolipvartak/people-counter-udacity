# Project Write-Up

You can use this document as a template for providing your project write-up. However, if you
have a different format you prefer, feel free to use it as long as you answer all required
questions.

## Explaining Custom Layers

TensorFlow Object Detection Model Zoo (https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md) has several pre-trained models on the coco dataset. The Ssd_inception_v2_coco and the faster_rcnn_inception_v2_coco performed good as compared to rest of the models.But, in this project, faster_rcnn_inception_v2_coco is used which is fast in detecting people with less errors. 
Intel openVINO already contains extensions for custom layers used in TensorFlow Object Detection Model Zoo.

As per instructions, the model can not be the existing models provided by Intel. So, I converted the TensorFlow model to Intermediate Representation (IR) or OpenVINO IR format.

Here are the steps: 

Downloading the model from the GitHub repository of Tensorflow Object Detection Model Zoo by the following command:

```
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_v2_coco_2018_01_28.tar.gz 
```
Extracting the tar.gz file by the following command:


```
tar -xvf faster_rcnn_inception_v2_coco_2018_01_28.tar.gz
```
Changing the directory to the extracted folder of the downloaded model:
```
cd faster_rcnn_inception_v2_coco_2018_01_28
```
The model can't be the existing models provided by Intel. So, converting the TensorFlow model to Intermediate Representation (IR) or OpenVINO IR format. The command used is given below:

```

python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model faster_rcnn_inception_v2_coco_2018_01_28/frozen_inference

```
## Comparing Model Performance


I compared the models based on their accuracy. Following are some insights about the two models:

Model-1: Ssd_inception_v2_coco_2018_01_28

I converted the model to intermediate representation. But, this model lacked accuracy as it was not able to detect people accurately in the video. Thus, I altered the threshold for increasing its accuracy but the results were not as expected.

Model-2: Faster_rcnn_inception_v2_coco_2018_01_28

I converted the model to intermediate representation. This model performed better in the output video. After using a threshold of 0.4, the model works better than all the previous methods.

### COMPARISION

Talking about comparision, I was concerned about the speed of these models:

Speed (in ms):
faster_rcnn_inception_v2_coco: 58
Ssd_inception_v2_coco: 42


## Assess Model Use Cases

This application is capable to tract the number of people in a particular area or room where there is restriction on the count of people. Following are the use-cases:

Use-case 1: ATM cabin
It is advisable that for a single ATM machine, there should be only a single person to be present. 

Use-case 2: Covid 19 pandamic
Due to outbreak of Covid-19, there are certain restrictions on the number of people in a room. So, with this application we can tract the number of people present in a room. 

## Assess Effects on End User Needs

There could be several observtions that can be drawn from the model by testing in on  different scenarious. Scenarios could be a room with dim light, camera resoulution. These are the important factor to consider while selecting suitable model.

## Running the application

After converting the downloaded model to the OpenVINO IR, all the three servers can be started on separate terminals i.e.

- MQTT Mosca server
- Node.js* Web server
- FFmpeg server

**Set up the environment**

Configuring the environment to use the Intel® Distribution of OpenVINO™ toolkit one time per session by running the following command:
```

source /opt/intel/openvino/bin/setupvars.sh -pyver 3.5
```

Next, from the main directory:

Step 1 - Start the Mosca server
```cd webservice/server/node-server```
```node ./server.js```

If the command runs successfully, it will display the following message:

```Mosca server started.```

Step 2 - Start the GUI
Opening new terminal and executing below commands:

```cd webservice/ui```
```npm run dev```
If the command runs successfully, it will display the following message:

```webpack: Compiled successfully```
Step 3 - FFmpeg Server
Open new terminal and execute below command:

```sudo ffserver -f ./ffmpeg/server.conf```
Step 4 - Run the code
Open new terminal and execute below command:

```python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m faster_rcnn_inception_v2_coco_2018_01_28/frozen_inf```