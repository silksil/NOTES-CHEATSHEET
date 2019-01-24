### The DOM
The Document Object Model (DOM) lets our code read from and write to the content of a web-page. It is a programming interface for HTML. We can use it to make our web pages interactive.
When the browser reads an HTML file, it creates a tree-like structure of *nodes*. A node is a branching point containing more nodes. Why the word "node" and not "*element*"? That's because a node is a more general concept than an element. An element is a node, but the text inside elements are also nodes. The tree structure is called the DOM tree. DOM stands for document object model. We use the DOM tree, and related methods, to inspect and control the page.

#### Manipulating The Dom
The HTML elements in the DOM are JavaScript objects with different properties. The properties are represented with keys and values just like ordinary JavaScript objects. Because HTML elements in the DOM are JavaScript objects you can call methods on them. The elements in the DOM have a lot of methods already built in. In order to make it interactive, we have to respond to events. An event is something interesting that happened on the page, like: "someone clicked on a button!". There are lots of events. Reacting to events requires two things:
- An event listener that listens for a specific event on a specific element
- An event handler a javascript function that gets called when the event we are listening for gets fired

You can add JavaScript code inside your HTML to respond to events by assiging a *attribute* to an element. For example you could used the attribute onClick on a <button> element. The text value of the HTML element's attribute is a bit of JavaScript code. When the browser parses the HTML, that code becomes the body of a JavaScript function => the code in the attribute has been wrapped inside an event handler function. 
  
#### Event Object
When 'something interesting' happens on a web-page, the event object describes what that something is. Our functions that we make to respond to events always receive that object as a first parameter. The event object gives us access to useful information about the event. The one you'll probably use the most is Event.target which gives you access to the HTML element on which the event was triggered. In our example, that means the <button> element.
  
```js
  const button2 = document.getElementById('onclick-button2');
button2.onclick = function(event) {
    console.log('I was here second');
}
```

When we assigned a function to the onclick property, it took precedence over the code HTML attribute. So, when the browser reads our page it initializes the DOM element for our button with an onclick property based on the onclick attribute. But afterwards we can change the property directly and we can forget about the attribute.

This raises an important distinction between attributes and properties. Attributes are the name & value pairs in our HTML code, and properties are part of JavaScript objects. We can read and write attributes with getAttribute(name) and setAttribute(name, value) respectively. Meanwhile properties on our DOM elements can be read and assigned like any other JavaScript object.

The biggest disadvantage of using on<event> attributes and properties is that you can only have one event handler per element. With addEventListener we can add as many as we want.
