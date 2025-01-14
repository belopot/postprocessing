---
layout: single
collection: sections
title: Getting Started
draft: false
menu: main
weight: 30
---

# Getting Started

## Installation

This library requires the peer dependency [three](https://github.com/mrdoob/three.js/). Check the [release notes](https://github.com/vanruesc/postprocessing/releases) for supported versions.

```sh
npm install three postprocessing
```

## Build Setup

To get started, pick a bundler of your choice:
- [esbuild](https://github.com/evanw/esbuild)
- [swc](https://github.com/swc-project/swc)
- [rollup](https://github.com/rollup/rollup)
- [webpack](https://github.com/webpack/webpack)
- [parcel](https://github.com/parcel-bundler/parcel)

This list is not exhaustive, so feel free to look around for another tool that best suits your needs. For more information about configuration, features and plugins, see the official documentation of your chosen bundler.

### Example

The following example uses esbuild for bundling.

__package.json__

```json
{
	"scripts": {
		"build": "esbuild src/app.js --bundle --minify --outfile=dist/app.js"
	},
	"dependencies": {
		"postprocessing": "x.x.x",
		"three": "x.x.x"
	},
	"devDependencies": {
		"esbuild": "x.x.x"
	}
}
```

__src/app.js__

```js
import { RenderPipeline } from "postprocessing";
console.log(RenderPipeline);
```

Install [node.js](https://nodejs.org) and use the command `npm run build` to generate the code bundle. Include the bundle in an HTML file to run it in a browser.

## Usage

Postprocessing extends the common rendering workflow with fullscreen image manipulation tools. The following WebGL attributes should be used for an optimal workflow:

```js
import { WebGLRenderer } from "three";

const renderer = new WebGLRenderer({
	powerPreference: "high-performance",
	antialias: false,
	stencil: false,
	depth: false
});
```

[RenderPipelines]() are used to group passes. Common setups will only require one pipeline that contains a [ClearPass](), a [GeometryPass]() and one or more [EffectPass]() instances. The latter is used to render fullscreen [Effects](). Please refer to the [three.js manual](https://threejs.org/docs/#manual/en/introduction/Creating-a-scene) for more information on how to setup the renderer, scene and camera.

```js
import {
	BloomEffect,
	ClearPass,
	EffectPass,
	GeometryPass,
	RenderPipeline
} from "postprocessing";

const renderer = ...;
const scene = ...;
const camera = ...;

const pipeline = new RenderPipeline(renderer);
pipeline.addPass(new ClearPass());
pipeline.addPass(new GeometryPass(scene, camera, { samples: 4 }));
pipeline.addPass(new EffectPass(new BloomEffect()));

requestAnimationFrame(function render() {

	requestAnimationFrame(render);
	pipeline.render();

});
```
