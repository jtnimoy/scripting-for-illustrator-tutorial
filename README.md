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

An object model describes which types of things contain other types of things. In this case, the Application object is the top-most, root of the tree. The diagram shows that the Application directly contains TextFont, Document, and those 4 objects on the far right. In the next part of this tutorial, I will show you how to work with the documents. Please think of this as akin to the Processing tutorial on the `background()` and `size()` functions. 

###5. Getting the open Document

Continuing our discussion of the object model diagram, the Document object, in turn, looks like it directly contains most every kind of thing you would want to create or access, including shapes and art boards. Awesome, so let's work our way down into the cave! Please open the reference PDF (the file you downloaded, called "Illustrator Scripting Reference - JavaScript.pdf") and turn to page 3, the table of contents. Notice it's just a list of object names. Click on "Application" and it will jump to page 8, where it describes the Application object. The description says the following:

"The Adobe® Illustrator® application object, referenced using the pre-defined global app object, which contains all other Illustrator objects."

That tells me that there is an app object already available to my script, for free, and it's a global instance of the Application object. Let's test that by printing it to the console.

```js
$.writeln(app);
```

You will see this printed to the console:
[Application Adobe Illustrator]

and that means that something exists there. If there was nothing, the execution would have stopped and colored the line red, telling you the symbol is undefined. But, because it didn't, we can traverse further down! The documentation for the Application object shows that there is a property called activeDocument. Let's print that too:

```js
$.writeln(app.activeDocument); 
```

You'll see the system gets confused unless you have a document open in illustrator. If the execution stopped and gave you a red line, try creating a new Illustrator document and leaving it open before running the script again, and it will not error that time.

###6. Create a New Document

If you wish to simply work with the document you already have open, your shortcut is app.activeDocument. However, if you wish to create a new document, here's how we go about it. In the reference, we see the Application has a property called documents, which is an instance of Documents . Click the blue link to Documents and it will jump to page 45, documentation on the collection of Document objects. This object is basically an array of documents. If you have nothing open, the length property will be 0. To create a new document using a pop-up dialog to ask the user what sort of document they need, you can go like this:

```js
app.documents.addDocument('',new DocumentPreset(),true);
```

Notice that this time, the console ended with `Result: [Document Untitled]` , meaning that this function returns a document instance. You could catch that in a variable and work with it from that point on:

```js
var doc = app.documents.addDocument('',new DocumentPreset(),true);
```

Let's say you want to automate the creation of documents, for example, pulling from a paper-size database, or using a generative pattern of paper-sizes. You would be calling `Documents.add()` like this:

```js
var doc = app.documents.add();
```

Very simple, but there are parameters you could be adding to control the size, number of artboards, and to some extent, the grid layout of those art boards.

```js
//create a new document of size 100x100 points
var doc = app.documents.add( null , 100,100);

//create a new document of default size, with 4 art boards.
var doc = app.documents.add(null,null,null,4); 

// makes a 10x10 grid of artboards, each sized 10x10, with margin spacing of 10.
var doc = app.documents.add(null,10,10,100,DocumentArtboardLayout.Column,10,10);
```

###7. Shape and Color

To draw a shape, you need to add a new path to the paths object. This is similar to the structure of app.documents but this time, it's Document.pathItems.

This script:

```
var doc = app.documents.add(null,100,100);
doc.pathItems.rectangle(0,0,50,20);
```

will give you this result:

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/04.png)


Notice that unlike Processing, whose `0,0` origin point is at the top-left, the Illustrator document's `0,0` origin point is at the bottom-left. To get it inside the document, we would need to use a negative Y value. We can account for this by drawing the shape using the height of the document for the Y value. Notice in this example that the rectangle's X and Y order are swapped:

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
doc.pathItems.rectangle(height,0,50,20);
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/05.png)


There are a few other handy shape functions you can call. Always check the documentation (reference, page 135) to see the order of parameters. Here is a demo that calls a few more of those shape functions:

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
var p = doc.pathItems;
p.rectangle(height,0,50,20);
p.ellipse(20,30,20,10);
p.polygon(10,50,20,6);
p.roundedRectangle(height,60,30,30,5,5);
p.star(70,30,10,20,8);
```
![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/06.png)

Now that we've gotten the shape primitives out of the way, I'd like to show you how to build a path from vertices. In Processing, this is akin to `beginShape()`, `vertex()`, and `endShape()`. In Illustrator, you tell the pathItems collection to create a new path, and that path will live in that collection. You take that shape as a variable and do things to it, including setting the vertices, closing/opening it (whether the last and first points connect), and the stroke/fill information.

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
var p = doc.pathItems;
var shape = p.add();
shape.setEntirePath([ 
    [78.5,26],
    [59.8,77.3],
    [18.3,77.3],
    [16.3,74.7],
    [17.5,71.5],
    [33.2,71.5],
    [39.5,77.5],
    [54.7,77.5],
    [31.7,35.8],
    [42,50.8],
    [48,51.8],
    [45,59.5],
    [49.2,68.8],
    [59,64],
]);
shape.closed = true;
shape.filled = false;
shape.strokeCap = StrokeCap.ROUNDENDCAP;
shape.strokeJoin = StrokeJoin.ROUNDENDJOIN;
shape.strokeWidth = 10;
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/07.png)

Unlike Processing, the stroke and fill colors are not global properties of the sketch. Instead, they are individual properties of the object. Here is an example of how to take a pathItem object, and color it.

```js
var doc = app.documents.add(null,100,100);
var p = doc.pathItems;

var star1 = p.star(40,50,50,60,3);
star1.blendingMode = BlendModes.LIGHTEN;
star1.fillColor = makeColor(255,0,0);
star1.strokeColor = makeColor(0,255,0);
star1.strokeWidth = 10;

var star2 = p.star(60,50,50,60,3);
star2.blendingMode = BlendModes.LIGHTEN;
star2.fillColor = makeColor(0,0,255);
star2.strokeColor = makeColor(255,0,255);

/*
    necessary since RGBColor class has no constructor.
*/
function makeColor(r,g,b){
    var c = new RGBColor();
    c.red   = r;
    c.green = g;
    c.blue  = b;
    return c;
}
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/08.png)

You can see I wrote a wrapper function to make the code look a bit more like Processing. I'm sure someone could sit down and write a wrapper class that sits on top of the Illustrator ExtendScript and supports a well-documented subset of the Processing API. However, doing so would not open you up to the things that are unique about Illustrator's system. For example, they have more color models than those which come bundled with Processing (not that you couldn't just implement those yourself). Here is an example of subtractive color... you know, the kind of color-mixing you learned from crayons and paint.

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
var p = doc.pathItems;

for(var y=0;y<height;y+=10){
    for(var x=0;x<width;x+=width/3){    
        var s = p.rectangle(width-y,x,width/3,10);
        if(x==0){
            s.fillColor = makeColor(y,100-y,100,0);
        }else if(x==width/3){
            s.fillColor = makeColor(0,y,100-y,y);
        }else{
            s.fillColor = makeColor(100-y,y,0,0);
        }
        s.stroked = false;
    }
}

/*
    necessary since CMYKColor class has no constructor.
*/
function makeColor(c,m,y,k){
    var ink = new CMYKColor();
    ink.cyan   = c;
    ink.magenta = m;
    ink.yellow  = y;
    ink.black  = k;
    return ink;
}
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/09.png)

###8. Text

All the text objects live inside the text collection, `Document.textFrames`. We add new things to it in the same way we did with shapes and artboards.

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
var text1 = doc.textFrames.pointText( [20,height-50] );
text1.contents = "Handgloves";
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/10.png)

This is called a pointText, and it's akin to the Processing `text()` function. Here is how to change some of the styles for the entire text object. There are many more ways in which you can change the style of text, and at different scopes. More information in the reference on page 17:

```js
var width = 100;
var height = 100;
var doc = app.documents.add(null,width,height);
var text1 = doc.textFrames.pointText( [0,height-50] );
text1.contents = "Handgloves";
var fontStyle = text1.textRange.characterAttributes;
fontStyle.fillColor = makeColor(255,150,150);
fontStyle.textFont = app.textFonts.getByName("Impact");
fontStyle.strokeColor = makeColor(0,255,255);
fontStyle.strokeWeight = 0.3;
fontStyle.size = 20.7;

/*
    necessary since RGBColor class has no constructor.
*/
function makeColor(r,g,b){
    var c = new RGBColor();
    c.red   = r;
    c.green = g;
    c.blue  = b;
    return c;
}
```

![](http://cdn.jtn.im/2014-11-13-ai-scripting-tut/11.png)


