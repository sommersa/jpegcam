# 2014-03-04 Check out WebcamJS on Github #

Hey guys, I have released a new webcam capture library called WebcamJS, which is available on Github.  It uses HTML5 getUserMedia (Chrome / Firefox), but transparently falls back to Adobe Flash when necessary.  If you are interested, here is the link:

[WebcamJS on GitHub](https://github.com/jhuckaby/webcamjs)

# Development Has Ceased On JPEGCam #

Hey everyone, I am very sorry to report that development on JPEGCam has ended.  I just don't have the time to maintain it anymore.  I deeply apologize to everyone, and hope that someone picks up the reins.  You have my permission to copy the source code and host it on GitHub or wherever.  No attribution required.

# JPEGCam #

Flash + Javascript library which allows you to display a variable-sized Flash movie in your page that captures Webcam snapshots (still frames), and submits them to your server in JPEG format.  Sample PHP 5 code included for receiving submissions and saving them to disk.

The Flash movie first activates the webcam and allows the user to make adjustments before submitting.  All controls for displaying the Flash device configuration panel and taking snapshots is handled from Javascript.  This way you can control the user interface, using your own buttons and layout.  The Flash movie consists of nothing except a full-canvas camera control with NO flash user interface elements.

See the [Wiki](http://code.google.com/p/jpegcam/w/list) for complete instructions.

Here is a live [Test Page](http://bowser.effectgames.com/~jhuckaby/jpegcam/) where you can try it out for yourself.  The test page HTML and PHP code are both included in the source.  Requires Flash Player 9 and a Webcam attached to your PC.

## Version 1.0.9 Released ##

This version introduces a new, optional "stealth" feature.  Using this you can take one or more snapshots and upload them without interrupting the video stream.  Thanks to user 'praveen.baratam' for this feature request!

Here is a [stealth test page](http://bowser.effectgames.com/~jhuckaby/jpegcam/stealth.html) that demonstrates the feature.

## Version 1.0.8 Released ##

This version auto-detects Mac iSight cameras and selects them by default.  Suggestion by manuel.gonzalez.noriega, code copied from [this article](http://www.squidder.com/2009/03/09/trick-auto-select-mac-isight-in-flash/).

## Version 1.0.7 Released! ##

In this version you can now specify a smaller video preview size, but a larger upload size.  The image is not "upscaled" for submit, but is actually captured at the larger size.  Here is a [test page](http://bowser.effectgames.com/~jhuckaby/jpegcam/test_resize_enlarge.html) that demonstrates the feature.  Thanks to user onlineventures for this feature request!

## Version 1.0.6 Released! ##

This version fixes a bug where systems without a camera attached failed to throw an error.  Thanks to Mark Pace for catching this!

## Version 1.0.5 Released! ##

New in version 1.0.5 is the ability to automatically resize images prior to upload.  For example, certain webcams can only capture at 320x240, but you may want a smaller image uploaded to your server.

Please check out the new [Resize Test Page](http://bowser.effectgames.com/~jhuckaby/jpegcam/test_resize.html) which shows the feature in action.

## Version 1.0.4 Released! ##

New in version 1.0.4 is the ability to provide separate "Capture" and "Upload" buttons, so the user may take multiple photos and decide which one looks best before uploading.  Also, several performance improvements including disconnecting the camera and freezing the image when the image is compressed and uploaded.

Here is a [New Test Page](http://bowser.effectgames.com/~jhuckaby/jpegcam/test2.html) where you can try it out for yourself.  The test page HTML and PHP code are both included in the source.  Requires Flash Player 9 and a Webcam attached to your PC.