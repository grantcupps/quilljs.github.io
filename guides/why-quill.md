---
layout: guide
title: Why Quill
permalink: /guides/why-quill/
redirect_from: /guides/
---

Content creation has been at the core to the web since its beginning. The `<textarea>` provides a native and essential solution to almost any web applicaiton. But at some point you may need to add formatting to text input. This is where rich text editors come in. There are many solutions to choose from, but Quill brings a few modern ideas to consider.


### API Driven Design

Rich text editors are built to help people write text. Yet surprisingly, most rich text editors have no idea what text the user composed. They see their content through the same lens a web developer does: the DOM. But the DOM is made up of Nodes organized in an unbalanced tree, whereas text is made up of lines, words and characters. There is no DOM API where characters is the unit of measure. With this limitation, most rich text editors cannot answer simple questions such as "What text is in this range?" or "Is the cursor on bolded text?" Trying to build rich editing experiences on top of such primitives is very difficult and frustrating.

Quill was designed with text and characters in mind and built its APIs on top of these units. To find out if something is bold in Quill does not require traversing a tree looking for a `<b>` or `<strong>` tag or a font-weight style&mdash;just call [`getFormat(5, 1)`](/docs/api/#getformat). All of its core [API](/docs/api/) calls allow arbitrary indexes and lengths for access or modification. Its [event API](/docs/api/#events) also reports changes in an intuitive JSON format. No need to parse HTML or diff DOM trees.


### Custom Content and Formatting

It was not far in the past that evaluating rich text editors was as simple as comparing a checklist of desired formats. The mark of a good rich text editor was simply how many formats it supported. This is still important measure, but the lower bound is approaching infinity.

Text is no longer written to be printed. It is written to be rendered on the web&mdash;a much richer canvas than paper. Content can be live, interactive, or even collaborative. Only some rich text editors can even support rich media like images and videos. Almost none can embed a tweet or interactive graph. Yet this is the direction the web is moving: richer and more interactive. The tools supporting content creation need to consider these use cases.

Quill exposes its own document model for extension and customization. The upper limit on the formats and content it can support is unlimited. Users have already used it to embed slide decks, language translation autocompletion, and executable models.


### Cross Platform

Cross platform support is important to many Javascript libraries. But the criteria for what this means often differs. For Quill, the bar is not just that it runs or works, it has to run or work *the same way*. In this way, not only is functionality a cross platform consideration, but user and developer experience is as well. If some content produces a particular markup in Chrome on OSX, it will produce the same markup on IE. If hitting enter preserves bold format state in Firefox on Windows, it will be preserved on mobile Safari.


### Easy to Use

All of these benefits come in an easy to use package. Quill ships with sane defaults you can immediately use with just a few lines of Javascript:

```js
var quill = new Quill('#editor', {
  modules: { toolbar: true },
  theme: 'snow'
});
```

If your application never demands it, you never have to customize Quill&mdash;just enjoy the rich and consistent experience that comes out of the box.

Enjoy!
