# Introduction #

JPEGCam is a simple, Javascript and Flash library that allows you to enable
your users to submit Webcam snapshots to your server in JPEG format.  The
Flash movie is variable-sized, and has no visible user interface controls.
All commands sent to the movie are done so from Javascript, so you can
implement your own look & feel on your site, create your own buttons, and
tell the Flash movie what to do from your own code.

# Requirements #

  * **Javascript-enabled browser**
    * Tested on:
      * Safari 3.0 Mac OS X Leopard
      * Firefox 2.0 Mac OS X Leopard
      * MSIE 6.0, 7.0 Windows XP SP2
      * Firefox 2.0 Windows XP SP2
      * Chrome 0.3 Beta Windows XP SP2
  * **Flash Player 9 & 10**

# Installation #

(For a working example, see "test.html" in the htdocs folder.)

First, copy the following files to your web server:

  * webcam.js
  * webcam.swf
  * shutter.mp3

# Embedding In Your Page #

To use the tool in your page, edit your HTML (or PHP or whatever) and load the JavaScript library:

```
  <script type="text/javascript" src="webcam.js"></script>
```

Next, configure a few settings to taste (see [APIDocs](APIDocs.md) for all the available API calls you can make):

```
<script language="JavaScript">
	webcam.set_api_url( 'test.php' );
	webcam.set_quality( 90 ); // JPEG quality (1 - 100)
	webcam.set_shutter_sound( true ); // play shutter click sound
</script>
```

Next, load the movie into the page.  If you want to load the movie immediately,
simply use `document.write()` as shown below.  If you are designing a DHTML
application, you can call `webcam.get_html(...)` at any time to dynamically
populate a DIV or other element after the page is finished loading.

```
<script language="JavaScript">
	document.write( webcam.get_html(320, 240) );
</script>
```

Add some controls for sending commands to the movie (see [APIDocs](APIDocs.md) for a complete list of commands).

```
<br/><form>
	<input type=button value="Configure..." onClick="webcam.configure()">
	&nbsp;&nbsp;&nbsp;
	<input type=button value="Take Snapshot" onClick="webcam.snap()">
</form>
```

Finally, add some code for handling the server response:

```
<script language="JavaScript">
	webcam.set_hook( 'onComplete', 'my_callback_function' );
	function my_callback_function(response) {
		alert("Success! PHP returned: " + response);
	}
</script>
```

That's it! See [APIDocs](APIDocs.md) for a complete list of all the available API calls.  See the next section for how to write the server-side receiver.

# Server Side Code #

The Flash movie makes a HTTP POST to your server-side script, using the
Content-Type 'image/jpeg'.  This is a NON-STANDARD method which is unlike
submitting a form from a web page.  If you are using PHP, the JPEG data
will NOT be in the normal `$_POST` associative array.  Instead, you should read it from the special PHP wrapper: 'php://input'.  For example:

```
  $jpeg_data = file_get_contents('php://input');
```

You can write this raw, binary JPEG data to a file handle using the PHP
function `file_put_contents()`:

```
$jpeg_data = file_get_contents('php://input');
$filename = "my_file.jpg";
$result = file_put_contents( $filename, $jpeg_data );
```

Any output from your script is passed back through the Flash movie to the
JavaScript code, which in turn passes it to your onComplete callback function.

For example, if you want your script to pass back a URL to the JPEG image,
save the file where you want it, and construct a URL to the file.  Then simply
print the URL to the output like this:

(This assumes you are saving the files to the current working directory)

```
$url = 'http://' . $_SERVER['HTTP_HOST'] . dirname($_SERVER['REQUEST_URI'])
 	. '/' . $filename;
print "$url\n";
```

(See "test.php" for a working example.)

# FAQ #

Q. **I cannot see the image from my camera!  What am I doing wrong?**

A. You probably have to setup the camera device in the Flash Camera settings dialog first.  Often Flash doesn't auto-detect the right device.

```
webcam.configure( 'camera' );
```

It is always a good idea to provide a "Configure..." button on your page which calls this function, so users can easily get to it.

Q. **What is this ugly permission dialog?  Can't I just make it remember me?**

A. Yes, you certainly can!  In the Flash setup dialogs, click on the 2nd icon from the left (i.e. Privacy Settings), and you can click "Allow", then check the "Remember" checkbox.

You can send your users directly to the Privacy config panel by calling:

```
webcam.configure( 'privacy' );
```

A cool trick is to detect "new" users (via a cookie) and register an onLoad handler to send them directly to the Privacy settings.

```
webcam.set_hook( 'onLoad', 'my_load_handler' );
function my_load_handler() {
	if (is_new_user())
		webcam.configure( 'privacy' );
}
```

Of course, you have to write the is\_new\_user() function yourself.  I no wanna be settin' no cookies on your domain.