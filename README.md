# Labelbox
[Labelbox](https://www.labelbox.com/) is a data labeling tool that's purpose
built for machine learning applications. Start labeling data in minutes using
pre-made labeling interfaces, or create your own pluggable interface to suit
the needs of your data labeling task. Labelbox is lightweight for single users
or small teams and scales up to support large teams and massive data sets.

## Semantic Segmentation (brush & superpixels)
![](https://s3-us-west-2.amazonaws.com/labelbox/website/images/pixelwise.gif)

## Bounding boxes, Polygons, lines, points, nested classifications
![](https://s3-us-west-2.amazonaws.com/labelbox/website/images/vectors.gif)


Table of Contents
=================

   * [Labelbox](#labelbox)
   * [Benefits](#benefits)
   * [Full Documentation](#full-documentation)
   * [Quickstart](#quickstart)
   * [Creating Custom Labeling Interfaces](#creating-custom-labeling-interfaces)
      * [A Minimal Example](#a-minimal-example)
      * [Labelbox Pluggable Interface Architecture](#labelbox-pluggable-interface-architecture)
      * [Using labeling-api.js](#using-labeling-apijs)
      * [Hello World Example](#hello-world-example)
      * [Full Example](#full-example)
      * [Full API Reference](#full-api-reference)
      * [Reference Interfaces](#reference-interfaces)
      * [Local Development of Labeling Interfaces](#local-development-of-labeling-interfaces)
      * [Installing a Labeling Frontend in labelbox.com](#installing-a-labeling-frontend-in-labelboxio)
   * [Request features](#request-features)
   * [Terms of use, privacy and content dispute policy](#terms-of-use-privacy-and-content-dispute-policy)


## Benefits
- **Simple image labeling**: Labelbox makes it quick and easy to do basic image
  classification or segmentation tasks. To get started, simply upload your data
  or a CSV file containing URLs pointing to your data hosted on a server,
  select a labeling interface, (optional) invite collaborators and start labeling.

- **Label just about anything**: Create a custom labeling interface to meet the
  needs of your labeling task. Start by customizing one of the standard
  Labelbox interfaces or build one from the ground up, just import
  `labeling-api.js` in a script tag. See 
  [Creating Custom Labeling Interfaces](#creating-custom-labeling-interfaces)
  to get started.

- **Manage Teams**: Invite your team members to help with labeling tasks. Labelbox
  streamlines your labeling workflows, from micro labeling projects for quick
  R&D to production grade projects requiring hundreds of collaborators.

- **Measure Performance**: Maintain the highest quality standards for your data by
  keeping track of labeling task performance of individuals and teams.

## [Full Documentation](https://support.labelbox.com/docs/getting-started)


## Quickstart
1. Sign up on [Labelbox](https://www.labelbox.com)
2. Jump into the example project or create a new one
3. Step through the setup, attach a data set and start labeling
4. Export your labels

## Creating Custom Labeling Interfaces
You can create custom labeling interfaces to suit the needs of your
labeling tasks. All of the pre-made labeling interfaces are open source.

### A Minimal Example
```html
<script src="https://api.labelbox.com/static/labeling-api.js"></script>
<div id="form"></div>
<script>
function label(label){
  Labelbox.setLabelForAsset(label).then(() => {
    Labelbox.fetchNextAssetToLabel();
  });
}

Labelbox.currentAsset().subscribe((asset) => {
  if (asset){
    drawItem(asset.data);
  }
})
function drawItem(dataToLabel){
  const labelForm = `
    <img src="${dataToLabel}" style="width: 300px;"></img>
    <div style="display: flex;">
      <button onclick="label('bad')">Bad Quality</button>
      <button onclick="label('good')">Good Quality</button>
    </div>
  `;
  document.querySelector('#form').innerHTML = labelForm;
}

</script>
```

### Labelbox Pluggable Interface Architecture
Labelbox allows the use of custom labeling interfaces. Custom labeling interfaces
minimally define a labeling ontology and optionally the look and feel of the
labeling interface. A minimal labeling interface imports `labeling-api.js` and
uses the `fetch` and `submit` functions to integrate with Labelbox. While
Labelbox makes it simple to do basic labeling of images and text, there are a
variety of other data types such as point clouds, maps, videos or medical DICOM
imagery that require bespoke labeling interfaces. With this in mind, Labelbox
is designed to facilitate the creation, installation, and maintenance of custom
labeling frontends.

<img src="https://s3-us-west-2.amazonaws.com/labelbox/documentation.assets/images/architecture.jpg" width="100%">


### Using `labeling-api.js`
To develop a Labelbox frontend, import `labeling-api.js` and use the 2 APIs
described below to `fetch` the next data and then `submit` the label against the
data. Note that multiple data can be loaded in a single `fetch` if a row in the
CSV file contains an array of data pointers.

__Attach the Labelbox Client Side API__

```html
<script src="https://api.labelbox.com/static/labeling-api.js"></script>
```

__Get a row to label__

```javascript
Labelbox.fetchNextAssetToLabel().then((dataToLabel) => {
  // ... draw to screen for user to view and label
});
```

__Save the label for a row__

```javascript
Labelbox.setLabelForAsset(label); // labels the asset currently on the screen
```

### Hello World Example

[Try it in your browser](https://hello-world.labelbox.com)  
(The project must be setup first)

### Full Example
```html
<script src="https://api.labelbox.com/static/labeling-api.js"></script>
<div id="form"></div>
<script>
function label(label){
  Labelbox.setLabelForAsset(label).then(() => {
    Labelbox.fetchNextAssetToLabel();
  });
}

Labelbox.currentAsset().subscribe((asset) => {
  if (asset){
    drawItem(asset.data);
  }
})
function drawItem(dataToLabel){
  const labelForm = `
    <img src="${dataToLabel}" style="width: 300px;"></img>
    <div style="display: flex;">
      <button onclick="label('bad')">Bad Quality</button>
      <button onclick="label('good')">Good Quality</button>
    </div>
  `;
  document.querySelector('#form').innerHTML = labelForm;
}

</script>
```

### [Full API Reference](docs/api-reference.md)

### Reference Interfaces

#### [Image classification interface source code](https://github.com/Labelbox/Labelbox/tree/master/templates/image-classification)
<img src="https://s3-us-west-2.amazonaws.com/labelbox/documentation.assets/images/classification.png" width="400">

#### [Image segmentation interface source code](https://github.com/Labelbox/Labelbox/tree/master/templates/image-segmentation)
<img src="https://s3-us-west-2.amazonaws.com/labelbox/documentation.assets/images/segmentation.png" width="400">

#### [Text classification interface source code](https://github.com/Labelbox/Labelbox/tree/master/templates/text-classification)
<img src="https://s3-us-west-2.amazonaws.com/labelbox/documentation.assets/images/text-classification.png" width="400">

### Local Development of Labeling Interfaces
Labeling interfaces are developed locally. Once the interface is ready to use,
it is installed in Labelbox by pointing to a hosted version of the interface.

**Run localhost server**
1. Start the localhost server in a directory containing your labeling frontend
   files. For example, run the server inside `templates/hello-world` to run the
   hello world labeling interface locally.
```
python -m SimpleHTTPServer
```

2. Open your browser and navigate to the `localhost` endpoint provided by the
   server.
  
3. Customize the labeling frontend by making changes to `index.html`. Restart the
   server and refresh the browser to see the updates.

![](https://s3-us-west-2.amazonaws.com/labelbox/labelbox_localhost.gif)

### Installing a Labeling Frontend in labelbox.com
When you are ready to use your custom labeling interface on
[Labelbox](https://www.labelbox.com), upload your `index.html` file to a cloud
service that exposes a URL for Labelbox to fetch the file. If you don't have a
hosting service on-hand, you can quickly get setup with
[Now](https://zeit.co/now) from **Zeit**.

**Custom Interface Hosting Quickstart with [Now](https://zeit.co/now)**
* Create an account at Zeit, download and install Now here: https://zeit.co/download
* With Now installed, navigate to the directory with your labeling interface
  (where your `index.html` is located) and launch Now in your terminal by typing `now`. The
  Now service will provide a link to your hosted labeling interface file.

* Within the *Labeling Interface* menu of the *Settings* tab of your
  Labelbox project, choose *Custom* and paste the link in the *URL to
  labeling frontend* field as shown in the video below.
  
![](https://s3-us-west-2.amazonaws.com/labelbox/labelbox_cloud_deploy.gif)

## Request features
Have a feature request? Need a custom labeling interface built for your labeling task? We can help.  

Create an issue here: https://github.com/Labelbox/Labelbox/issues or contact us at support@labelbox.com

## Terms of use, privacy and content dispute policy
Here is our [terms of service, privacy and content dispute policy](https://www.dropbox.com/s/ph6w2ov4i4v5pf9/Labelbox_Terms_Privacy_Content.pdf?dl=0)
