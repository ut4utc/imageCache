# imageCache

This JavaScript library is intended to make image caching as simple as possible.  The syntax is:

var s=["images/1.jpg", "images/2.jpg", "images/3.jpg"]; // or any array of image URLs
imageCache.pushArray(s, loadImageEvent, loadAllEvent);

loadImageEvent is a callback that fires everytime an image is loaded.
loadAllEvents is a callback that fires when all images are loaded.

More info can be found at http://www.useragentman.com/blog/?p=4329