# hugo-shortcode-unity-webgl
A Hugo shortcode to embed a Unity WebGL player

## Usage
Run a WebGL build in Unity, and take note of the folder name you save your build into. This folder name will determine the file names of files within the `Build` directory, and is used in the shortcode.

Host the WebGL build files somewhere, e.g. AWS S3, Azure Blob, etc. Ensure CORS settings allow your files to be loaded from the domain you are using this shortcode. Or just put it right inside your static site's storage and don't worry about CORS.

Shortcode example:

```
{{< unity-webgl-player 
    gameTitle="Build 001"
    width="1024" 
    height="576"
    buildURL="https://somewhere.com/path/to/files/Build" 
    buildFileName="webgl"
    playerID=""  >}}
```

**buildURL** is the location of the "Build" directory.

**buildFileName** is the name of the files (without extensions) that Unity creates from your build. E.g. if you saved your build into a folder called "webgl", the build files will `webgl.data`, `webgl.framework.js`, `webgl.loader` and `webgl.wasm`. So the buildFileName for this case should be 'webgl'.

**playerID** is used to allow multiple instances on a single page. Must be unique per page. You can leave blank if only one player on the page.


## How it's made
I'm a Hugo noob — but it seems to work fine for me. Comments/issues/etc welcome.

I just took the default Unity WebGL player template, customised it slightly (e.g. added a loading button) and then converted it to a Hugo shortcode. In order to allow multiple players per page, I'm simply:
- converting the CSS to use classes instead of ID's
- appending "playerID" to the end of each HTML element ID and JavaScript variable related to the player. There might be a better way, but this works!

The CSS has been un-SCSS-ified using https://jsonformatter.org/scss-to-css.

## Known issues
Multiple Unity WebGL players on a single page seem to all receive input, regardless of which player has focus. This can be resolved *inside the Unity build* by using `WebGLInput.captureAllKeyboardInput` property. More info here: https://docs.unity3d.com/Manual/webgl-input.html.

## TODO
- Make the player responsive, with aspect ratio maintained. Currently it's fixed size based on the shortcode width & height params and won't resize (unless user clicks the fullscreen toggle).