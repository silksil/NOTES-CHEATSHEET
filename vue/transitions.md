## Intro Transitions
Vue provides a variety of ways to apply transition effects when items are inserted, updated, or removed from the DOM.

## Fade
```html
<transition name="fade">
    <div>
    </div>
<transition> 
```

```css
.fade-enter-active {
    transition: opacity .5s
}
.fade-enter, .fade-leave-active {
    opacity: 0
}
```



