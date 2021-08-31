# jetson-nodered-tensorflow

This guide is for using Tensorflow (tfjs-node-gpu) in Node-RED using Jetson Nano or Xavier NX device and run object detection on images. Might work on Xavier AGX also, but I didnt have one to test. 

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| Jetpack       | 4.6           | 
| L4T           | 32.6.1        | 
| CUDA          | 10.0.300      |  
| cuDNN         | 8.2.1.32	    | 
| libtensorflow | 2.4.1		      | 
| Node-RED	    | 2.0.5	        |
| Node.js       | 14.17.5       |
| tfjs-node-gpu | 3.9.0	        | 

## Installation

### 1. Install Jetpack 4.6 for Jetson NX Xavier/Nano

https://developer.nvidia.com/embedded/learn/getting-started-jetson

### 2. Run update & upgrade

```
sudo apt update && sudo apt upgrade
```

### 3. Install jtop 

```
sudo apt-get install python3-pip
```

```
sudo -H pip3 install -U jetson-stats
```

```
sudo systemctl restart jetson_stats.service
```

```
sudo reboot
```

```
jtop
```

### 4. Install Node-RED 

```
sudo apt-get install curl
```

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

```
sudo systemctl enable nodered.service
```

```
sudo systemctl start nodered.service
```

### 5. Install tfjs-node-gpu

```
cd ~/.node-red
```

```
npm install @tensorflow/tfjs-node-gpu@3.9.0 
```

Installation will finish with some errors. Ignore them.

### 6. Download, extract and install libtensorflow (C-libraries for Tensorflow 2.4.1)

```
cd ~/.node-red/node_modules/@tensorflow/tfjs-node-gpu/deps
```

```
wget https://jetson-nodered-files.s3.eu.cloud-object-storage.appdomain.cloud/libtensorflow-gpu-linux-arm64-1.15.0.tar.gz
```

```
tar xzvf libtensorflow-gpu-linux-arm64-1.15.0.tar.gz
```

```
sudo npm install --global node-pre-gyp
```

```
npm run build-addon-from-source
```

### 6.  Check that tensorflow is working in Node.js

```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```

### 7. Install canvas for annotating images

https://www.npmjs.com/package/canvas

Install dependencies first

```
sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
```

```
npm install canvas
```

### 8. Use Tensorflow in Node-RED

Start Node-RED

```
node-red-start
```

### 9. Import example flow 

Go to:

https://github.com/juhaautioniemi/tequ-api-client/

Copy and import 'example-ai-detect-sm.json' to your Node-RED.

You should see something like this in Node-RED log after flow is deployed, if everything regarding to Tensorflow went well:

### 10. Use Tensorflow in Node-RED

Configure model folder

Inject image to flow and start detecting objects.

First inference is slow and it takes something like ~5-30 seconds. After that it should run smoothly.

### 11. Custom object detection model

If you need to build your own model, you can follow this guide:

https://github.com/juhaautioniemi/tequ-tf1-ca-training-pipeline

## OPTIONALLY 12. Disable GUI

```
sudo service gdm stop
```

```
sudo systemctl set-default multi-user.target
```
