scripting-for-illustrator-tutorial
==================================

Scripting for Illustrator, a tutorial for Processing coders.

###0. Intro

I'm going to show you how to create a new document, work with existing documents, make shapes, text, and placed images. I will also show you how to manipulate preexisting objects in a layout. The tutorial assumes you are familiar with basic concepts of object oriented language and the basics of Processing. Naturally, it helps to know the Javascript language, and the basics of Adobe Illustrator. If you are new to the Javascript language (even if you know Java from Processing), here is a nice JS tutorial: http://www.codecademy.com/en/tracks/javascript

###1. Install

Beginning with Illustrator CC, the scripting is not bundled so please download and install it now. It can be found in the Adobe Creative Cloud system menu in the Apps tab. Otherwise, you can find the link here: https://creative.adobe.com/products/estk

You will see a new folder called /Applications/Adobe ExtendScript Toolkit CC ... so now you can run the app called "ExtendScript Toolkit". Please do so now.

You will see a cute little scripting app pop up, but it won't be aimed at Illustrator by default. Tell it to control Illustrator by selecting "Adobe Illustrator CC ####" (which ever version you prefer) from the top-left menu.

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/00.png)

If Illustrator is not yet running, you will get this popup:

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/01.png)

Choose "YES" and it will launch Illustrator for you.

The chain link to the left of the dropdown menu is now linked-up and colored green. If illustrator closes, you will see the link icon looking "broken".

###2. HelloWorld

It's deceptively simple. Type `"Hello World"` into the editor (INCLUDING THE QUOTES), then hit the green play button. Illustrator will flash for a moment, then return you back to ExtendScript.

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/02.png)

Notice the console shows `hello world`. This is because the entire script is treated as an expression, so what ever is evaluated by the end of the interpretation will show up in the console. In fact, you can use it as a basic calculator. Type `1+3` and the console will show `4`.

Now let's get more explicit. There is an actual function for printing to console, akin to `println()` in Processing. It's a static method of the `$` object. Like the JQuery javascript library, we have a frequently-used global object called `$` whose methods have to do with the ExtendScript system at large. For example, run this script:

```js
$.writeln("hi there!");
$.writeln("here is another line.");
```
and you will see:

```
hi there!
here is another line.
Result: undefined
```

At the end, you see Result: undefined since the last function call was to `$.writeln()` and that has a void return type, and nothing is sent back. 

At this time, you may notice the editor's font is a sans-serif. You will most likely prefer a fixed-width font like Monaco or Courier New. I include instructions in tips and tricks for that at the far end of this tutorial.

###3. Documentation

Let's explore the $ object in the documentation. Open up this PDF:

*/Applications/Adobe ExtendScript Toolkit CC/SDK/JavaScript Tools Guide CC.pdf*

. . . and jump to page 216. There, you will see a list of properties and functions of this object, including `writeln()`. Amongst the functions, there are some that catch my eye:

`$.colorPicker()` - asks the user to select a color from a standard color chooser, then returns an integer that, when represented in hexadecimal, is a standard HTML color triplet. Also works in Processing.

`$.sleep()` - tells the system to wait a little bit between operations so you can perhaps see each shape being drawn at a slower pace.

Okay, so let's stop printing test messages to the console and begin to control the Illustrator app.

###4. Install Illustrator API Documentation

I'm not sure why, but Adobe does not bundle the Illustrator API documentation with ExtendScript or with Illustrator. Here is where to get it: http://www.adobe.com/devnet/illustrator/scripting.html  ...  Download the PDFs called "Illustrator Scripting Reference - JavaScript.pdf" and "Illustrator Scripting Guide.pdf". The guide is an overview of the entire system and how it works, although the language may possibly be developer-centric for an ITPer. The reference is a list of all the objects, describing their properties and methods. A great place to start is page 14. Here is the drawing you see. Don't worry if it's overwhelming information. The API is vast and you don't need to memorize this stuff. Just know that it's there for reference as you work.

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/03.png)


