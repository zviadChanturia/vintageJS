# vintageJS
Add a retro/vintage effect to images using the HTML5 canvas element.

## How to use

```javascript
vintagejs :: TSourceElement -> $Shape<TEffect> -> Promise<TResult>
```

The `vintagejs` function takes two arguments, a source element (image or canvas) and an effect object and returns a promise that resolves to a result object with the following methods:

```javascript
// returns the data url of the updated image. Use it to update the source of an existing image
getDataURL(mimeType?: string, quality?: number): string;
// returns the canvas with the updated image. Use it to draw your changes onto another canvas
getCanvas(): HTMLCanvasElement;
// returns a promise that resolves to an HTMLImageElement of the updated image
genImage(mimeType?: string, quality?: number): Promise<HTMLImageElement>;
```

If not provided, mimeType defaults to `image/jpeg` and quality defaults to `1`.

### Examples

```javascript
// use an image as source and update image with data url
const srcEl = document.querySelector('img.myImage');
vintagejs(srcEl, { brightness: 0.2 })
  .then(res => {
    srcEl.src = res.getDataURL();
  });

// use a canvas as source and draw result to canvas
const srcEl = document.querySelector('canvas.myCanvas');
const ctx = srcEl.getContext('2d');
vintagejs(srcEl, { brightness: 0.2 })
  .then(res => {
    ctx.drawImage(res.getCanvas(), 0, 0, srcEl.width, srcEl.height);
  });
```

## Effect options

All properties on the effect object are optional. If you provide them however, you must follow these types:

```javascript
type TEffect = {
  curves: false | TCurve,     // default: false
  screen: false | TRGBAColor, // default: false
  saturation: number,         // float between 0 and 1, default: 1
  vignette: number,           // float between 0 and 1, default: 0
  lighten: number,            // float between 0 and 1, default: 0
  viewfinder: false | string, // string must be URL, default: false
  sepia: boolean,             // default: false
  brightness: number,         // float between -1 and 1, default: 0
  contrast: number,           // float between -1 and 1, default: 0
};

// every channel, r=red, g=green, b=blue serves as a look up table for color mappings
type TCurve = {
  r: Array<Uint8> | Uint8ClampedArray, // array of int between 0 and 255, length of array === 256
  g: Array<Uint8> | Uint8ClampedArray, // array of int between 0 and 255, length of array === 256
  b: Array<Uint8> | Uint8ClampedArray, // array of int between 0 and 255, length of array === 256
};

type TRGBAColor = {
  r: Uint8,  // int between 0 and 255
  g: Uint8,  // int between 0 and 255
  b: Uint8,  // int between 0 and 255
  a: number, // float between 0 and 1
};
```

### Examples

```javascript
const noEffect = {};

const effect_1 = {
  brightness: -0.2,
  contrast: 0.15,
};

const effect_2 = {
  brightness: 0.1,
  vignette: 0.3,
  viewfinder: './film-1.jpg',
  screen: {
    r: 227,
    g: 12,
    b: 169,
    a: 0.15,
  },
};
```

See examples folder for more examples.

## Browser support
Check support for the canvas element [canisue.com/canvas](http://caniuse.com/canvas).

Higher performance when canvas blend modes are supported [caniuse.com/#feat=canvas-blending](http://caniuse.com/#feat=canvas-blending), but fallbacks are implemented.

## License

[MIT](http://www.opensource.org/licenses/mit-license.php)

## Changelog

### Version 2.0.0 - <small>Mar, 2017</small>
* Rewrite from ground up
* Functional API

### Version 1.1.5 - <small>May 16, 2016</small>
* Added "main" field to package.json

### Version 1.1.4 - <small>Oct 24, 2013</small>
* Added universal module definition (umd) wrapper

### Version 1.1.3 - <small>Nov 19, 2013</small>
* Added minified versions
* Fixed same-origin error

### Version 1.1.2 - <small>Jul 24, 2013</small>
* added AngularJS support thanks to [@dpiccone](https://github.com/dpiccone)
* grunt based build script for all versions

### Version 1.1.1 - <small>May 20, 2013</small>
* performance improvements
* new effect options:
    * brightness
    * contrast

### Version 1.1.0 - <small>May 19, 2013</small>
* New and faster core

### Version 1.0.0 - <small>Mar 17, 2011</small>
* Initial release
