There are multiple ways to use animation in React.

### 1. CSS Transition
The most assumable way is to use CSS transitions.

Let's say we have a situation where we show something based on a piece of state. In order to do this, we create the following React code:
```js
import React from "react";

import "./Modal.css";

const modal = props => {
  const cssClasses = [
    "Modal",
    props.show ? "ModalOpen" : "ModalClosed"
  ];

  return (
    <div className={cssClasses.join(' ')}>
      <h1>A Modal</h1>
      <button className="Button" onClick={props.closed}>
        Dismiss
      </button>
    </div>
  );
};

export default modal;
```

As you see, we have 3 classes. We also we include Css code for the three classes.
```cs
.Modal {
    position: fixed;
    z-index: 200;
    border: 1px solid #eee;
    box-shadow: 0 2px 2px #ccc;
    background-color: white;
    padding: 10px;
    text-align: center;
    box-sizing: border-box;
    top: 30%;
    left: 25%;
    width: 50%;
    // 'transition': you say apply it not instantly, but animate it over time
    // 'all': apply it to all properties
    // 'ease-out': you say start faster than you end
    transition: all 0.3s ease-out;
}

.ModalOpen {
    opacity: 1;
    // position it on the same position as in the html
    transform: translateY(0);
}

.ModalClosed {
    opacity: 0;
    // move it 100% up
    transform: translateY(-100);
}
```
### 2. CSS Animation
As an alternative to css transition, we could use css animation. It allows you to define a more complex animation than transition. In order to do it, you will create `keyframes`:
```cs
.Modal {
    position: fixed;
    z-index: 200;
    border: 1px solid #eee;
    box-shadow: 0 2px 2px #ccc;
    background-color: white;
    padding: 10px;
    text-align: center;
    box-sizing: border-box;
    top: 30%;
    left: 25%;
    width: 50%;
    transition: all 0.3s ease-out;
}

// To make the keyframes work, you have to add the animation property
// Refer to the `openModal` keyframe
// You have to define the timing
// 'ease-out' define how it moves: slower to faster
// To state you want to keep the final state (100%) you can the 'forward' property
.ModalOpen {
    animation: openModal 0.4s ease-out forwards;
}

.ModalClosed {
    animation: closeModal 0.4s ease-out forwards;
}

// you can define in each individual period of time
// how the component should look
// In this case, we define how it looks in 3 periods of time
@keyframes openModal {
    0% {
        opacity: 0;
        transform: translateY(-100%);
    }
    50% {
        opacity: 1;
        transform: translateY(90%);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes closeModal {
    0% {
        opacity: 1;
        transform: translateY(0);
    }
    50% {
        opacity: 0.8;
        transform: translateY(60%);
    }
    100% {
        opacity: 0;
        transform: translateY(-100%);
    }
}
```
### Limitations CSS Transition and animation
The disadvantage is that all the components in all cases still exist in the the DOM, even though it is not visible. As an alternative you could choose to not include a component in the DOM based on the state. This will work when then clicking on the button that includes a component, but the animation will not work if you remove it from the DOM, as it won't wait for the animation.

### ReactTransitionGroup
As a solution to the limitation above we can use ReactTransitionGroup. It allows you to smoothly animate elements if they are added or moved from the DOM.

`npm install react-transition-group --save`



```js
import React from "react";
import CSSTransition from "react-transition-group/CSSTransition";

import "./Modal.css";

const animationTiming = {
    enter: 400,
    exit: 1000
};

const modal = props => {
  return (
    <CSSTransition
        mountOnEnter
        unmountOnExit
        in={props.show}
        timeout={animationTiming}
        classNames={{
            enter: '',
            enterActive: 'ModalOpen',
            exit: '',
            exitActive: 'ModalClosed'
        }}>
          <div className="Modal">
            <h1>A Modal</h1>
            <button className="Button" onClick={props.closed}>
              Dismiss
            </button>
          </div>
    </CSSTransition>
  );
};

export default modal;
```
