<p align="center">
<img width=150 src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/logo.png">
</p>
<h1 align="center">TensorSpace.js</h1>
<p align="center"><b>Present Tensor in Space</b></p>

<p align="center">
<strong>English</strong> | <a href="https://github.com/tensorspace-team/tensorspace/blob/master/README_zh.md"><strong>中文</strong></a>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/tensorspace"><img src="https://img.shields.io/npm/v/tensorspace.svg" alt="npm version" height="18"></a>
  <a href="https://github.com/tensorspace-team/tensorspace/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-green.svg" alt="license badge"></a>
  <a href="https://github.com/mrdoob/three.js/"><img src="https://img.shields.io/badge/dependencies-Three.js-brightgreen.svg" alt="dependencies badge"></a>
      <a href="https://github.com/tensorflow/tfjs"><img src="https://img.shields.io/badge/dependencies-TensorFlow.js-brightgreen.svg" alt="dependencies badge"></a>
  <a href="https://travis-ci.org/syt123450/giojs"><img src="https://travis-ci.org/syt123450/giojs.svg" alt="build"></a>
</p>

TensorSpace is a neural network 3D visualization framework built by TensorFlow.js and Three.js. TensorSpace provides Keras-like APIs to build deep learning layers, load pre-trained models, and generate a 3D visualization  in the browser. From TensorSpace, it is intuitive to learn what the model structure is, how the model is trained and how the model predicts the results based on the intermediate information. After preprocessing the model, TensorSpace supports to visualize pre-trained model from TensorFlow, Keras and TensorFlow.js.

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_lenet.gif">
</p>
<p align="center">
<b>Fig.1</b> - Interactive LeNet created by TensorSpace
</p>

## Table of Content

* [Motivation](#motivation)
* [Getting Start](#getting-start)
* [Example](#example)
* [Documentation](documentation)
* [Contributors](#contributors)
* [License](#license)

## Motivation

TensorSpace is a neural network 3D visualization framework designed for not only showing the basic model structure, but also presenting the processes of internal feature abstractions, intermediate data manipulations and final inference generations. 

By applying TensorSpace API, it is more intuitive to visualize and understand any pre-trained models built by TensorFlow, Keras, TensorFlow.js, etc. TensorSpace introduces a way for front end developers to be involved in the deep learning networks. As an open source library, TensorSpace team welcomes any further development on visualization applications.

* **Interactive** -- Use Keras-like API to build interactive model in browser.
* **Intuitive** -- Visualize the information from intermediate inferences.
* **Integrative** -- Support pre-trained models from TensorFlow, Keras, TensorFlow.js.


## Getting Start

### Install

* **Step 1: Download TensorSpace.js**

There are three ways to download TensorSpace.js: npm, yarn, or official website

Option 1: NPM
```bash
npm install tensorspace
```

Option 2: Yarn
```bash
yarn add tensorspace
```

Option 3: [Website download page](http://tensorspace.org/index.html#download)

* **Step 2: Install Dependency**

Include [TensorFlow.js](https://github.com/tensorflow/tfjs), [Three.js](https://github.com/mrdoob/three.js), [Tween.js](https://github.com/tweenjs/tween.js), [TrackballControl.js](https://github.com/mrdoob/three.js/blob/master/examples/js/controls/TrackballControls.js) in html file before include TensorSpace.js

```html
<script src="tf.min.js"></script>
<script src="three.min.js"></script>
<script src="tween.min.js"></script>
<script src="TrackballControls.js"></script>
```

* **Step 3: Install TensorSpace.js**

Include TensorSpace.js into html file:
```html
<script src="tensorspace.min.js"></script>
```

### Preprocessing

For presenting multiple intermediate outputs, we need to [preprocess](https://github.com/tensorspace-team/tensorspace/tree/master/docs/preprocess) the pre-trained model.

Based on different training libraries, we provide different tutorials: [TensorFlow model preprocessing](https://github.com/tensorspace-team/tensorspace/tree/master/docs/preprocess/TensorFlow), [Keras model preprocessing](https://github.com/tensorspace-team/tensorspace/tree/master/docs/preprocess/Keras) and [TensorFlow.js model preprocessing](https://github.com/tensorspace-team/tensorspace/tree/master/docs/preprocess/TensorFlowJS).


### Usage

If installed TensorSpace and preprocessed the pre-trained deep learning model successfully, let's create an interactive 3D TensorSpace model.

We use a [preprocessed TensorSpace compatible LeNet model](https://github.com/tensorspace-team/tensorspace/tree/master/examples/helloworld/model) as an example to continue the process.

[helloworld](https://github.com/tensorspace-team/tensorspace/tree/master/examples/helloworld)
[5](https://github.com/tensorspace-team/tensorspace/blob/master/examples/helloworld/data/5.json)

First, we need to new a TensorSpace model instance:
```JavaScript
let container = document.getElementById( "container" );
let model = new TSP.model.Sequential( container );
```

Next, based on the LeNet structure: Input + 2 X (Conv2D & Maxpooling) + 3 X (Dense), we build the structure of the model:
```JavaScript
model.add( new TSP.layers.Input2d({ shape: [28, 28, 1] }) );
model.add( new TSP.layers.Padding2d({ padding: [2, 2] }) );
model.add( new TSP.layers.Conv2d({ kernelSize: 5, filters: 6, strides: 1 }) );
model.add( new TSP.layers.Pooling2d({ poolSize: [2, 2], strides: [2, 2] }) );
model.add( new TSP.layers.Conv2d({ kernelSize: 5, filters: 16, strides: 1 }) );
model.add( new TSP.layers.Pooling2d({ poolSize: [2, 2], strides: [2, 2] }) );
model.add( new TSP.layers.Dense({ units: 120 }) );
model.add( new TSP.layers.Dense({ units: 84 }) );
model.add( new TSP.layers.Output1d({
    units: 10,
    outputs: ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
}) );
```

Last, we should load our [preprocessed TensorSpace compatible model]() and use `init()` method to create the TensorSpace model:
```JavaScript
model.load({
    type: "tfjs",
    url: './lenetModel/mnist.json',
    onComplete: function() {
        console.log( "\"Hello World!\" from TensorSpace Loader." );
    }
});
model.init();
```

We can get the following model in the browser if everything looks good:
<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/HelloWorld_empty_lenet.jpg">
</p>
<p align="center">
<b>Fig. 2</b> - Generated LeNet model
</p>


We provide a extracted file which is a handwritten "5" as the input of our model:

```
model.init(function() {
    model.predict( image_5 );
});

```

We put the `predict( image_5 )` method in the callback function of `init()` to ensure the prediction is after the initialization complete.

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/HelloWorld_5.jpg">
</p>
<p align="center">
<b>Fig. 3</b> - Generated LeNet model with input data "5"
</p>

## Example

* **LeNet** 

[➡ Live Demo](http://tensorspace.org/html/playground/lenet.html)

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_lenet.gif">
</p>
<p align="center">
<b>Fig.4</b> - TensorSpace LeNet
</p>

* **AlexNet** 

[➡ Live Demo](http://tensorspace.org/html/playground/alexnet.html)

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_alexnet.gif">
</p>
<p align="center">
<b>Fig.5</b> - TensorSpace AlexNet
</p>

* **Yolov2-tiny**

[➡ Live Demo](http://tensorspace.org/html/playground/yolov2-tiny.html)

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_yolov2.gif">
</p>
<p align="center">
<b>Fig.6</b> - TensorSpace YOLO-v2-tiny
</p>

* **ResNet-50**

[➡ Live Demo](http://tensorspace.org/html/playground/resnet50.html)

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_resnet50.gif">
</p>
<p align="center">
<b>Fig.7</b> - TensorSpace ResNet-50
</p>

* **Vgg16**

[➡ Live Demo](http://tensorspace.org/html/playground/vgg16.html)

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_vgg.gif">
</p>
<p align="center">
<b>Fig.8</b> - TensorSpace VGG-16
</p>

* **ACGAN**

[➡ Live Demo](http://tensorspace.org/html/playground/acgan.html)

<p align="center">
<img width="100%" src="https://github.com/tensorspace-team/tensorspace/blob/master/assets/tensorspace_acgan.gif">
</p>
<p align="center">
<b>Fig.9</b> - TensorSpace ACGAN
</p>

## Documentation

* For a quick start, checkout [Getting Start](http://tensorspace.org/html/docs/startHello.html)
* To download/install, see [Download](http://tensorspace.org/index.html#download)
* To learn more about the [Basic Concepts](http://tensorspace.org/html/docs/basicIntro.html)
* To process a deep learning model, checkout [Model Preprocessing](http://tensorspace.org/html/docs/preIntro.html)
* To learn core components: [Models](http://tensorspace.org/html/docs/modelIntro.html), [Layers](http://tensorspace.org/html/docs/layerIntro.html) and [Merge Function](http://tensorspace.org/html/docs/mergeIntro.html)
* Checkout the official website [TensorSpace.org](http://tensorspace.org) for more about TensorSpace.

## Contributors

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars2.githubusercontent.com/u/7977100?v=4" width="100px;"/><br /><sub><b>syt123450</b></sub>](https://github.com/syt123450)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=syt123450 "Code") [🎨](#design-syt123450 "Design") [📖](https://github.com/tensorspace-team/tensorspace/commits?author=syt123450 "Documentation") [💡](#example-syt123450 "Examples") | [<img src="https://avatars3.githubusercontent.com/u/4524339?v=4" width="100px;"/><br /><sub><b>Chenhua Zhu</b></sub>](https://github.com/zchholmes)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=zchholmes "Code") [🎨](#design-zchholmes "Design") [✅](#tutorial-zchholmes "Tutorials") [💡](#example-zchholmes "Examples") | [<img src="https://avatars0.githubusercontent.com/u/21956621?v=4" width="100px;"/><br /><sub><b>YaoXing Liu</b></sub>](https://charlesliuyx.github.io/)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=CharlesLiuyx "Code") [🎨](#design-CharlesLiuyx "Design") [✅](#tutorial-CharlesLiuyx "Tutorials") [💡](#example-CharlesLiuyx "Examples") | [<img src="https://avatars2.githubusercontent.com/u/19629037?v=4" width="100px;"/><br /><sub><b>Qi(Nora)</b></sub>](https://github.com/lq3297401)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=lq3297401 "Code") [🎨](#design-lq3297401 "Design") |
| :---: | :---: | :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

## License

[Apache License 2.0](https://github.com/tensorspace-team/tensorspace/blob/master/LICENSE)