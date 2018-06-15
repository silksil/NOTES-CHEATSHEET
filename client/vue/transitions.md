## Intro Transitions
Vue provides a variety of ways to apply transition effects when items are inserted, updated, or removed from the DOM.

## Fade
```html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```
```javascript
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

```css
.fade-enter-active {
    transition: opacity .5s
}
.fade-enter, .fade-leave-active {
    opacity: 0
}
```



