# imageCache

Caching Images With JavaScript and HTML5 progress Bars

In my last article, Cross Browser HTML5 Progress Bars In Depth, I discussed how to create progress bars and how to do fancy-pants CSS styling on them in all browsers. Sure, it’s cool eye candy, but where would you use one? I’m sure you have seen Flash sites that have a progress bars before (or what some of my designer friends call a “loader”), but just in case, take a look at the Star Trek Movie site to see an example of one in the wild. This loader tells the user how long it will take for the page to load and display. If this page takes a long time to load, a loader will prevent users from thinking the page is not loading properly and go someplace else. In most cases, these loaders show the percentage of assets have been loaded, and when it reaches 100%, the content is displayed. This prevents images from appearing half loaded when the content is first shown. While I wouldn’t want loaders for most HTML-based web sites, there are definitely times you would to want to do this, like when creating a full-screen slideshow. This article will show you how to make one easily.


A Really Cool Photo Gallery Example of Image Caching With HTML5 <progress>
Caching Images.

Before we do anything, we need to know how to cache images. Developers have been using the DOM Level 0 Image() object to cache images since almost the beginning of JavaScript’s history. Basically it involves creating a new Image() object, and then setting the src to be URL of the image.

    var im = new Image();
    im.src = "http://www.useragentman.com/images/zoltFront.jpg";

Back in the 1990?s, this technique was used a lot to cache images involved in animation of hover states. A good example of this is on the old Netcom Canada website circa 1997 (available today thanks to the magic of The Way Back Machine). If you mouseover any of the navigation links in the header, you’ll see the image button change in appearance. In order for this animation to happen as soon as the user hovers over the links, the page must cache all the hover-state images, so that the user doesn’t see any flicker on-hover as the image is loading. Modern developers would use CSS sprites to do this, but back in the day, this was the only way to go.

But how can one use this technique to update a progress bar? The Image() object has a load event, so we can use that to increment a progress bar. To simplify this process, I have created a very simple JavaScript library, imageCache.js, that will cache an array of image URLs and fire off two events: one when each image is loaded, and one where the whole array is loaded. Image caching is as simple as this:

    var s=["images/1.jpg", "images/2.jpg", "images/3.jpg"];
    imageCache.pushArray(s, loadImageEvent, loadAllEvent);

loadImageEvent is a callback that is called every time an image is loaded, and loadAllEvent is called when all of the images are loaded.

Download imageCache.js from GitHub.
Using <progress> to Show When Each Image Has Loaded

In the image gallery example at the beginning of this article, we have 28 images that we want to load into cache before showing the content of the page. We also want a progress bar to show these images loading. In order to do this, we need to make two containers: one that holds the progress bar and one that holds the content:

    <-- The Progress Bar Container -->
    <div id="progressContainer">
        Loading: <br />
        <progress id="loader" max="28" value="0">
           <strong>Loaded <span id="value">0</span>/28</strong>
        </progress>
    </div>

    <-- The Content Container -->
    <div id="content">
       <!-- this should contain the content for the rest of the site -->  
    </div>

You would then create a JavaScript snippit that would pre-cache all the images. What follows is an example that uses jQuery to do this, but since the imageCache.js library itself doesn’t need jQuery, feel free to use any framework you like (or lack of framework if you are “Teh Hardcorez“):

    var dnfmomd = new function () {
       var me = this;
       
       var $loader,
           currentImage = 0;
       
       
       me.init = function () {
          
          var s = [];
          
          for (var i=1; i<=28; i++) {
             s.push('images/dnfmomd/' + i + '.jpg');
          }
          $loader = $('#loader');
          
          $loader.max = s.length;
          imageCache.pushArray(s, loadImageEvent, loadAllEvent);
          
       }

       function loadImageEvent() {
          val = parseInt($loader.attr('value'));
          val++;
          $loader.attr('value', val);
       }
       
       function loadAllEvent() {
          $('body').addClass('loaded');
          showImage(1, true);
       }

       ....

    }

    /*
     *  Use $(window).load() instead of $(document).ready() because we wan't to start caching images
     *  as soon as the progress bar images are loaded.
     */

    $(window).load(dnfmomd.init)

Note that progress bar’s value is increased every time an image is loaded, and when all the images are loaded, we add the loaded class to the body. This will result in the progress bar disappearing and the content becoming visible, due to this CSS in the page:

      #content {
         display: none;
      }

      body.loaded #content {
         display: block;
      }

      body.loaded #progressContainer {
         display: none !important;
      }

Can We Show Other Things Loading in the Progress Bar Besides Images?

It is possible to show CSS and JavaScript in the progress bar using yepnope.js. It is a little bit more involved, but very doable (I have done it before), and I leave it as an exercise to the reader to do it. Post it below, and I’ll give you full geek-cred. If no one does it, I will probably take some time and publish an example in a few weeks, but I thought it would be neat to offer up a little challenge. :-)

This JavaScript library is intended to make image caching as simple as possible.  The syntax is:

var s=["images/1.jpg", "images/2.jpg", "images/3.jpg"]; // or any array of image URLs
imageCache.pushArray(s, loadImageEvent, loadAllEvent);

loadImageEvent is a callback that fires everytime an image is loaded.
loadAllEvents is a callback that fires when all images are loaded.

More info can be found at http://www.useragentman.com/blog/?p=4329