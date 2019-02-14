# Lists

To display a list next to each other use inline-block

```css
li {
  display: inline-block;
}
```

# Show / Don't Show

Overflow
Z-index

# Length Units

## Absolute Lenghts

### px

With pixels the size is gonna by always the same size, no matter how big the screen.

### Relative Lenghts

### em

Em's are relative to the default font size to the root of the root element, which is the html element, until one of one of it's children changes, which than will pass that font size to it's children. If element is 16 px, 10em is 160px.

### rem

Rm is only relative to the root size of the html element. It doesn't matter what font size the element or the parent has.

## %

Relative to it's parent.

## vh & vw

One unit on any of the three values is 1% of the viewport axis. "Viewport" == browser window size == window objct. If the viewport is 40cm wide, 1vw == 0.4cm.
