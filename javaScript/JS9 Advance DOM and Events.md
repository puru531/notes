# Advance DOM and Events

## How DOM works and organized internally

DOM : Interface between JavaScript and Browser.

- Allow us to interact with the Browser.
- We can write JavaScript to create, modify, delete HTML elements; change styles, classes, attributes; and listen to events.
- DOM tree is generated from the HTML document, which we can then interact with using JavaScript.
- DOM is very complex API that contains many methods and properties to interact with the DOM tree.
- DOM is a tree-like structure where each node represents an object.

### How DOM is organized internally

```
    -------------
    |EventTarget|
    -------------
       |
       |
       |
    -------  --------
    | Node|  |Window|
    -------  --------
       |
       |
       |
    ---------
    |Element|
    ---------
       |
       |
       |
    ------------
    |HTMLElement|
    ------------
       |
       |
       |
    ----------------
    |HTMLDivElement|
    ----------------
EVENTTARGET : A special Node type that can be targetted by events. It is the parent of Node and Window. It has methods like addEventListener(), removeEventListener(), dispatchEvent(), etc.
NODE :  textContent, parentNode, childNodes, etc. are properties and methods of Node
ELEMENT : innerHTML, classList, etc. + all Node properties and methods
HTMLElement : click(), focus(), etc. + all Element properties and methods
HTMLDivElement : all HTMLElement properties and methods + specific properties and methods of HTMLDivElement

WINDOW : It is the global object in the browser. It is the parent of all objects in the browser. It has methods like alert(), setTimeout(), setInterval(), etc. many unrelated to the DOM.
```

- Every single node of the DOM tree is of the type Node.
- Each node is represented by an object.
- The Node object has many properties and methods. (e.g. `parentNode, childNodes, firstChild, lastChild, nextSibling, previousSibling, textContent, cloneNode() etc.`)
- There are different types of nodes (child-types of Node):

  - **Element nodes:** Represent HTML elements. (e.g. `<p>, <div>, <h1>`, etc.) Ech element has many methods : `innerHTML, textContent,children, parentElement, classList, querySelector(), querySelectorAll(), getAttribute(), setAttribute(), removeAttribute(),  append(), remove(), insertAdjacentHTML(), closest(), matches(), scrollIntoView(), etc.`<br>

    Element type has internally HTMLElement child type. And that HTML has One different type HTMLElement for each HTML element. (e.g. HTMLDivElement, HTMLParagraphElement, HTMLAnchorElement, etc.), which has many methods and properties specific to that element. like `<a>` has `href, target, etc.` and `<image>` has `src, alt, etc.`

  - **Text nodes:** Contain text. (e.g. `<p>`**Hello World**`</p>`)
  - **Comment nodes:** Represent comments. (e.g. `<!-- some comment -->`)
  - **Document nodes:** Represent the entire document. special type of node that represents the entire document. contains methods like `querySelector(), querySelectorAll(), getElementById(), getElementsByClassName(), getElementsByTagName(), etc.`

    \*\*querySelector() is available both in document and element.

- **Inheritance:** All the child types will also get access to the properties and methods of the parent type. (e.g. HTMLElement has access to all the properties and methods of Element and Node.)

# Selecting, Creating, and Deleting Elements

## Selecting Elements

Selecting entire document of the page:

```js run
console.log(document.documentElement); // <html>...</html>
```

Selecting the head and body elements:

```js run
console.log(document.head); // <head>...</head>
console.log(document.body); // <body>...</body>
```

### Selecting elements by their tag name:

```js run
const allParagraphs = document.getElementsByTagName("p"); // HTMLCollection of all <p> elements in the document (eg. HTMLCollection(3) [p, p, p]). It is live collection. It means if we add or remove any element from the collection, it will automatically update the collection.
console.log(allParagraphs);
```

### Selecting elements by their ID:

```js run
const myId = document.getElementById("my-id"); // <p id="my-id">...</p> (element with id "my-id")
```

### Selecting elements by their class name:

```js run
const myClass = document.getElementsByClassName("my-class"); // HTMLCollection of all elements with class "my-class"
```

### querySelector and querySelectorAll

```js run
console.log(document.querySelector("p")); // <p>...</p> (first <p> element in the document)
console.log(document.querySelectorAll("p")); // NodeList of all <p> elements in the document (eg. NodeList(3) [p, p, p])

console.log(document.querySelector(".my-class")); // <p class="my-class">...</p> (first element with class "my-class")
console.log(document.querySelectorAll(".my-class")); // NodeList of all elements with class "my-class"

console.log(document.querySelector("#my-id")); // <p id="my-id">...</p> (element with id "my-id")
console.log(document.querySelectorAll("#my-id")); // NodeList of all elements with id "my-id" (eg. NodeList(1) [p#my-id.my-class])

console.log(document.querySelector("p.my-class")); // <p class="my-class">...</p> (first <p> element with class "my-class")
console.log(document.querySelectorAll("p.my-class")); // NodeList of all <p> elements with class "my-class"

console.log(document.querySelector("p#my-id")); // <p id="my-id">...</p> (element with id "my-id")
console.log(document.querySelectorAll("p#my-id")); // NodeList of all <p> elements with id "my-id"

console.log(document.querySelector("p.my-class#my-id")); // <p id="my-id" class="my-class">...</p> (element with id "my-id" and class "my-class")
console.log(document.querySelectorAll("p.my-class#my-id")); // NodeList of all <p> elements with id "my-id" and class "my-class"
```

## HTMLCollection vs NodeList:

- HTMLCollection is live collection. It means if we add or remove any element from the collection, it will automatically update the collection.
- NodeList is not live collection. It means if we add or remove any element from the collection, it will not automatically update the collection.
- NodeList is more powerful than HTMLCollection. NodeList has more methods and properties than HTMLCollection.
- NodeList is returned by querySelectorAll() and HTMLCollection is returned by getElementsByTagName(), getElementsByClassName(), etc.

## Creating and inserting Elements

```js run
// Create a new element
const message = document.createElement("div"); // <div></div>

// Add a class to the element
message.classList.add("message"); // <div class="message"></div>

// Add text to the element
// message.textContent = "Hello World"; // <div class="message">Hello World</div>

// Add HTML to the element
message.innerHTML = "<h1>Hello World</h1>"; // <div class="message"><h1>Hello World</h1></div>

// Insert the element into the DOM
document.body.prepend(message); // <body><div class="message"><h1>Hello World</h1></div>...</body> --> prepend() will insert the element as the first child of the parent element.
document.body.append(message); // <body>...<div class="message"><h1>Hello World</h1></div></body> ---> append() will insert the element as the last child of the parent element.

/*
NOTE: If we try to insert the same element again, it will move the element to the new position instead of creating a new element. This is because an element can only exist in one place in the DOM tree at a time. 
*/

// If we want same element to be inserted at multiple places, we need to clone the element.
document.body.append(message.cloneNode(true)); // <body>...<div class="message"><h1>Hello World</h1></div><div class="message"><h1>Hello World</h1></div></body>

const header = document.querySelector(".header");
header.before(message);
/*
<body> 
  <div class="message"><h1>Hello World</h1></div>
  <header>...</header>
</body> 

--> before() will insert the element before the given element as sibling.
*/

header.after(message);
/*
<body> 
  <header>...</header>
  <div class="message"><h1>Hello World</h1></div>
</body>

after() will insert the element after the given element as sibling.
*/
```

## Deleting Elements

```js run
/*
<body> 
  <header>...</header>
  <div class="message"><h1>Hello World</h1></div>
  <button class="btn-close-msg">Close Message</button>
</body>
*/

const message = document.querySelector(".message");
message.remove(); // <body> <header>...</header> <button class="btn-close-msg">Close Message</button> </body> --> remove() will remove the element from the DOM tree.

//Deleting message div on button click
const btnCloseMsg = document.querySelector(".btn-close-msg");
btnCloseMsg.addEventListener("click", function () {
  message.remove(); // new method to remove the element from the DOM tree.

  // message.parentElement.removeChild(message); // old method to remove the element from the DOM tree.
});
```

## insertAdjacentHTML

- insertAdjacentHTML is a method that allows you to insert HTML into a specific position in the DOM.
- It takes two arguments:
  - The position where you want to insert the HTML.
  - The HTML you want to insert.

```js run
const div = document.createElement("div");
div.innerHTML = "<h1>Hello World</h1>";
document.body.insertAdjacentHTML("afterbegin", div.outerHTML);
/*
body
  div
    h1
      Hello World
*/
```

element.outerHTML returns the HTML of the element including the element itself.

```js run
const div = document.createElement("div");
div.innerHTML = "<h1>Hello World</h1>";
console.log(div.outerHTML); // <div><h1>Hello World</h1></div>
```

Visual representation of the positions:

```html
<!------- beforebegin ------->
<p>
  <!------- afterbegin ------->
  some text
  <!------- beforeend ------->
</p>
<!------- afterend ------->
```

# Styles, Attributes, and Classes

## Styles

### Setting Inline Styles

```js run
/*
<body> 
  <header>...</header>
  <div class="message"><h1>Hello World</h1></div>
  <button class="btn-close-msg">Close Message</button>
</body>
*/
const message = document.querySelector(".message");

message.style.backgroundColor = "blue"; // <div class="message" style="background-color: blue;"><h1>Hello World</h1></div> --> style property is used to set the inline styles of the element.
message.style.width = "120px"; // <div class="message" style="background-color: blue; width: 120px;"><h1>Hello World</h1></div>
```

### Getting Inline Style Values

```js run
/*
<body>
  <header>...</header>
 <div class="message" style="background-color: blue; width: 120px;"><h1>Hello World</h1></div>
  <button class="btn-close-msg">Close Message</button>
</body>
*/
const message = document.querySelector(".message");
console.log(message.style.backgroundColor); // blue --> style property is used to get the inline styles of the element.
console.log(message.style.height); // "" --> If the style is not set inline, it will return an empty string.
```

### Getting Computed Styles

```js run
//html
/*
<body>
  <header>...</header>
 <div class="message" style="background-color: blue; width: 120px;"><h1>Hello World</h1></div>
  <button class="btn-close-msg">Close Message</button>
</body>
*/
//css
/*
.message {
  height: 150px;
}
*/
const message = document.querySelector(".message");
console.log(getComputedStyle(message).height); // 150px --> getComputedStyle() method is used to get the computed styles of the element.

// getComputedStyle() method returns a CSSStyleDeclaration object that contains all the computed styles of the element.

//increase the height of the message div by 40px
message.style.height = `${parseFloat(getComputedStyle(message).height) + 40}px`; // <div class="message" style="background-color: blue; width: 120px; height: 190px;"><h1>Hello World</h1></div>
```

## CSS Variables / Custom Properties

Set the value of the CSS variable.

```js run
//css file
/*
:root {
  --primary-color: blue;
}
.message {
  background-color: var(--primary-color);
}
*/

document.documentElement.style.setProperty("--primary-color", "orangered"); // Set the value of the CSS variable --primary-color to orangered in the root element (html element).
```

## Attributes

Attributes are the extra information that we can add to the HTML elements. (e.g. id, class, src, href, etc.).

### Getting Attributes

```js run
//<img src="logo.png" alt="Logo" class="nav__logo" designer="Puru">
const logo = document.querySelector(".nav__logo"); // <img src="logo.png" alt="Logo" class="nav__logo"> (element with class "nav__logo")
console.log(logo.src); // http://site.com/logo.png --> src attribute of the element.
console.log(logo.alt); // Logo --> alt attribute of the element.
console.log(logo.className); // nav__logo --> class attribute of the element.

// Non-standard attributes
console.log(logo.designer); // undefined --> custom attribute of the element. It is not a standard attribute, so it will return undefined.

// Get the value of the custom attribute
console.log(logo.getAttribute("designer")); // Puru --> getAttribute() method is used to get the value of the custom attribute.

console.log(logo.src); // http://site.com/logo.png --> gives the absolute URL of the image.
console.log(logo.getAttribute("src")); // logo.png --> gives the relative URL of the image.

//Same with href attribute
```

### Setting Attributes

```js run
//<img src="logo.png" alt="Logo" class="nav__logo" designer="Puru">
const logo = document.querySelector(".nav__logo"); // <img src="logo.png" alt="Logo" class="nav__logo"> (element with class "nav__logo")

logo.alt = "New Logo"; // <img src="logo.png" alt="New Logo" class="nav__logo"> --> alt attribute of the element is changed.

// Set the value of the custom attribute
logo.setAttribute("designer", "Purushottam"); // <img src="logo.png" alt="New Logo" class="nav__logo" designer="Purushottam"> --> setAttribute() method is used to set the value of the custom attribute.

logo.setAttribute("company", "CodePuru"); // <img src="logo.png" alt="New Logo" class="nav__logo" designer="Purushottam" company="CodePuru"> --> new custom attribute is added to the element.
```

### Data Attributes

```js run
// <div data-id="123"></div>
const div = document.querySelector("div");
console.log(div.dataset.id); // 123 --> dataset property is used to get the value of the data attribute.

// Set new value of the data attribute
div.dataset.id = 456; // <div data-id="456"></div>
```

```js run
// <div class="card">Some content</div>
const card = document.querySelector(".card");
// set data cardNumber attribute
card.dataset.cardNumber = 123456;
console.log(card); // <div class="card" data-card-number="123456">Some content</div> --> camelCase is converted to kebab-case.

// get data cardNumber attribute
console.log(card.dataset); // {cardNumber: "123456"}
console.log(card.dataset.cardNumber); // 123456
console.log(card.getAttribute("data-card-number")); // 123456

console.log(card); // <div class="card" data-card-number="123456">Some content</div>

// remove data cardNumber attribute
delete card.dataset.cardNumber;
console.log(card); // <div class="card">Some content</div>
```

## Classes

`element.classList` is used to add, remove, toggle, and check classes of an element.

`element.classList.add()`, `element.classList.remove()`, `element.classList.toggle()`, `element.classList.contains()` are the methods of classList.

```js run
// <div class="card">Some content</div>
const card = document.querySelector(".card");

//add()
card.classList.add("new-class"); // <div class="card new-class">Some content</div> --> add() method is used to add a class to the element.
card.classList.add("new-class", "another-class"); // <div class="card new-class another-class">Some content</div> --> add() method can take multiple classes as arguments.

//remove()
card.classList.remove("new-class"); // <div class="card another-class">Some content</div> --> remove() method is used to remove a class from the element.
card.classList.remove("new-class", "another-class"); // <div class="card">Some content</div> --> remove() method can take multiple classes as arguments.

//toggle()
card.classList.toggle("new-class"); // <div class="card new-class">Some content</div> --> toggle() method is used to toggle a class on the element. If the class is present, it will remove it. If the class is not present, it will add it.
card.classList.toggle("new-class"); // <div class="card">Some content</div> --> toggle() method is used to toggle a class on the element. If the class is present, it will remove it. If the class is not present, it will add it.

//contains()
console.log(card.classList.contains("card")); // true --> contains() method is used to check if the element has a specific class. It will return true if the class is present, otherwise false.
```

# Element position, sizes, and scrolling

## Offset

Offset properties are used to get the coordinates of the element.

```js run
// <a class="btn--scroll-to" href="#section1">Go to Section 1</a>
const btn = document.querySelector(".btn--scroll-to");

// Element offset properties
console.log(btn.offsetTop); // 100 --> distance from the top edge of the document to the top edge of the element.
console.log(btn.offsetLeft); // 200 --> distance from the left edge of the document to the left edge of the element.
console.log(btn.offsetWidth); // 100 --> width of the element.
console.log(btn.offsetHeight); // 20 --> height of the element.

console.log(window.pageXOffset); // 0 --> horizontal scroll position of the document.
console.log(window.pageYOffset); // 0 --> vertical scroll position of the document.

// Height and width of the viewport
console.log(document.documentElement.clientHeight); // 900 --> height of the viewport.
console.log(document.documentElement.clientWidth); // 1440 --> width of the viewport.
```

## getBoundingClientRect()

`getBoundingClientRect()` method is used to get the coordinates of the element.

Offset properties are relative to the viewport, while client properties are relative to the top-left corner of the document.

```js run
// <a class="btn--scroll-to" href="#section1">Go to Section 1</a>
const btn = document.querySelector(".btn--scroll-to");
console.log(btn.getBoundingClientRect()); // DOMRect {x: 0, y: 0, width: 100, height: 20, top: 0, right: 100, bottom: 20, left: 0}
/*
  x: distance from the left edge of the viewport to the left edge of the element.
  y: distance from the top edge of the viewport to the top edge of the element.
  width: width of the element.
  height: height of the element.
  top: distance from the top edge of the viewport to the top edge of the element.
  right: distance from the left edge of the viewport to the right edge of the element.
  bottom: distance from the top edge of the viewport to the bottom edge of the element.
  left: distance from the left edge of the viewport to the left edge of the element.
  */

//show a tooltip on mouseover
btn.addEventListener("mouseover", function (e) {
  const rect = btn.getBoundingClientRect();
  console.log(rect);
  console.log(e.clientX, e.clientY); // coordinates of the mouse pointer
  console.log(window.pageXOffset, window.pageYOffset); // scroll position
  console.log(rect.top + window.pageYOffset, rect.left + window.pageXOffset); // coordinates of the top-left corner of the element

  const tooltip = document.createElement("div");
  tooltip.classList.add("tooltip");
  tooltip.textContent = "Tooltip";
  tooltip.style.top = `${rect.top + window.pageYOffset}px`; // position the tooltip at the top of the element --> top edge of the element + scroll position
  tooltip.style.left = `${rect.left + window.pageXOffset}px`; // position the tooltip at the left of the element --> left edge of the element + scroll position
  document.body.append(tooltip);

  btn.addEventListener("mouseout", function () {
    tooltip.remove();
  });

  btn.addEventListener("mousemove", function (e) {
    tooltip.style.top = `${e.clientY + 10}px`; // position the tooltip 10px below the mouse pointer
    tooltip.style.left = `${e.clientX + 10}px`; // position the tooltip 10px to the right of the mouse pointer
  }); // move the tooltip with the mouse pointer
});
```

## Scrolling

```js run
// <a class="btn--scroll-to" href="#section1">Go to Section 1</a>

//old way
const btnScrollTo = document.querySelector(".btn--scroll-to");
const section1 = document.querySelector("#section1");

//Add event listener to the button
btnScrollTo.addEventListener("click", function (e) {
  //first get the coordinates of the section
  const s1coords = section1.getBoundingClientRect(); // getClientBoundingRect() method is used to get the coordinates of the element.

  //scroll to the section
  // window.scrollTo( s1coords.left + window.pageXOffset, s1coords.top + window.pageYOffset); // scrollTo() method is used to scroll the document to the specified coordinates.
  window.scrollTo({
    left: s1coords.left + window.pageXOffset,
    top: s1coords.top + window.pageYOffset,
    behavior: "smooth", // smooth scrolling
  });
});

// new way --> works in all modern browsers
btnScrollTo.addEventListener("click", function (e) {
  section1.scrollIntoView({ behavior: "smooth" }); // scrollIntoView() method is used to scroll the element into view.
});
```

# Event and Event Handlers

An event is an action that occurs as a result of user interaction or other actions. (e.g. click, mouseover, keydown, etc.)

## Event Listeners

`addEventListener()` method is used to add an event listener to the element. It takes two arguments: the event name and the callback function.

`removeEventListener()` method is used to remove the event listener from the element.

```js run
// <h1>Hello World</h1>

const h1 = document.querySelector("h1");
const showAlert = function () {
  alert("Hello World");
};
h1.addEventListener("mouseenter", showAlert); // addEventListener() method is used to add an event listener to the element.

// remove the event listener
h1.removeEventListener("mouseenter", showAlert); // removeEventListener() method is used to remove the event listener from the element.

h1.onmouseenter = function () {
  h1.style.color = "red";
  alert("Mouse entered");
}; // old way to add event listener

/*
 addEventListener() method is better because:
 - it allows us to add multiple event listeners to the same element.
 - it allows us to remove the event listener.
*/
```

Mouse events :

- **mouseenter** : Fires when the mouse pointer enters the element.
- **mouseleave**: Fires when the mouse pointer leaves the element.
- **mouseover**: Fires when the mouse pointer is moved onto the element.
- **mouseout**: Fires when the mouse pointer is moved out of the element.
- **mousemove**: Fires when the mouse pointer is moved over the element.
- **mousedown**: Fires when a mouse button is pressed down on the element.
- **mouseup**: Fires when a mouse button is released on the element.
- **click**: Fires when the element is clicked.
- **dblclick**: Fires when the element is double-clicked.
- **contextmenu**: Fires when the right mouse button is clicked on the element, opening the context menu.
- **wheel**: Fires when the mouse wheel is scrolled while the element is in focus.
- **drag**: Fires when an element is dragged.

Stopping right-click in the document and show an alert message : right click is not allowed:

```js run
document.addEventListener("contextmenu", function (e) {
  e.preventDefault(); // prevent the default behavior of the context menu.
  alert("Right click is not allowed");
});
```

Keyboard events :

- **keydown**: Fires when a key is pressed down.
- **keyup**: Fires when a key is released.
- **keypress**: Fires when a key is pressed down and then released.

```js run
document.addEventListener("keydown", function (e) {
  console.log(e.key); // a --> key that is pressed
  console.log(e.code); // KeyA --> code of the key that is pressed
  console.log(e.ctrlKey); // true --> whether the control key is pressed
  console.log(e.altKey); // false --> whether the alt key is pressed
  console.log(e.shiftKey); // false --> whether the shift key is pressed
  console.log(e.metaKey); // false --> whether the meta key is pressed
});
```

## Event Propagation: Bubbling and Capturing

A Sample DOM tree:

```
    ------------
    | Document |
    ------------
        |
        |
        |
    -----------
    | Element |
    | <html>  |
    -----------
        |
        |
        |
    ----------
    | Element |
    | <body>  |
    -----------
        |
        |
        |
    ------------
    |  Element |
    | <section>|
    ------------
        |
        |
        |
    -----------
    | Element |
    |  <p>    |
    -----------
        |
        |
        |
    -----------
    | Element |
    |  <a>    |
    -----------
```

When a click happens on the link `<a>` element, it will trigger the click event, the event is generated on the root element (Document) and from there `capuring phase` starts from the root element and event goes down to the target element. And as the event travels down the DOM tree, it will pass through every parent element of the target element.

As soon as the event reaches the target element, the `target phase` begins. Where events can be handled on the target element using event listeners.

After reaching the target element, the event travels back up the DOM tree in the `bubbling phase`. And just like the capturing phase, the event will pass through every parent element of the target element. (Not sibling elements). It is as if the event happened to each parent element as well.

Which means if we add an event listener to the parent element of the target element, it will also be triggered when the event bubbles up to the parent element. In this case, if we add an event listener to the `<section>` element, it will also be triggered when the `<a>` element is clicked.

This whole process is called `event propagation`.

NOT all events bubble up. Some of them happens only on the target element. (e.g. focus, blur, load, unload, etc.)

Example of event propagation, in which the click event bubbles up the DOM tree:

```html
<html>
  <head>
    <title>A Simple Page</title>
  </head>
  <body>
    <section>
      <p>A paragraph with a <a>link</a></p>
      <p>A second paragraph</p>
    </section>
    <section>
      <img src="dom.png" alt="The DOM" />
    </section>
  </body>
</html>
```

```js run
const section = document.querySelector("section");
const link = document.querySelector("a");

link.addEventListener("click", function (e) {
  e.preventDefault(); // prevent the default behavior of the link.
  alert("Link clicked");
});

section.addEventListener("click", function (e) {
  alert("Section clicked");
});

document.addEventListener("click", function (e) {
  alert("Document clicked");
});

link.click(); // simulate a click on the link

/*
If we click on the link, the alert messages will be shown in the following order:
- Link clicked
- Section clicked
- Document clicked

This is because the click event bubbles up the DOM tree.
*/
```

### Event Bubbling and Capturing example

```html
<nav class="nav">
  <ul class="nav__links">
    <li class="nav__item"><a href="#" class="nav__link">Link 1</a></li>
    <li class="nav__item"><a href="#" class="nav__link">Link 1</a></li>
    <li class="nav__item"><a href="#" class="nav__link">Link 1</a></li>
  </ul>
</nav>
```

```js run
document.querySelector(".nav__link").addEventListener("click", function (e) {
  this.style.backgroundColor = "orange";
  console.log("LINK", e.target);
});

document.querySelector(".nav__links").addEventListener("click", function (e) {
  this.style.backgroundColor = "blue"; //this refers to the element on which the event listener is attached.
  console.log("CONTAINER", e.target);

  // Here e.currentTarget is same as this keyword and e.target is the element on which the event is triggered. currentTarget is the element on which the event listener is attached.

  // e.target  = <a href="#" class="nav__link" style="background-color: orange;">Link 1</a>
  // e.currentTarget = <ul class="nav__links" style="background-color: blue;">...</ul>
});

document.querySelector(".nav").addEventListener("click", function (e) {
  this.style.backgroundColor = "red";
  console.log("NAV", e.target);
});

document.querySelector(".nav__link").click(); // simulate a click on the link

/*
If we click on the link, the console messages will be shown in the following order:

- LINK <a href="#" class="nav__link" style="background-color: orange;">Link 1</a>
- CONTAINER <a href="#" class="nav__link" style="background-color: orange;">Link 1</a>
- NAV <a href="#" class="nav__link" style="background-color: orange;">Link 1</a>

And the background colors of the elements will be changed accordingly.



The target property of the event object will always be the element on which the event listener is attached.

This is because the click event bubbles up the DOM tree.
*/
```

### Stopping Event Propagation

Stopping event propagation is done using the `stopPropagation()` method of the event object. If we stop the event propagation, the event will not bubble up the DOM tree and will not trigger the event listeners on the parent elements.

In the above example, if we stop the event propagation in the event listener of the link element, the event will not bubble up to the parent elements.

```js run
document.querySelector(".nav__link").addEventListener("click", function (e) {
  e.stopPropagation(); // stop the event propagation --> the event will not bubble up to the parent elements.
  this.style.backgroundColor = "orange";
  console.log("LINK", e.target);
});

document.querySelector(".nav__links").addEventListener("click", function (e) {
  this.style.backgroundColor = "blue";
  console.log("CONTAINER", e.target);
}); // this event listener will not be triggered
```
