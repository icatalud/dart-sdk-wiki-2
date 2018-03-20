# Dart Web Libraries Chrome 63 WebIDLs

The Dart Web Libraries have not been updated since Chrome 50.  With this release these libraries have been rev’d to the Chrome 63 APIs (WebIDLs).  Below are the known differences between the Chrome 50 and Chrome 63 Dart Web libraries.  These are the changes that have affected Dart user’s code:

* Touch and TouchEvent change
    - _initTouchEvent removed see [Chrome Change](https://www.chromestatus.com/features/4923255479599104)
    - Example of [Migrating initTouchEvent to Map](https://developers.google.com/web/updates/2016/09/chrome-54-deprecations#use_of_inittouchevent_is_removed)
* TouchEvent constructor takes a map argument, strong mode catches these failures in our tests.
* Web_audio
AudioScheduledSourceNode.start (inherits in OscillatorNode too) e.g., operation void start([num when]);
AudioBufferSourceNode.start([num when, num grainOffset, num grainDuration]) can't be overloaded void start2([num when, num grainOffset, num grainDuration])
Attributes of type double changed to type num table of attributes changed.
onWheel event exposed
Created for union of two kinds of canvas HTMLCanvasElement and OffscreenCanvas interface WebGLCanvas
all other RenderingContext
readonly attribute WebGLCanvas canvas;
interface WebGLRenderingContext
readonly attribute HTMLCanvasElement canvas;
KeygenElement was removed in Chrome 57
IDBFactory.webkitGetDatabaseNames() constructor removed in Chrome
Depreciated FileError and DomError have been removed use DomException
error.code replaced with error.name  (e.g., see fileapi_directory_test.dart)
Additional because FileError is gone there is no easy way to determine which errors are File only errors.  However, these are:
NOT_FOUND_ERR
SECURITY_ERR
ABORT_ERR
NO_MODIFICATION_ALLOWED_ERR
INVALID_STATE_ERR
SYNTAX_ERR
INVALID_MODIFICATION_ERR
QUOTA_EXCEEDED_ERR
TYPE_MISMATCH_ERR
registerElement and register maps to old style (Chrome 50)
registerElement2 2nd parameter is a map {'prototype': xxx, 'extends': xxxx} not 2 separate arguments.
List<Rectangle> getClientRects() is now DomRectList getClientRects()
Rectangle getBoundingClientRect() is now DomRect getBoundingClientRect()
postMessage(/*any*/ message, String targetOrigin, [List<MessagePort> transfer]) changed to void postMessage(/*any*/ message, String targetOrigin, [List<Object> transfer]) transfer can now be ArrayBuffer, MessagePort and ImageBitmap
RTCPeerConnection.setLocalDescription and setRemoteDescription
takes a Map setLocalDescription(Map description) description is a map e.g.,  {'type': localSessionDescription, 'sdp': fakeLensSdp}
nounce removed from HtmlScriptElement added to HtmlElement
https://codereview.chromium.org/2801243002 https://github.com/whatwg/html/issues/2369
Event.deepPath() was removed Chrome 54
https://developer.mozilla.org/en-US/docs/Web/API/Event/deepPath
Event.scoped replaced by Event.composed
https://developer.mozilla.org/en-US/docs/Web/API/Event/composed

***
Summary of API changes from Chrome 51 thru Chrome 63:
<br><br>
[https://developers.google.com/web/updates/tags/chrome51](https://developers.google.com/web/updates/tags/chrome51)
<br>
[https://developers.google.com/web/updates/tags/chrome52](https://developers.google.com/web/updates/tags/chrome52)
<br>
[https://developers.google.com/web/updates/tags/chrome53](https://developers.google.com/web/updates/tags/chrome53)
<br>
[https://developers.google.com/web/updates/tags/chrome54](https://developers.google.com/web/updates/tags/chrome54)
<br>
[https://developers.google.com/web/updates/tags/chrome55](https://developers.google.com/web/updates/tags/chrome55)
<br>
[https://developers.google.com/web/updates/tags/chrome56](https://developers.google.com/web/updates/tags/chrome56)
<br>
[https://developers.google.com/web/updates/tags/chrome57](https://developers.google.com/web/updates/tags/chrome57)
<br>
[https://developers.google.com/web/updates/tags/chrome58](https://developers.google.com/web/updates/tags/chrome58)
<br>
[https://developers.google.com/web/updates/tags/chrome59](https://developers.google.com/web/updates/tags/chrome59)
<br>
[https://developers.google.com/web/updates/tags/chrome60](https://developers.google.com/web/updates/tags/chrome60)
<br>
[https://developers.google.com/web/updates/tags/chrome61](https://developers.google.com/web/updates/tags/chrome61)
<br>
[https://developers.google.com/web/updates/tags/chrome62](https://developers.google.com/web/updates/tags/chrome62)
<br>
[https://developers.google.com/web/updates/tags/chrome63](https://developers.google.com/web/updates/tags/chrome63)
<br>