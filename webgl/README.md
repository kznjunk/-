# WebGL and BabylonJS

## Overview

[Introduction](##introduction)
- [History](#history)
- [Main elements for a 3D scene](#main-elements-for-a-3D-scene)
  - [The scene](#the-scene)
  - [The graphic engine](#the-graphic-engine)
  - [The camera](#the-camera)
  - [The lights](#the-lights)
  - [Meshes (also objects)](#meshes-(also-objects))
  - [A shader](#a-shader)
  - [The loop render](#the-loop-render)
- [First lines of code](#first-lines-of-code)
  - [The heart](#the-heart)
  - [The player](#the-player)
  - [The map](#the-map)
- [Meshes](#meshes)
  - [Ground](#ground)
  - [Cube](#cube)
  - [Sphere](#sphere)
  - [Cylinder](#cylinder)
- [Lights](#lights)
  - [Hemispherical lights](#hemispherical-lights)
  - [Directional lights](#directional-lights)
  - [Ponctual lights](#ponctual-lights)
  - [Spot](#spot)
- [Camera](#camera)
  - [Free camera](#free-camera)
  - [ArcRotate camera](#arcrotate-camera)


## Introduction

### History

**WebGL** is an OpenGL API created by the Khronos Group where it is possible to create some 3D apps inside a browser.

- `OpenGL` has been created in 1992 to do some 2D and 3D maths.
- `OpenGL Embedded Systems` is a specification created for mobile console and web.
- The `Khronos group` is a consortium of big companies such as Apple, Google, Nvidia, Intel, ... that are working to create free stuff

**BabylonJS** is a JavaScript library that allows to develop scenes inside a canvas tag with WebGL.
It has been created by David Catuhe and David Rousset (Microsoft's developers) in 2013.

### Main elements for a 3D scene

#### The scene

This is an object that will contain all the others. It is not a visible. [In cinéma: It is the place where all the actors will play and the action will happen. It stops to what the camera is seeing, even if the camera can go everywhere].


#### The graphic engine

The scene contains the elements, and the graphic engine's job is to render them inside an image (handling the visible stuff in the screen). It will transform maths into a visible image.
It is not a physic engine - this one gonna calculate the trajectories and gravity.

#### The camera

This is all about checking what to look for!

#### The lights

There is scene that contains a camera looking at a specific place with a graphic engine ready to receive the first calculations to do. Yet if we add an object, everything would be in the dark. Lights are there to enlight objects and create shadows.

#### Meshes (also objects)

They are the first visible elements. They are composed by several points to represents polygones and then faces.
Every object in screen is a mesh.

#### A shader

A shader is an element associated to a mesh that will say to the graphic engine to react according to several parameters (réfraction et réflexion de la lumière, couleur, matériaux).
It will create shadows, colors, lights on objects, ...

#### The loop render

With everything above, it remains to ask the scene to render what has been created and to show it in the browser's tab. It's a requirement to do this several times to show some images per second - most of the time 60 FPS.

## First lines of code

### The heart (`Heart.js`)

```js
// Once DOM ready, here we go
document.addEventListener('DOMContentLoaded', function () {
    new Game('renderCanvas')
}, false)

Heart = function (canvasId) {
    var canvas = document.getElementById(canvasId)
    // Will create a graphic engine based on Babylon and related to the canvas tag
    var engine = new BABYLON.Engine(canvas, true)
    // For the loop of the class
    var _this = this
    
    // Initialization of the scene with function associated to the Game object
    this.scene = this._initScene(engine)
    
    // Init a player and the map
    var _player = new Player(_this, canvas)
    var _map = new Map(_this)

    // Generate the frames. Everything defined over there will be render as soon as possible
    engine.runRenderLoop(function () {
        _this.scene.render()
    })

    // Re-calculation while resizing
    window.addEventListener('resize', function () {
        if (engine) {
            // Native function
            engine.resize()
        }
    }, false)
}

Game.prototype = {
    // Initialization prototype of the scene
    _initScene: function (engine) {
        // Everything will be contain inside the scene
        var scene = new BABYLON.Scene(engine)
        // Define the background color of the app (it should not be visible in the future)
        scene.clearColor = new BABYLON.Color3(0, 0, 0)
        return scene
    }
}
```

### The player (`Player.js`)

```js
Player = function (game, canvas) {
    // The scene of the game
    this.scene = game.scene

    // Initialization of the camera
    this._initCamera(this.scene, canvas)
    
}

Player.prototype = {
    _initCamera: function (scene, canvas) {
        // Creation of the camera
        this.camera = new BABYLON.FreeCamera('camera', new BABYLON.Vector3(0, 5, -10), scene)

        // Set the camera to look at the zero point of the scene
        this.camera.setTarget(BABYLON.Vector3.Zero())

        // Attach the control of the camera to the canvas
        this.camera.attachControl(canvas, true)
    }
}
```

### The map (`Map.js`)

```js
Map = function (game) {
    this.game = game
    var scene = game.scene

    // Creation of the main light
    var light = new BABYLON.HemisphericLight('light1', new BABYLON.Vector3(0, 1, 0), scene)

    // Sphere
    var sphere = BABYLON.Mesh.CreateSphere('sphere1', 16, 2, scene)
    sphere.position.y = 1

    // Ground (so the sphere is somewhere)
    var ground = BABYLON.Mesh.CreateGround('ground1', 6, 6, 2, scene)
}
```

## Meshes:

Grounds, Squares, Sphere, Cylindre, .. these objects have commun properties:
- `name` (string): id of the object
- `size`: size of the object
- `scene`: scene that will contain the object
- `updatable` (boolean): is the mesh will change (size, properties) later
- `orientation`: way that the faces will be displayed
- `subdivision`: level of details of the object (the bigger, the more detailled it will be - number of points)

There are 3 types of updates with the Vector3:
- `position`: move the mesh
- `rotation`: rotate based on a rad angle
- `scaling`: multiply the size

When a mesh has been created at 0, 0, 0, and that the ground is at 0, the center of the object will be in the middle of the other one. All the thing is to move it up of half of its side.

Then, BabylonJS applies a standard white material to the objects. A material is like every other object but without physic. There are 3 types of application:
- `Ambiant`: general light (ambiante) of the object
- `Specular`: shiny aspect of the object
- `Emissive`: light generated by the object itself

With this, there are 2 types of application:
- `Reflexion`
- `Refraction`

### Ground:

```js
var ground = BABYLON.Mesh.CreateGround(name, width, depth, subdivision, scene)
```

Example:
```js
// Init the material
var materialGround = new BABYLON.StandardMaterial('groundTexture', scene)
materialGround.diffuseTexture = new BABYLON.Texture('assets/images/brick.jpg', scene)
materialGround.diffuseTexture.uScale = 4.0
materialGround.diffuseTexture.vScale = 4.0

// 20x20 ground
var ground = BABYLON.Mesh.CreateGround('ground1', 20, 20, 2, scene)
// Axis X: *2
// Axis Z: *3
// Axis Y: nothing, it is a ground!
ground.scaling = new BABYLON.Vector3(2, 10, 3)
// Another way
ground.scaling.z = 2
// Material
ground.material = materialGround
```

### Cube:

```js
// Simple
var box = BABYLON.Mesh.CreateBox(name, size, scene)

// Advanced
var box = BABYLON.Mesh.CreateBox(name, size, scene, updatable, orientation)
```

Example:
```js
// Init the material
var materialWall = new BABYLON.StandardMaterial('groundTexture', scene)
materialWall.diffuseTexture = new BABYLON.Texture('assets/images/wood.jpg', scene)

// First cube as model
var mainBox = BABYLON.Mesh.CreateBox('box1', 3, scene)
mainBox.material = materialWall

// 3 clones
var mainBox2 = mainBox.clone('box2')
var mainBox3 = mainBox.clone('box3')
var mainBox4 = mainBox.clone('box4')

// Multiply the size of the object per K on the Y Axis
mainBox.scaling.y = 1
mainBox2.scaling.y = 2
mainBox3.scaling.y = 3
mainBox4.scaling.y = 4

// Move the objects on the 3 axis
// Move it up from half of the height and handle the scaling height by what we did before
mainBox.position = new BABYLON.Vector3(5, ((3 / 2) * mainBox.scaling.y), 5)
mainBox2.position = new BABYLON.Vector3(5, ((3 / 2) * mainBox2.scaling.y), -5)
mainBox3.position = new BABYLON.Vector3(-5, ((3 / 2) * mainBox3.scaling.y), -5)
mainBox4.position = new BABYLON.Vector3(-5, ((3 / 2) * mainBox4.scaling.y), 5)

// 45deg in radian (no need for the other thanks to clone)
mainBox.rotation.y = (Math.PI * 45) / 180
```

### Sphere:

```js
// Simple
var sphere = BABYLON.Mesh.CreateSphere(name, segments, size, scene)

// Advanced
var sphere = BABYLON.Mesh.CreateSphere(name, segments, size, scene, updatable,  orientation)
```
- `segments`: quality of the object

### Cylinder:

```js
// Simple
var cylinder = BABYLON.Mesh.CreateCylinder(name, height, diamTop, diamBottom, tesselation, subdivision, scene)

// Advanced
var cylinder = BABYLON.Mesh.CreateCylinder(name, height, diamTop, diamBottom, tesselation, subdivision, scene, updatable, orientation)
```

- `diamTop`: width of the top colomn
- `diamBottom`: width of the bottom colomn
- `tesselation`: quality of the object

Example:
```js
var cylinder = BABYLON.Mesh.CreateCylinder('cyl1', 20, 5, 5, 20, 4, scene)
cylinder.material = materialWall
```

## Lights:

Like for the objects, there are 3 types of updates with the Vector3:
- `position`: move
- `rotation`: rotate
- `scaling`: multiply the size (no effect at all)

As an important note, only 3 lights can touch a mesh. Yet there is a way to avoid this issue.

A light will always has 2 value to set:
- `diffuse`: define the oclor of the light (default: 1, 1, 1)
- `specular`: define the intensity of the shine (the more it is white, the more it is shiny; the more it is black, the more it is mat) (default: 1, 1, 1)
- `intensity`: it is also possible to decrease the intensity of the lights with this parameter

For nice effects, it is pretty cool to use the `range` function.

### Hemispherical lights:

Its lighting is very close to the sky's one, with soft shadows. It is represented with an infinite plan and it's enlightened in one single direction.

The parameters are:
- light name
- position from where it will enlight
- the current scene

```js
var light = new BABYLON.HemisphericLight('light1', new BABYLON.Vector3(0, 20, 0), scene)
light.diffuse = new BABYLON.Color3(1, 1, 1)
light.specular = new BABYLON.Color3(1, 1, 1)
```

### Directional lights:

This one is close to the previous one but the second parameter here is the light direction:
- light name
- direction of the light
- the current scene

```js
var light1 = new BABYLON.DirectionalLight('Dir1', new BABYLON.Vector3(0, -1, 0), scene)
light1.diffuse = new BABYLON.Color3(1, 1, 1)
light1.specular = new BABYLON.Color3(1, 1, 1)
```

### Ponctual lights:

This light can't diffuse shadows, but its particularity is to diffuse in all the directions. It's useful for mood lights:

The parameters are:
- light name
- position of the light
- the current scene

```js
var light1 = new BABYLON.PointLight('Omni1', new BABYLON.Vector3(1, 10, 1), scene)
light1.diffuse = new BABYLON.Color3(1, 1, 1)
light1.specular = new BABYLON.Color3(1, 1, 1)
```

### Spot:

Finally, this one is working like a physical spot: it's a cone of light. Use cases are mainly for moving lights inside a scene.

The parameters are:
- light name
- position of the light
- direction of the light
- open angle of the cone of light
- the exponent of diminishing light with distance (the more it is high, the more the light decrease quickly with the distance)
- the current scene

```js
var light1 = new BABYLON.SpotLight('Spot1', new BABYLON.Vector3(0, 30, -10), new BABYLON.Vector3(0, -1, 0), 0.8, 2, scene)
light1.diffuse = new BABYLON.Color3(1, 1, 1)
light1.specular = new BABYLON.Color3(1, 1, 1)
```

## Camera:

There are 2 basics camera on BabylonJS, but several ones have been created for specific use cases (anaglyphe, Occulus, follow a target, ..).

### Free camera:

This one allows you to have right at the beginning mouse and keyboard controls. Pretty close than what it is used for FPS.

The parameters are:
- an id
- a camera position
- a scene

There are also two important parameters to define:
- `setTarget`: is about where the camera should look at the scene creation (`BABYLON.Vector3.Zero()` can be used to look at the center 0 of the scene)
- `attachControl`: render the camera enable with the keyboard an the mouse

```js
var camera = new BABYLON.FreeCamera('camera', new BABYLON.Vector3(0, 5, -10), scene)
camera.setTarget(BABYLON.Vector3.Zero())
camera.attachControl(canvas, true)
```

### ArcRotate camera:

This one is specific to turn around a defined point and to be always centered on it.

The parameters are:
- an id
- an alpha value (horizontal in rad)
- a beta value (vertical in rad)
- a ray (radius)
- the pivot position
- the scene

```js
var camera = new BABYLON.ArcRotateCamera('ArcRotateCamera', 1, 0.8, 10, new BABYLON.Vector3(0, 0, 0), scene)
camera.attachControl(canvas, true)
```
