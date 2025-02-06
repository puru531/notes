# DOM and Event Fundamentals

DOCUMENT OBJECT MODEL : Structured representation of HTML documnents. Allows Javascript to access html elements and styles to manipulate them. <br>
Dom is automatically created by the browser as soon as the HTML page loads. Stored in a tree structure.
<br>
DOM and DOM methods are part of web API, libraries that browsers implement and we can access from Javascript code.

### Selecting and manipulating Elements

```js run
//select the element
document.querySelector(".message"); //selecting a element having class name message.
document.getElementById("id1"); //selects elemnet by id.
document.getElementByClassname("modal"); //selects elemnet by class.
document.getElementByTagName("h1"); //selects elemnet by tag name.

//get and set the text content
document.querySelector(".message").textContent; //getting text inside the element.
document.querySelector(".message").textContent = "Some other text"; //setting text inside the element.

//get and set the value
console.log(document.querySelector(".message").value); //getting value inside the element (input element).
document.querySelector(".message").value = "Some other text"; //setting value inside the element (input element).
```

### Handling Click Events

With an eventlistener we can wait for a certain action (keyboard keypress, mouse click, mouse hover, etc.) to happen and react to it.

```js run
//Listening to click of a button, addEventListener takes two arguments 1. type of event to listen to, 2. function : which contains what we want to do on that event.

document.querySelector(".btn").addEventListener("click", function () {
  console.log("button clicked");
});
```

### Manipulating CSS Styles

```js run
//Style attributes are camelCase.
document.querySelector("body").style.backgroundColor = "green";
document.querySelector(".number").style.width = "30rem";
```

NOTE : querySelector selects only the first element, event if there are multiple elements with same class :

```js run
html :
<button class="btn">Button1</button>
<button class="btn">Button2</button>
<button class="btn">Button3</button>

js :
document.querySelector('.btn')  // this will select : <button class="btn">Button1</button> only

//To select all : querySelectorAll
document.querySelectorAll('.btn'); //give NodeList containing all elements.
```

### Manipulate classes

```js run
document.querySelector(".modal").classList; //returns array of classes available in the element

//Remove classes from an element
document.querySelector(".modal").classList.remove("hidden"); //will remove the hidden class.
document.querySelector(".modal").classList.remove("hidden", "anyOther"); //will remove both classes

//Add classes
document.querySelector(".modal").classList.add("hidden");
```

### Handling keyboard Keypress Event

Keyboard events are global events and they do not occur on any specific element, so we listen to the whole document.

```js run
//three types of events for the keyboard : keydown, keyup, keypress
keydown : emits event as soon as we press the key
keyup : emits as we leave pressing the key
keypress : emits for evry press or if we keep pressing the key.

// eventObj = after any action, we get an event object which contains all information about object.{key, code, location, ctrlKey, altKey, etc..}

document.addEventListener('keydown', function(eventObj) {
    if(eventObj.key === 'Escape') {
        console.log('Esc key was pressed);
    }
});
```
