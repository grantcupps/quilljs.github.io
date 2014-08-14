---
layout: post
permalink: /blog/building-a-custom-module/
javascript: //codepen.io/assets/embed/ei.js
---

Quill's core strength as an editor is its rich API and powerful customization capabilities. One of the best ways to customize Quill is with a module. A module is a simple way to augment Quill's functionality. For the purpose of this guide, we will walk through one way to build a word counter module, a commonly found feature in many word processors.

### Counting Words

At its core a word counter simply counts the number of words in the editor and displays this value in some UI. Thus we need to:

1. Listen on text changes in Quill.
1. Count the number of words.
1. Display this value.

<!-- more -->

Let's just in with a complete example:

<div data-height="366" data-theme-id="7845" data-slug-hash="qkniF" data-default-tab="js" class='codepen'><pre><code>
// Implement and register module
Quill.registerModule('counter', function(quill, options) {
  var container = document.querySelector('#counter');
  quill.on('text-change', function() {
    var text = quill.getText();
    container.innerHTML = text.split(/\s+/).length;
  });
});

// We can now initialize Quill with something like this:
var quill = new Quill('#editor', {
  modules: {
    counter: true
  }
});
</code></pre></div>

That's all it takes to add a custom module to Quill. A function can be registered as a module and it will be passed the corresponding Quill editor object along with any options.

### Using Options

Modules are passed an options object that can be used to fine tune the desired behavior. We can use this to accept a selector for the counter container. Let's also customize the counter to either count words or characters:

<div data-height="466" data-theme-id="7845" data-slug-hash="eKIBb" data-default-tab="js" class='codepen'><pre><code>
Quill.registerModule('counter', function(quill, options) {
  var container = document.querySelector(options.container);
  quill.on('text-change', function() {
    var text = quill.getText();
    if (options.unit === 'word') {
      container.innerHTML = text.split(/\s+/).length + ' words';
    } else {
      container.innerHTML = text.length + ' characters';
    }
  });
});

var quill = new Quill('#editor', {
  modules: {
    counter: {
      container: '#counter',
      unit: 'word'
    }
  }
});
</code></pre></div>

### Constructors

Since any function can be registered as a Quill module, we could have implemented our counter as a constructor. This allows us to access and utilize the module directly. More complex modules may benefit from this design pattern.

<div data-height="666" data-theme-id="7845" data-slug-hash="zCIur" data-default-tab="js" class='codepen'><pre><code>
var Counter = function(quill, options) {
  this.quill = quill;
  this.options = options;
  var container = document.querySelector(options.container);
  var _this = this;
  quill.on('text-change', function() {
    var length = _this.calculate();
    container.innerHTML = length + ' ' + options.unit + 's';
  });
};

Counter.prototype.calculate = function() {
  var text = this.quill.getText();
  if (this.options.unit === 'word') {
    return text.split(/\s+/).length;
  } else {
    return text.length;
  }
};

Quill.registerModule('counter', Counter);

var quill = new Quill('#editor');
var counter = quill.addModule('counter', {
  container: '#counter',
  unit: 'word'
});

// We can now access calculate() directly
console.log(counter.calculate(), 'words');
</code></pre></div>

### Wrapping It All Up

Now let's polish off the module and fix a couple bugs you may or may not have noticed.

<div data-height="766" data-theme-id="7845" data-slug-hash="wxtvI" data-default-tab="js" class='codepen'><pre><code>
var Counter = function(quill, options) {
  this.quill = quill;
  this.options = options;
  this.container = document.querySelector(options.container);
  quill.on('text-change', this.update.bind(this));
  this.update();  // Account for initial contents
};

Counter.prototype.calculate = function() {
  var text = this.quill.getText();
  if (this.options.unit === 'word') {
    text = text.trim();
    // Splitting empty text returns a non-empty array
    return text.length > 0 ? text.split(/\s+/).length : 0;
  } else {
    return text.length;
  }
};

Counter.prototype.update = function() {
  var length = this.calculate();
  var label = this.options.unit;
  if (length !== 1) {
    label += 's';
  }
  this.container.innerHTML = length + ' ' + label;
}

Quill.registerModule('counter', Counter);

var quill = new Quill('#editor');
var counter = quill.addModule('counter', {
  container: '#counter',
  unit: 'word'
});
</code></pre></div>

And that's all there is to it! Let us know what you think of this guide or [Quill](http://www.twitter.com/quilljs).