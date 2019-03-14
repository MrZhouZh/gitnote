## 10 Javascript Performance Boosting Tips from Nicholas Zakas

1. Define local variables(定义变量)
   
   When a variable is referenced, Javascript hunts it down by looping through the different members of the [scope chain](). This scope chain is the set of variables available within the current scope, and across all major browsers it has at least two items: a set of local variables and a set of global variables.

   Simply put, the deeper the engine has to dig into this scope chain, the longer the operation will take. It first goes through the local variables starting with this, the function arguments, then any locally defined variables, and afterwards iterates through any global variables.

   Since local variables are first in this chain, they’re always faster than globals. So anytime you use a global variable more than once you should redefine it locally, for instance change:
   ```js
   var blah = document.getElementById('myID'),
   blah2 = document.getElementById('myID2');
   ```
   to
   ```js
   var doc = document,
   blah = doc.getElementById('myID'),
   blah2 = doc.getElementById('myID2');
   ```

2. Don't use the `with()` statement(不要使用 `with()`)
   
   It’s pretty much a fact: the with() statement is [pure Javascript evil](http://www.yuiblog.com/blog/2006/04/11/with-statement-considered-harmful/).

   This is because with() appends an extra set of variables to the beginning of the scope chain described above. This extra item means that anytime any variable is called, the Javascript engine must loop through the with() variables, then the local variables, then the global variables.

   So with() essentially gives local variables all the performance drawbacks of global ones, and in turn derails Javascript optimization.
   
3. Use closures sparingly(谨慎使用闭包)
   
   [Closures](http://www.jibbering.com/faq/faq_notes/closures.html) are a very powerful and useful aspect of Javascript, but they come with their own performance drawbacks.

   Although you may not know the term ‘closures’, you probably use them often. Closures can basically be thought of as the new in Javascript, and we use one whenever we define a function on the fly, for instance:

   The problem with closures is that by definition, they have a minimum of three objects in their scope chain: the closure variables the local variables, and the globals. This extra item leads to all the performance problems we’ve been avoiding in tips #1 & #2.

   But I don’t think Nicholas wants us to throw the baby out with the bathwater. Closures are still useful both for convenience and for making code more readable, just try not to overuse them (especially not in a loop).

4. Object properties and array items are slower than variables(对象属性和数组项比变量慢)
   
   When it comes to Javascript data, there’s pretty much four ways to access it: literal values, variables, object properties, and array items. When thinking about optimization, literal values and variables perform about the same, and are significantly faster than object properties and array items.

   So whenever you reference an object property or array item multiple times, you can get a performance boost by defining a variable. (This applies to both reading and writing data.)

   While this rule holds mostly true, Firefox has done some interesting things to [optimize array indexes](http://www.webreference.com/programming/javascript/ncz/column4/), and actually performs better with array items than with variables. But the performance drawbacks of array items in other browsers mean that you should still avoid array lookups, unless you are really only targeting Firefox performance.

5. Don't dig too deep into arrays(不要深层次挖掘数组)
   
   Additionally, you should avoid digging too deep into arrays. This is because the deeper you dig, the slower the operation.

   Simply put, digging deep into an array is slow because array item lookups are slow. Think about it: if you dig three levels into an array, that’s three array item lookups instead of one.

   So if you constantly reference foo.bar you can get a performance boost by defining var bar = foo.bar;

6. Avoid `for-in` loops(and function based iteration)(避免使用 `for-in`)
   
   Here’s another pretty black-and-white performance tip: don’t use for-in loops.

   The logic behind this is pretty straightforward: instead of looping through a set of indexes like you would with a for or a do-while, a for-in not only might loop through additional array items, but also requires more effort.

   In order to loop through these items, Javascript has to set up a function for each one. This function-based iteration comes with a slew of performance issues: an extra function is introduced with a corresponding execution context that is created and destroyed, on top of which an additional object is added to the scope chain.


7. Combine control conditions and control variable changes when using loops(使用循环时结合控制条件和控制变量的改变)
   
   Whenever talking about performance, work avoidance in loops is a hot topic because, quite simply, loops run over and over again. So if there are any performance gains to be had, you will most likely see the largest boosts within your loops.

   One way to take advantage of this is to combine your control condition and control variable when you define your loop. Here’s an example that doesn’t combine these controls:

   ```js
   for(var x = 0; x < 10; x++) {};
   ```
   Before we add anything at all to this loop, there are a couple operations that will occur every iteration. The Javascript engine must #1 test if x exists, #2 test if x < 0 and #3 add the increment x++.

   However if you're just iterating over some items in an array, you can cut out one of these operations by flipping this iteration around and using a while loop:
   ```js
   var x = 9;
   do { } while( x-- );
   ```
   If you want to take loop performance to the next level, Zakas also provides a more advanced [loop optimization technique](http://www.nczonline.net/blog/2009/01/13/speed-up-your-javascript-part-1/), which runs through the loop asynchronously (so cool!).

8. Define arrays for HTML collection objects(为HTML集合对象定义数组)
   
   Javascript uses a number of HTML collection objects such as document.forms. document.images, etc. Additionally these are called by methods such as getElementsByTagName and getElementsByClassName.

   As with any DOM selection, HTML collection objects are pretty slow, but also come with additional problems. As the DOM Level 1 spec says, "collections in the HTML DOM are assumed to be live, meaning that they are automatically updated when the underlying document is changed".

   This is horrible.

   While collection objects look like arrays, they are something quite different: the results of a specific query. Anytime this object is accessed for reading or writing, this query has to be rerun, which includes updating all the peripheral aspects of the object such as its length.

   HTML collection objects are extremely slow; Nicholas mentioned metrics in the ballpark of 60 times slower for a small operation. Additionally, these collection objects can lead to infinite loops where you might not expect. For instance:
   ```js
   var divs = document.getElementsByTagName('div');

   for (var i=0l i < divs.length; i++ ) {
       var div = document.createElement("div"); 
       document.appendChild(div);
   }
   ```
   This causes an infinite loop because divs represents a live HTML collection, rather than an array like you might expect. This live collection is updated every time we append a new <div> to the DOM, so i < divs.length never terminates.

   The way around this is to define these items in an array, which is a little more complex than just setting var divs = document.getElementsByTagName('div');. Here's a script Zakas uses to force an array:
   ```js
   function array(items) {
       try {
           return Array.prototype.concat.call(items);
       } catch (ex) {
        
            var i = 0,
                len = items.length,
                result  = Array(len);
        
            while (i < len) {
                result[i] = items[i];
                i++;
            }
        
            return result;
       }
   }

   var divs = array( document.getElementsByTagName('div') );

   for (var i=0l i < divs.length; i++ ) {
       var div = document.createElement("div"); 
       document.appendChild(div);
   }
   ```

9. Stop touching the DOM(禁止操作 `DOM`)
   

10. 
   
   Leaving the DOM alone is another big topic in Javascript optimization. The classic example is appending an array of list items: if you append each of these to the DOM individually, it is considerably slower than appending them all at once. This is because DOM operations are expensive.

   Zakas went into great detail on this, explaining that DOM operations are resource-heavy because of [reflow](). Reflow is basically the process by which the browser re-renders the DOM elements on the screen. For instance, if you change the width of a div with Javascript, the browser has to refresh the rendered page to account for this change.

   Any time an element is added or removed from the DOM, reflow will occur. The solution for this is a very handy Javascript object, [documentFragment](http://ejohn.org/blog/dom-documentfragments/), which I felt better about not having used after [Steve Souders](http://stevesouders.com/) admitted the same.

   DocumentFragment is basically a document-like fragment that isn't visually represented by the browser. Having no visual representation provides a number of advantages; mainly you can append nodes to a documentFragment without incurring any browser reflow.

10. Change CSS classes not styles(更改CSS类而不是样式)
   You may have heard that changing CSS classes is more optimal than changing styles. This boils down to another reflow issue: whenever a layout style is changed, reflow occurs.

   Layout styles mean anything that might affect the layout and force the browser to reflow. For instance, width, height, font-size, float, etc.

   Now don't get me wrong, CSS classes don't avoid reflow, they simply minimize it. Instead of incurring a reflow penalty every time you change a given style, you can use a CSS class to change a number of styles at once, and in turn only incur a single reflow.

  So it makes performance sense to use CSS classnames whenever changing more than one layout style. Additionally, it's also optimal to [append a style node to the DOM](http://jonraasch.com/blog/javascript-style-node) if you need to define a number of classes on the fly.
