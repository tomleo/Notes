# Backbone

| Key       | Function                         |
|-----------|----------------------------------|
| C-c C-c l |       markdown-live-preview-mode |
| C-c C-c p |      markdown-preview            |
| C-c C-c v |      markdown-export-and-preview |


## Views

### Rendering

To create elements not attached to the DOM:

``` javascript
Backbone.View.extend({
    tagName: 'li',
});
```

To create elements with container already in the DOM:

``` javascript
Backbone.View.extend({
    el: '#propertyList',
});
```
### setElement

`setElement` will create a cached `$el` reference for you, moving the
delegated events for a view from the old element to the new one.

``` javascript
var button1 = $('<button></button>');
var button2 = $('<button></button>');
var view = new View({el: button1});
view.setElement(button2);
```
