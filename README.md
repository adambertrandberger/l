<div align="center">
<img src="img/logo.png" width="100px"></img>
<br>
<a href="#"><img alt="no dependencies" src="https://img.shields.io/badge/dependencies-none-blue.svg"></img></a>
<a href="https://travis-ci.org/adambertrandberger/l"><img alt="build status" src="https://travis-ci.org/adambertrandberger/l.svg?branch=master"></img></a>
<h1>for Javascript</h1>
</div>

Whether creating DOM nodes for your client-side app, or generating HTML for a Node.js webserver, <img src="img/l.png" alt="l" height="14px"></img> makes it easy.
For your client-side apps, you will be free from the chains that are the DOM API. No longer will you have verbose calls like `document.createElement`
binding you to your keyboard. With your new found freedom you can focus on describing HTML in a natural way. For your webservers, you can use the same 
liberating abstractions offered by <img src="img/l.png" alt="l" height="14px"></img> to generate HTML.

<img src="img/l.png" alt="l" height="14px"></img>:
```javascript
l(() => div(
h1('Example'),
div(p('First'), hr, p('Second'), span(span(span('Nesting is easy.')))),
section(header(h1('HTML 5 is supported too!')),
div('Properties can be set', { style: { color: 'red' }}),
div('Attributes can be set too!', { class: 'wow' }))
)
);
```

HTML:
```html
<div>
<h1>Example</h1>
<div>
<p>First</p>
<hr>
<p>Second</p>
<span><span><span>Nesting is easy.</span></span></span>
</div>
<section>
<header><h1>HTML 5 is supported too!</h1></header>
<div style="color: red;">Properties can be set</div>
<div class="wow">Attributes can be set too!</div>
</section>
</div>
```

## Installation
### For Browsers
Just download `dist/l.js` and place it next to the HTML you're importing it from. Don't forget to add a script tag: 

```html
<script src="l.js"></script>
```

### For Node.js
<img src="img/l.png" alt="l" height="14px"></img> works in Node.js too! Install with `npm install l-html`, then import using `require`:

```javascript
const l = require('l-html')
```

## Test
<img src="img/l.png" alt="l" height="14px"></img> has `mocha` tests that verify that the code works as expected. If you want to run the tests, run:

```
git clone https://github.com/adambertrandberger/l
npm install
npm test
```

This is an optional step. You should only do it if you want to verify everything works for yourself. But, reading some of these tests is a good supplement to the tutorial below. 

## Getting Started
After importing <img src="img/l.png" alt="l" height="14px"></img> in your environment, there will be one new variable called `l` that you can use. 
Everything we go over in the following sections describes what you can do with this object.

## Generating Elements
There are multiple ways to generate HTML through the `l` object. 
The most straightforward way is to create a tag by calling the tag name using the dot operator on the `l` object.

```javascript
l.div()
// <div></div>

l.section()
// <section></section>
```

The `l` object has a function like this for every valid HTML tag.

You may be saying "what about custom HTML elements?". You're right, the HTML spec allows for custom tag names and so does <img src="img/l.png" alt="l" height="14px"></img>. 
If you want to generate a custom HTML tag, you can make it by calling `l` as a function:

```javascript
l('customtag')
// <customtag></customtag>

l('burrito')
// <burrito></burrito>
```

In a later section we will talk about adding children to these HTML elements. You'll see examples where we can generate elements by using the value of a tag generating function like `l.div` or `l.span`:

```javascript
l.div(l.br, l.hr, l.span)
// <div><br><hr><span></span></div>
```

See how we used `l.br`, `l.hr`, and `l.span` without calling them like `l.br()`, `l.hr()`, and `l.span()`? This works for all HTML elements when they are children of another element.

## Adding Content
Now we can make tags, but how can we add content to them? Starting from the methods above, we can
add one more argument to each of these function calls. You can pass it a string to set the innerHTML:

```javascript
l.div('This is the innerHTML')
// <div>This is the innerHTML</div>

l('burrito', "I'm in a burrito")
// <burrito>I'm in a burrito</burrito>
```

You can keep adding in more strings as additional arguments too:

```javascript
l.div('First', 'Second', 'Third')
// <div>FirstSecondThird</div>

l('burrito', "I'm", "in", "a", "burrito")
// <burrito>I'minaburrito</burrito>
```

This will be more useful later when we learn about nesting HTML elements inside of each other.

## Attributes and Properties
You can pass an object to configure attributes and properties of the HTML element:

```javascript
l.div({ style: { color: 'pink'}})
// <div style="color: pink;"></div>

l('burrito', { id: 'special' })
// <burrito id="special"></burrito>

l.span({ innerHTML: 'You can set the html like this too' })
l.span({ html: 'You can set the html like this too' })
// <span>You can set the html like this too</span>
```

In the object that we passed, <img src="img/l.png" alt="l" height="14px"></img>  will try to guess if the value should be set as an attribute, or a property on the node.
If you want to force a value to be a property or attribute explicitly you can use the reserved key `attrs` for attributes
and `props` for properties:

```javascript
l.textarea({ value: 'My value' })
// <textarea value="My value"></textarea>

l.textarea({ props: { value: 'My value' }})
// <textarea></textarea>
```

Notice how <img src="img/l.png" alt="l" height="14px"></img>  inferred that `value` was an attribute, but when told to use it as a property it will be assigned to the node 
as a property. This results in the HTML not containing the potentially long `value` attribute, but it is still shown in the HTML. 


You can mix these two arguments (the string and object) to make the syntax more compact:
```javascript
l.div('This is the text', { style: { color: 'red' }})
// <div style="color: red;">This is the text</div>
```

## Children
There are two ways of adding children to a node. You can pass a list as one of the arguments:

```javascript
l.div([l.span, l.span])
// <div><span></span><span></span></div>
```

Or you can add any number of additional arguments as nodes:

```javascript
l.div(l.span)
// <div><span></span></div>

l.div(l.span, l.span)
// <div><span></span><span></span></div>
```

Children don't have to be nodes either. You can pass strings, or numbers and they will be converted to
text nodes:

```javascript
l.div(l.span('a', 'b', 'c'), 'd')
// <div><span>abc</span>d</div>

l.span(1, 2, 3, l.div(4, 'five', l.div(l.div('six'))))
// <span>123<div>4five<div><div>six</div></div></div></span>
```

### <img src="img/l.png" alt="l" height="16px"></img>  Function
You know how we have been saying `l.div()` and `l.span()`? With the `l` _dot_? That isn't what the example code looks like is it? In the example code we didn't 
have to type `l` _dot_. To get the pretty code you see in the examples, you have to use <img src="img/l.png" alt="l" height="14px"></img>  functions. An <img src="img/l.png" alt="l" height="14px"></img>  function is a shortcut so that you 
can skip typing `l.` all the time. Everything works the same as before. All the arguments are the same, its just easier to type.
The main advantage is that you can feel like you're typing actual HTML instead of using a library.

```javascript
l(() => div(span('Feels more like writing HTML'), br, span('Pretty cool')))

l.div(() => span('You use it anywhere'), () => l.br, () => l.span('Yeah'))
```

There is a performance overhead that comes with using <img src="img/l.png" alt="l" height="14px"></img> functions. If you are using this to create a 
mission-critical page that would benefit from being a couple of milliseconds faster, you can skip using <img src="img/l.png" alt="l" height="14px"></img> functions, or even look into using `l.import`. But, everyone else will be able enjoy the
cleaner interface.

A nice trait of <img src="img/l.png" alt="l" height="14px"></img> functions is that they don't clobber variables in the outer or global scope:

```javascript
var a = "Can't touch this";
l(() => a('What are you talking about', { href: '#' }))
// <a href="#">What are you talking about</a>
console.log(a === "Can't touch this");
```

See how inside of the <img src="img/l.png" alt="l" height="14px"></img>  function, `a` changed its value from being `"Can't touch this"` to being a function that creates an anchor tag? Even better,
the `a` defined in the outer scope is untouched. You can think of any tag name as a reserved keyword inside of an <img src="img/l.png" alt="l" height="14px"></img>  function.
If you need to close a variable over an <img src="img/l.png" alt="l" height="14px"></img>  function, you should make sure it isn't the name of an HTML tag, as it will not
be visible inside of the <img src="img/l.png" alt="l" height="14px"></img>  function.

#### <img src="img/l.png" alt="l" height="14px"></img> Functions and Variable Scopes
You might be wondering exactly which variables <img src="img/l.png" alt="l" height="14px"></img> functions are capable of accessing. Due to implementation problems, <img src="img/l.png" alt="l" height="14px"></img> functions 
aren't able to keep the same properties that exist in Javascript functions. They do not capture local variables as Javascript's closures do. This means
the following code will _not_ work:

```
// Does NOT work:
function myFunc() {
const thing = 12;
console.log(() => div(thing));
}

myFunc(); // throws "ReferenceError: thing is not defined"
```

To look on the bright-side, this is good because it means that no local variables (variables defined in functions) will get clobbered with names of the element generators in the 
<img src="img/l.png" alt="l" height="14px"></img> functions. But it would be nice to access local variables from within an <img src="img/l.png" alt="l" height="14px"></img> function.
In the next section we will take a look at `l.with`, which is a way to pass arguments to <img src="img/l.png" alt="l" height="14px"></img> functions.

While they can't access local variables, they _can_ access all global variables:

```
// DOES work
const thing = 12;
function myFunc() {
console.log(() => div(thing));
}

myFunc(); // <div>12</div>
```

But, if we want <img src="img/l.png" alt="l" height="14px"></img> functions to be able to render different HTML based off different objects, we need a way of passing
information to the <img src="img/l.png" alt="l" height="14px"></img> functions. In the next section we will show the solution to this: `l.with`.

## Passing Arguments to <img src="img/l.png" alt="l" height="14px"></img> Functions
Because <img src="img/l.png" alt="l" height="14px"></img> aren't closures, we need some way of passing variables into <img src="img/l.png" alt="l" height="14px"></img> function if we ever want to render HTML based on some input data.

`l.with` lets this happen. `l.with` is just like calling `l` as a function (`l(...)`), except it takes a required object as its first argument.

All of the fields in that object will be available inside of the <img src="img/l.png" alt="l" height="14px"></img> function. Take a look:

```
const person = { age: 23, name: 'Dave' },
    animal = { type: 'birdy', talk: () => 'tweet' };
l.with({ person, animal }, () => div('You are: ', person.age, ' years old.', span(animal.talk()))); // <div>You are: 23 years old<span>tweet</span></div>
```

Notice how you don't need to give the  <img src="img/l.png" alt="l" height="14px"></img> function any additional parameters. The object you give `l.with` automagically includes them in the
scope. If you only needed the "person" and didn't want to have to type "person" all the time you can just pass in the "person" directly too:

```
const person = { age: 23, name: 'Dave' };
l.with(person, () => div('You are: ', age, ' years old.')); // <div>You are: 23 years old</div>
```

## Appending to Existing Nodes
<img src="img/l.png" alt="l" height="14px"></img>  supports a way of appending children to existing DOM nodes. When you use `l` as a function, as we have done <a href="#generating-elements"><i>Generating Elements</i></a>,
you can pass `l` an existing DOM node, and it will append any nodes that are given after it to that node.

```javascript
l(document.body, () => div("I've been appended!"))
// <body> ... <div>I've been appended!</div></body>
```

This works as you are building elements too:

```javascript
l(l.article, () => section(h1('First Part'), p), () => section(h1('Second Part'), p))
```

## Creating HTML strings
No matter if you are in a browser or Node.js, everything we have learned above will work wherever you use it. However, the output has always been DOM nodes. Having DOM nodes in Node.js isn't always useful. Most of the time
a string would be much better.

One way of getting a string from any one of these nodes is to access the `outerHTML` property:

```javascript
l(() => div(span, br)).outerHTML === '<div><span></span><br></div>'
```

There is something rather nasty about this... Oh yeah! It is the DOM API style name. If that is a turn-off, you can use the `l.str` function instead:

```javascript
l.str(() => div(span, br)) === '<div><span></span><br></div>'
```

`l.str` is the same as `l(...)`, but it outputs a string instead of an HTML element.

## Put Them All Together
Try mixing-and-matching adding content, with attributes and properties along with adding children and see what happens.
Put things where you think they ought to be, and see if it works. Its easy to check the output by looking at `outerHTML`.

```javascript
l.div(1, () => div(span, br, () => div, article, 'oiwjef'), 'three', l.hr, l.br, 'four')
// <div>1<div><span></span><br><div></div><article></article>oiwjef</div>three<hr><br>four</div>
```

That's a complicated example, but it shows how all the arguments we learned about can be nested together
and used interchangably. <img src="img/l.png" alt="l" height="14px"></img>  makes sense of whatever mess you give it. So try to give it your own mess
and see what it gives you back.

## What's Next?
Try mixing in Javascript with <img src="img/l.png" alt="l" height="14px"></img> to render your objects for you:

```javascript
class Profile {
constructor(name, age, phoneNumber) {
this.name = name;
this.age = age;
this.phoneNumber = phoneNumber;
}

editAge(e) {
alert('Call me at ' + this.phoneNumber);
}

render(parent) {
return l(parent, l.with(this, () => div(
h1(`Welcome, ${name}!`),
ul(li(`Age: ${age}`), 
li(`Phone Number: ${phoneNumber}`, { onclick: editAge })
),
)));
}
}
```

This way you can generate an HTML page for any person's profile. No Virtual DOM needed!

Make a game in Javascript, make a webserver, make visualizations, do anything you could do in normal Javascript and HTML, but enjoy it more.

## A Note on the Performance of <img src="img/l.png" alt="l" height="18px"></img> Functions
Everytime you use an <img src="img/l.png" alt="l" height="14px"></img> function, Javascript's `eval(...)` function is called. The function you supply
is actually converted to a string, then, among other things, a bunch of `var` declarations for each tag name are prepended to that string and fed into an `eval(...)`.

Popular opinion is to think that this is massively inefficient. From my tests, it is a bit inefficient, but still very usable even in production environments. Using `eval` allows the library to be even more flexible 
becuase it can use intermediate objects for HTML elements. Down the road additional helper functions and optimizations could be written that takes advantage of this. For example, when interfacing with the DOM, there are optimizations 
for calls which are made in succession intermediate objects allow us to be lazy about DOM node generation and allow us to do some optimizations around that. Element generator functions couldn't do this because
there is no way to say "whenever a tag generator completes, and the stack is empty, convert the result to a DOM node". Well... there is a way, but it includes using `Function.prototype.caller` which is widely
understood to be on the docket for removal from most Javascript implementations.

An optimization like this can still be done manually; however,
requiring a manual step from the programmers though. It would look like this: `l.dom(l.div, l.span, l.div(l.div()));`. But then, the whole library would have to be used this way.
but I'm not willing to make users have to worry about when the intermedate HTML element objects are turned into DOM nodes. At that point, I turn to my opinion that the programmers time
is not worth that small of an efficiency gain. This is why the option to use an `eval` based approach may be a good thing rather than a bad thing. There is no doubt about the weakened performance
at the moment though. I did a test on my macbook, and I was able to create ~500 DOM nodes (a `div` with 500 `span` children) in 0.025 milliseconds using element generators (`l.div()` and `l.span()`).
I tried the same experiment using <img src="img/l.png" alt="l" height="14px"></img> functions and was able to make 500 DOM nodes in 3.88 milliseconds. These tests were done in Google Chrome, not on Node.js.
I assume the running times will be different on Node.js based on how a fake DOM is used in that environment. 

Keep in mind this is a lot of DOM nodes. 500 nodes is enough for making most web pages. When this number becomes unusable is when you are using the library for a game that requires 60FPS updates and has > 10,000 DOM nodes (since every frame must take less than 16.66ms).
Perhaps a large n-body particle simulation would greatly benefit from skipping <img src="img/l.png" alt="l" height="14px"></img> functions all together and just going with element generators.

The conclusion I take away from these results is to use <img src="img/l.png" alt="l" height="14px"></img> functions whenever performance doesn't matter. Rendering a webpage, and even frequent re-rendering of a 
webpage is a good use for <img src="img/l.png" alt="l" height="14px"></img> functions because it will save programmer time. For tasks that are extremely time sensitive (such as rendering animation frames) or for
extremely large webpages (>10,000 DOM nodes), use element generators, or `l.import`.

## But I want high performance, and <img src="img/l.png" alt="l" height="14px"></img> functions!
We can do that. But you are going to have to sacrifice for a little good ol' fashioned namespace pollution:

```
l.import();

div(span(span()), 'it works!')
```

There is `l.import` for this need. The function takes an object to import the element generators into (defaults to `window`), and a boolean which indicates whether to clobber existing variables or not (defaults to `false`, as in "don't clobber").

This isn't a horrible choice if you really like the <img src="img/l.png" alt="l" height="14px"></img> function syntax, but want the performance. Although, it has potential to cause some nasty variable overriding issues. I figured I would keep it in the library for those who don't care about namespace pollution :).

## On the use of `eval` instead of `with`

A question I forsee from a small number of people is "why not use the `with` statement instead of building up your own variable declarations in the string that is eval'd?". Well, the Javascript community has effectively killed
the `with` statement. Javascript running in `strict mode` doesn't pass the parsing step if it finds a `with` statement. Many tools are hard coded to only use `strict mode` for
their build processes. This means if I wanted to use the latest transpilers and bundlers, like babel and webpack, I couldn't use the `with` statement.

I'm sure before the first question is asked most people would say "why were you even considering using the `with` statement?!". The dynamic nature of <img src="img/l.png" alt="l" height="14px"></img> functions required me to do some namespace hacking.

## Related Libraries
The idea of generating HTML in programming languages is old. It has been (almost famously) re-invented in lisps many times. [spinneret](https://github.com/ruricolist/spinneret), for example, is a library for generating HTML5. 
Programming languages like [prolog](http://www.swi-prolog.org/pldoc/man?section=htmlwrite), python, and c have done it too. However, these programming languages don't run directly in the browser like Javascript does. 
Those libraries are created for web servers or static page generation. No offense to server-side programming, especially because <img src="img/l.png" alt="l" height="14px"></img> supports that. But, having access to the DOM allows 
nodes to be created dynamically and for making interactive HTML. Seeing as Javascript can run in the browser like this, and be used for a webserver it is a natural fit for such a library.

The browser already has the capability of generating HTML through a builtin interface: the DOM. 
I've used the DOM enough to know how time consuming it is to use. Take a look at how messy it is to generate `<div><span>This is tedious!</span></div>` and append it to `document.body`:

```javascript
const node = document.createElement('span')
node.innerHTML = 'This is tedious!'
const container = document.createElement('div')
container.appendChild(node)
document.body.appendChild(container)
```

Others have noticed how messy this is too and have created libraries to make using it faster for the programmer. Here are some of the libraries that came before <img src="img/l.png" alt="l" height="14px"></img>  and inspired my work:
- [jaml](http://edspencer.github.io/jaml/) - Has the same interface as <img src="img/l.png" alt="l" height="14px"></img> functions, but only generates HTML strings. It seems like it was inspired from MVC style HTML templates like that of Ruby on Rails or Django.
- [http/html_write](http://www.swi-prolog.org/pldoc/man?section=htmlwrite) - A prolog library for generating HTML. Identical interface as JAML and <img src="img/l.png" alt="l" height="14px"></img> functions.
- [crel](https://github.com/KoryNunn/crel) - More lightweight than <img src="img/l.png" alt="l" height="14px"></img> with some interface differences (the constructor handles attributes/properties differently). Doesn't have an <img src="img/l.png" alt="l" height="14px"></img> function equivalent.
- [laconic](https://github.com/joestelmach/laconic) - Similar to crel (crel says this was its inspiration).
- [RE:DOM](https://redom.js.org/) - Has much in common with <img src="img/l.png" alt="l" height="14px"></img>, but doesn't have a <img src="img/l.png" alt="l" height="14px"></img> function equivalent, and focuses more on providing features for web components and updating the DOM.

## License

Copyright 2019 Adam Bertrand Berger

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## TODO:
- Add a way to print a pretty html string
- Rename Element to Node and Node to NodeProperties (and make a new Element class and TextNode class)
- Make website
- Add try-out-online page
- Create example apps
- why does doing `l.div` vs `l.div()` have such a large performance impact
- figure out why Proxy isn't being used in the browser for the `l` object
