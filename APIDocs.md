# Javascript API Calls #

Here are all the available API calls for the JPEGCam Javascript library.
Everything is under a top-level global 'webcam' namespace.

## webcam.set\_hook( HOOK\_NAME, USER\_FUNCTION ); ##

This allows you to set a user callback function that will be fired for
various events in the JPEGCam system.  Here are all the events you
can hook:

  * **onLoad**
    * Fires when the Flash movie is loaded on the page.  This is useful for knowing when the movie is ready to receive scripting calls.

  * **onComplete**
    * Fires when the JPEG upload is complete. Your function will be passed the raw output from the API script that received the file upload, as the first argument.

  * **onError**
    * Fires when an error occurs.  If this hook is not defined, the library will display a simple JavaScript alert dialog.  Your function will be passed the error text as the first argument.

**Example:**

```
webcam.set_hook( 'onComplete', 'my_callback_function' );

function my_callback_function(response) {
  alert("Success! PHP returned: " + response);
}
```

## webcam.set\_api\_url( URL ); ##

This allows you to set the URL to your server-side script that will
receive the JPEG uploads from the Flash movie. Beware of cross-domain
restrictions in Flash.

**Example:**

```
webcam.set_api_url( '/path/to/myscript.php' );
```

## webcam.set\_swf\_url( URL ); ##

This allows you to set the URL to the location of the "webcam.swf" Flash
movie on your server.  It is recommended to keep this file in the same
directory as your HTML page, but if that is not possible, set the path
using this function.  Beware of cross-domain restrictions in Flash.
The default is the current directory that your HTML page lives in.

**Example:**

```
webcam.set_swf_url( '/path/to/webcam.swf' );
```

## webcam.set\_quality( QUALITY ); ##

This allows you to adjust the JPEG compression quality of the images
taken from the camera.  The range is 1 - 100, with 1 being the lowest
quality (but smallest size files), to 100 being the highest quality
(but largest files).  This does NOT control the resolution of the images,
only the JPEG compression.  The default is 90.

**Example:**

```
webcam.set_quality( 90 );
```

## webcam.set\_shutter\_sound( ENABLED, URL ); ##

This allows you to enable or disable the "shutter" sound effect that
the Flash movie makes when a snapshot is taken.  Pass in a boolean
true or false to the function.  It defaults to true.  If set to false
the sound effect will not even be loaded.

You can optionally pass a second argument to this function, which
should be a URL (relative to page or fully qualified) to an MP3
sound effect for the shutter sound.  This defaults to 'shutter.mp3'
in the current directory relative to the HTML page.

These values cannot be changed after get\_html() is called (see below).

**Example:**

```
webcam.set_shutter_sound( true, 'shutter.mp3' );
```

## webcam.get\_html( WIDTH, HEIGHT, SERVER\_WIDTH, SERVER\_HEIGHT ); ##

This returns the necessary HTML code to embed the Flash movie into your
page.  Pass in the desired pixel width & height, which not only controls
the visual size of the movie, but also the JPEG image width & height.
Standard sizes are 320x240 and 640x480.

You can optionally pass a desired server image width and height.  If
these differ from the video width and height, the captured images will
be resized to match just prior to upload.

**Example:**

```
document.write( webcam.get_html(320, 240) );
```

## webcam.snap(); ##

This instructs the Flash movie to take a snapshot and upload the JPEG
to the server.  Make sure you set the URL to your API script using
`webcam.set_api_url()`, and have a callback function ready to receive
the results from the server, using `webcam.set_hook()`.

**Example:**

```
<a href="javascript:void(webcam.snap())">Take Snapshot</a>
```

## webcam.configure( PANEL ); ##

This launches one of Flash's configuration panels, used to setup camera
devices, privacy settings, and more.  Pass in one of the following strings
which sets the default panel "tab" in the settings dialog:
"camera", "privacy", "default", "localStorage", "microphone", or
"settingsManager".

**Example:**

```
<a href="javascript:void(webcam.configure('camera'))">Configure...</a>
```

## webcam.freeze(); ##

**Optional, new in v1.0.4**.  This is not required if you use webcam.snap(),
described above.

This captures an image from the webcam but does NOT upload it.
Instead, the image is displayed "frozen" in the Flash movie, and the user
may take further action.  For example, you may provide separate "Upload"
and "Reset" buttons to upload the frozen image and/or reset the camera.

**Example:**

```
<a href="javascript:void(webcam.freeze())">Freeze</a>
```

## webcam.upload(); ##

**Optional, new in v1.0.4**.  This is not required if you use webcam.snap(),
described above.

This uploads the captured image to the server, previously frozen with
`webcam.freeze()`.  This is provided as its own function so you can
have separate "Capture" and "Upload" buttons for the user.

**Example:**

```
<a href="javascript:void(webcam.upload())">Upload</a>
```

## webcam.reset(); ##

**Optional, new in v1.0.4**.  This resets the frozen image, previously captured
with webcam.freeze(), and restores the live webcam feed for further
capturing.

**Example:**

```
<a href="javascript:void(webcam.reset())">Reset</a>
```

## webcam.set\_stealth( ENABLED ); ##

**Optional, new in v1.0.9**.  This sets or disables "stealth" mode (defaults to disabled).  When enabled, this causes the image to be captured and uploaded without interrupting the video preview.  Meaning, the snapshot is not "frozen", but instead the webcam video continues to be played.  Using this mode you can, for example, capture and upload multiple snapshots at preset intervals.

**Example:**

```
webcam.set_stealth( true );
```