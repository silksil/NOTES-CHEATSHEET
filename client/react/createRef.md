# When to Use
Frameworks, like React, are build with a component-based architecture. As a result, it is more common to hear about components than it is to hear about DOM-based interactions since most of those are encapsulated within the component.
While you can do a lot with these modern JavaScript frameworks by leveraging on their built-in functionalities, there are times you need to interact with the actual DOM for some native behavior. Most of the modern frameworks provide APIs through which you can access the native DOM representation of your app, and React is not an exception.
React provides a feature known as refs that allow for DOM access from components. You simply attach a ref to an element in your application to provide access to the element’s DOM from anywhere within your component. Refs can also be used to provide direct access to React elements and not just DOM nodes. Generally, the use of refs should be considered only when the required interaction cannot be achieved using the mechanisms of state and props. However, there are a couple of cases where using a ref is appropriate. One of which is when integrating with third-party DOM libraries. Also, deep interactions such as handling text selections or managing media playback behavior also require the use of refs on the corresponding elements.

# How to create Refs
React provides three major ways of creating refs. Here is a list of the different methods starting from the oldest of them:
- String Refs (legacy method)
- Callback Refs
- React.createRef (from React 16.3)

The focus of the remaining article will be on createRef.

## createRef
You create a ref calling React.createRef() and assign the resulting ref to an element.Note that we **can only create refs on class components** since they create an instance of the class when mounted. Refs cannot be used on functional components.