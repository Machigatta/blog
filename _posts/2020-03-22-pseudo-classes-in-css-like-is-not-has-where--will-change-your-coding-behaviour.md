---
layout: post
title:  "Pseudo-Classes in CSS like :is(), :not(), :has() and :where() will change your coding behaviour"
date:   2020-03-22 19:00:00 +0200
categories: Workflow
---
Most of the time, when you are building a webpage, you are using a prefabricated layout or design. Programs like Adobe XD might be perfect for Designing a graphical user-interface or webpage, but in the end it has to be written in some kind of Code for the Webserver to serve. Given you have a complex Design with UI-Changes after pressing a button, chances are high you will be in need for using additional code to implement those changes.

# The Pseudo-Classes
The official [CSS Editors Draft](https://drafts.csswg.org/selectors-4/#relational) for selectors lists the now widely available pseudo-classes.

For all examples  i wil use this basic html-markup to explain the behaviour.

```html
<p id="FirstParagraph">
  <span class="First">First</span>
  <span class="Second">Second</span>
  <span class="Third">Third</span>
</p>
<p id="SecondParagraph">
  <span class="First">First</span>
  <span class="Second">Second</span>
  <span class="Third">Third</span>
</p>
<p id="ThirdParagraph">
  <span class="First">First</span>
  <span class="Second">Second</span>
  <span class="Third">Third</span>
</p>
```

## :is()
Lets say you want to highlight the first and second span out of the first paragrah on hovering over it. Your first approach would probably ha a selector list and would like this:
```css
#FirstParagraph > span.First:hover,
#FirstParagraph > span.Second:hover{
  background: red;
  cursor: pointer;
}
```

Here our pseudo-class comes in handy and we can simplify this into:

```css
#FirstParagraph > span:is(.First, .Second):hover {
  background: red;
  cursor: pointer;
}
```

The pseudo-class takes in a simple selector-list and can be advanced with even more selectors. Just for the record, this could look something like this:

```css
#FirstParagraph > span:is(.First, .Second):is(*:first-child):hover {
  background: red;
  cursor: pointer;
}
```
This would result in a great conditional `:hover` pseudo-class. It would only apply a `:hover` on the first span with a class of `First` or `Second`. Meaning, it would apply only to our first span. If we would delete this span, it would apply to the second span. If we would delete the second as well, the rule is not gripping to any element, since we set the scope only to our specific classes.

## :not()
This one is more or less the oposite of `:is()`.

```css
#FirstParagraph > span:not(.First, .Second):hover {
  background: green;
  cursor: pointer;
}
```

Would only add the `:hover` pseudo-class to our third span. In a more real-scenario you are more likely to use it to exclude IDs or Classes from greater amounts of objects.

```css
p:not(#FirstParagraph) > span:not(.First, .Second):hover {
  background: green;
  cursor: pointer;
}
```

This would apply the `:hover` to every third span in a paragraph except for the one with the ID of `FirstParagraph`.

## :has()
This one might be the most promising, but has a major downside. For that go to [Browser Support](#browser-support).

```css
p:has(> span) {
  background: green;
}
```

It would bring a green background to a paragraph which has a directional child of the type `span`. The best thing about this new class is probably the combination possibilitys.

```css
p:not(:has(> span)) {
  background: red;
}
```
This for an example would highlight every paragraph red, which does not has directional children of the type `span`.

## :where()
This one may be the trickyest one to understand. First of, it shares the funcionality and syntax with the `:is()` class. To understand why it differs from it's behaviour you need to understand the specificity of CSS. The rules behind the calculation of a specificity is well documented [here](https://drafts.csswg.org/selectors-4/#specificity-rules). To break it down to a more understanable way: the more specific the selector, the higher the priority is the rule.

As an example:

```css
p > span:not(:hover) {
  color:red;
}

p > span {
  border: 2px solid gray;
  color: green;
}
```
This would display our `span` in a red color. With the usage of `:where()` however, we are using a specificity of 0. So we can have our beautiful green back:

```css
p > span:where(:not(:hover)) {
  color:red;
}

p > span {
  border: 2px solid gray;
  color: green;
}
```

But what in the world would you need such things for ? That's simple: __Overriding__. 
Many times a new Programmer or Designer would use the Sequence of their _CSS_ Rules to override possible attributes... or even use the widely used `!important` to force it. 

A good example for usage of this class would be the following (The Comments explains the workflow):
```css
/*FirstParagraph and SecondParagraph will be red*/
p:is(#FirstParagraph, #SecondParagraph) > span {
  color: red;
}

/*SecondParagraph and ThirdParagraph will be orange if no other rule is applied*/
p:where(#SecondParagraph, #ThirdParagraph) > span {
  color: orange;
}

/*Oh noez another Rule got applied => ThirdParagraph will be green instead of orange*/
p#ThirdParagraph > span {
  color: green;
}
```

The result of this code is a red `#FirstParagraph`, `#SecondParagraph` and a green `#ThirdParagraph`. All that due to the specificity.

# Conclusion

## Why to use them?

These pseudo-classes are a powerful tool to give your page a well thought conditional styling. It also enables the user to discard some of their JS-code in old fashioned projects. It also gives you the oportunity to minifiy some of your existing _CSS_ Rules or future ones.

## The Downside
__Not supported Pseudo-Elements__

This means you are not able to select `::before` or `::after` for example. Besides from that and some restrictions on the browser support for `:has` there is only one more thing to keep in mind. Giving that more and more webpages are created by JavaScript Frameworks like _Vue.js_, _React_ or _Angular_, you have to keep in mind, that those bring conditional rendering with them. That means your html-dom will change based on variables in the JS-Logic. This will obviously reflect on the interpretation of your _CSS_ Rules as well.

# Browser Support
Dating today, most of the Browsers out there support `:is()`, `:not()` and `:where()`. And there not that much concerns about the ones who dont. Sadly we have little Looser here... `:has()` is not yet supported from any of the browsers out there. 

List of Browsersupport from `caniuse`:
- [:is()](https://caniuse.com/?search=%3Ais)
- [:not()](https://caniuse.com/?search=%3Anot)
- [:has()](https://caniuse.com/?search=%3Ahas)
- [:where()](https://caniuse.com/?search=%3Awhere)
