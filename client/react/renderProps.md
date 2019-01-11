### The problem with higher-order components
Higher-order component pattern is a popular method of code reuse in React codebases, but this can be replaced with a regular component called “render prop”. Two problems arrise with HOC's:
- Indirection: We can't know which HOC provides withc props.
- Naming collision: Two HOCs that try to use the same prop name will collide and overwrite one another. 
- Require a lot of boilerplate: HOCs introduce a lot of ceremony due to the fact that they wrap components and create new ones instead of being mixed in to existing components. The component that is returned from the HOC needs to act as similarly as it can to the component that it wraps (it should take the same props, etc.) This fact alone requires a lot of boilerplate code just to build a robust HOC.

### What is a render prop
A render prop is a function prop that a component uses to know what to render.
More generally speaking, the idea is this: instead of “mixing in” or decorating a component to share behavior, just render a regular component with a function prop that it can use to share some state with you.

This technique avoids all of the problems we had with mixins and HOCs:

- ES6 classes. Yep, not a problem. We can use render props with components that are created using ES6 classes.
- Indirection. We don’t have to wonder where our state or props are coming from. We can see them in the render prop’s argument list.
- Naming collisions. There is no automatic merging of property names, so there is no chance for a naming collision.

Also:
- And there’s absolutely no ceremony required to use a render prop because you’re not wrapping or decorating some other component. It’s just a function! Actually, if you’re using TypeScript or Flow, you’ll probably find it much easier to write a type definition for your component with a render prop than its equivalent HOC. Again, a topic for a separate post!
- Additionally, the composition model here is dynamic! Everything happens inside of render, so we get to take full advantage of the React lifecycle and the natural flow of props & state.
