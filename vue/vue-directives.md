
### v-model 
You can use the v-model directive to create two-way data bindings on form input and textarea elements. 
```html
<input v-model="data-property">
```

### v-bind
Dynamically bind one or more attributes, or a component prop to an expression
```html
<img v-bind:src="imageSrc">
```

### v-if & else
To wrote conditionial blocks. 
```html
<div class="search-results" v-if="loading">
  loading...
</div>
<div class="search-results" v-else>
  Found {{items.length}} results for search term {{ search}}
</div>
```
