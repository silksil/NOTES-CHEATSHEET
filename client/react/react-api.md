React is the entry point to the React library. If you load React from a <script> tag, these top-level APIs are available on the React global.

React components let you split the UI into independent, reusable pieces, and think about each piece in isolation. React components can be defined by subclassing React.Component or React.PureComponent.
- React.Component
- React.PureComponent



# Components
React components let you split the UI into independent, reusable pieces, and think about each piece in isolation. React components can be defined by subclassing React.Component or React.PureComponent.

- React.Component
- React.PureComponent

If you don’t use ES6 classes, you may use the create-react-class module instead. See Using React without ES6 for more information.

React components can also be defined as functions which can be wrapped:
- React.memo

# Creating React Elements
We recommend using JSX to describe what your UI should look like. Each JSX element is just syntactic sugar for calling React.createElement(). You will not typically invoke the following methods directly if you are using JSX.

# Transforming Elements
React provides several APIs for manipulating elements:

- cloneElement()
- isValidElement()
- React.Children

# Fragments
React also provides a component for rendering multiple elements without a wrapper.

React.Fragment

# Refs
- React.createRef
- React.forwardRef

# Suspense
Suspense lets components “wait” for something before rendering. Today, Suspense only supports one use case: loading components dynamically with React.lazy. In the future, it will support other use cases like data fetching.
- React.lazy
- React.Suspense

# Hooks
Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class. Hooks have a dedicated docs section and a separate API reference:

### Basic Hooks
- useState
- useEffect
- useContext

### Additional Hooks
- useReducer
- useCallback
- useMemo
- useRef
- useImperativeHandle
- useLayoutEffect
- useDebugValue
