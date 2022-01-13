# tequ-jetson-nodered-tensorflow

This guide is for installing and configuring Tensorflow 2 (tfjs-node-gpu) on Jetson Nano or Xavier NX device and run object detection on images using Node-RED. Might work on Xavier AGX also, but I didnt have one to test. 

After installation and configuration it is possible to at least run Tensorflow 2 SavedModel, Tensorflow 1 tfjs-frozen-graph, Tensorflow.js models exported from Microsoft Azure Custom Vision and Tensorflow.js models exported from Google Teachable Machine. 

Example flows and subflows are available from:

https://github.com/Lapland-UAS-Tequ/tequ-api-client/

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| Jetpack       | 4.6           | 
| L4T           | 32.6.1        | 
| CUDA          | 10.2.300      |  
| cuDNN         | 8.2.1.32	    | 
| libtensorflow | 2.4.1		      | 
| Node-RED	    | 2.1.5	        |
| Node.js       | 16.13.2       |
| tfjs-node-gpu | 3.13.0	      | 

## Installation

### 1. Prepare Jetson Nano / Xavier NX

https://github.com/Lapland-UAS-Tequ/tequ-jetson-setup

### 2. Install tfjs-node-gpu

```
cd ~/.node-red
```

```
npm install --ignore-scripts @tensorflow/tfjs-node-gpu@3.13.0 
```

If installation finish with some errors. Ignore them at this point.

### 3. Download, extract and install libtensorflow (C-libraries for Tensorflow 2.4.1)
```
mkdir ~/.node-red/node_modules/@tensorflow/tfjs-node-gpu/deps
```

```
cd ~/.node-red/node_modules/@tensorflow/tfjs-node-gpu/deps
```

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libtensorflow-2.4.1-jetson.tar.gz
```

```
tar xzvf libtensorflow-2.4.1-jetson.tar.gz
```

```
sudo npm install --global node-pre-gyp
```

```
npm run build-addon-from-source
```

### 4.  Check that tensorflow is working in Node.js

```
cd ~/.node-red
```

```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```

### 5. Install canvas for annotating images

https://www.npmjs.com/package/canvas

Install dependencies first

```
sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
```

```
cd ~/.node-red
```

```
npm install canvas
```

### 6. Use Tensorflow in Node-RED

Start Node-RED

```
node-red-start
```

### 7. Import example flow 

Go to:

https://github.com/Lapland-UAS-Tequ/tequ-api-client/

Copy, import and deploy 'example-ai-detect-sm.json' to your Node-RED.

**For unknown reason loading the model takes 1-3 minutes**

### 8. Use Tensorflow in Node-RED

Configure model folder

Inject image to flow and start detecting objects.

First inference is slow and it takes something like ~5-30 seconds. After that it should run smoothly.

### 9. Custom object detection model

If you need to build your own model, you can follow this guide:

https://github.com/Lapland-UAS-Tequ/tequ-tf2-ca-training-pipeline

### OPTIONALLY 14. Disable GUI

```
sudo service gdm stop
```

```
sudo systemctl set-default multi-user.target
```
