# Real-Time-Object-Detection-using-Tensorflow.js


<p align= "center">
  <img width="750" height="400" src="https://github.com/mossydidar/Real-Time-Object-Detection-using-Tensorflow.js/blob/master/1.png">
</p>



## Real Time Demonstration:
* [Click Here for Real Time Demo](https://jlo24226ww.codesandbox.io/)

  
  * Note: Loading the model can take several seconds.
  * It is best to load the model once and save a reference to it.
  * Use Google Chrome Browser on PC, Tablets, Phones. (Highly Recommended) 
  
## Object Detection:
Object detection is the process of finding instances of real-world objects such as faces, bicycles, and buildings in images or videos. Object detection algorithms typically use extracted features and learning algorithms to recognize instances of an object category.

## Detecting Objects
To make object detection predictions, all we need to do is import the TensorFlow model, coco-ssd, which can be installed with a package manager like NPM or simply imported in a <script> tag. We can then load the model, and make a prediction.
  
 ```
import * as cocoSsd from "@tensorflow-models/coco-ssd";
const image = document.getElementById("image")
cocoSsd.load()
  .then(model => model.detect(image))
  .then(predictions => console.log(predictions))
```
  
 
The image we pass into the detection function is just a reference to the html <img> tag:
```
<img id="image" src="image_url">

``` 

After we get our prediction, we need a way to display it to the screen. It should look something like this:

```
[{
  bbox: [x, y, width, height],
  class: "cat",
  score: 0.8380282521247864
}]

``` 

We then use the <canvas> element:

```
<canvas id="canvas">
``` 

The canvas element allows us to use the strokeRect function, which maps perfectly with our prediction results:

```
const x = prediction.bbox[0];
const y = prediction.bbox[1];
const width = prediction.bbox[2];
const height = prediction.bbox[3];

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

ctx.strokeRect(x, y, width, height);
``` 



## Streaming from the Webcam
To run real-time detection on a webcam stream is almost as easy as changing from an <img> tag, to a <video> tag …with the simple exception of this giant blob of code to start up the webcam:
  
 ```
const video = document.getElementById("video")
    
navigator.mediaDevices
  .getUserMedia({
    audio: false,
    video: {
      facingMode: "user",
      width: 600,
      height: 500
    }
  })
  .then(stream => {
    video.srcObject = stream
    video.onloadedmetadata = () => {
      video.play()
    }
  })
  
```  

We can then just pass our video element to our model for detection. However, this time we are going to call requestAnimationFrame which will call our detection function over and over in an infinite loop as fast as it can, skipping frames when it can’t keep up.

``` 
function detectFrame() {
  model.detect(video).then(predictions => {
    renderOurPredictions(predictions)
    requestAnimationFrame(detectFrame)
  })
}
``` 
  
## Dependencies:

* [Tensorflow Models: Coco SSD](https://www.npmjs.com/package/@tensorflow-models/coco-ssd) - Object detection model that aims to localize and identify multiple objects in a single image.
* [Tensorflow.js](https://www.npmjs.com/package/@tensorflow/tfjs) - TensorFlow.js is an open-source hardware-accelerated JavaScript library for training and deploying machine learning models.
* [React](https://www.npmjs.com/package/react) - React is a JavaScript library for creating user interfaces.
* [React DOM](https://www.npmjs.com/package/react-dom) - This package serves as the entry point to the DOM and server renderers for React. It is intended to be paired with the generic React package, which is shipped as react to npm.
* [React Scripts](https://www.npmjs.com/package/react-scripts) - This package includes scripts and configuration used by Create React App.

## Acknowledgement:
 **Nick Bourdakos

