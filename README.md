# SkipOnTab javascript library

* A [jQuery](http://jquery.com/) plugin to
 * exempt selected fields from the forward tab order.
 * include excluded fields in the reverse tab order.

When using <kbd>tab</kbd> to navigate through a form, skipping some fields will reduce key presses for the normal use cases. Skipped fields can still be navigated to by keyboard; once skipped and focusing the next form field, use <kbd>shift</kbd>-<kbd>tab</kbd> to step back. Mouse or touch navigation is unaffected.

This library is most useful when the users are familiar with the form, and uses it regularly. Casual users may not feel as comfortable - then again, if they are already using the <kbd>tab</kbd> button, they might see it as an optimization.

## Demos
* `examples/demo.html`: Simple demo for playing around.
* `examples/skip-some-fields-in-order-form.html`: Expanded demo with some thoughts on what fields to skip.

## Usage

### HTML

```html
<!-- Can be applied to skippable elements one by one -->
<input type="text" data-skip-on-tab="true" />
<textarea data-skip-on-tab="true"></textarea>
<a href="http://joelpurra.se/" data-skip-on-tab="true">Joel Purra</a>

<input type="button" value="This button is not skipped" />

<!-- Can be applied using a class name -->
<input type="text" value="" class="skip-on-tab" />

<!-- Can be applied to all skippable elements within a container -->
<ol data-skip-on-tab="true">
	<li><input type="checkbox" /> Checkbox</li>
	<li><input type="checkbox" /> Another checkbox</li>

	<!-- Can be explicitly exluded from skipping -->
	<li><input type="checkbox" data-skip-on-tab="false" /> Important checkbox</li>
	<li><input type="checkbox" class="disable-skip-on-tab" /> Another important checkbox</li>
</ol>
```

### Javascript

```javascript
// Apply skip on tab to the selected elements
$(selector).skipOnTab();

// Equivalent static function
JoelPurra.SkipOnTab.skipOnTab($(selector));
```

### Skippable elements
Elements that can be focused/tabbed include `<input>`, `<select>`, `<textarea>`, `<button>` and `<a href="...">` (the `href` attribute must exist). These are also the elements that can be skipped.

Note that `<input type="hidden" />`, `<a>` (without `href`), `disabled="disabled"` or `display: none;` elements cannot be focused.

### Static elements
Static skippable html elements can have, or be contained within elements that have, the attribute `data-skip-on-tab="true"` or the class `.skip-on-tab`. They are enabled automatically when the library has been loaded/executed.

### Dynamic elements
Dynamic elements are initialized to SkipOnTab in code after adding them to the DOM; `$("#my-optional-input").skipOnTab()`. This is not necessary if the added element already is contained within an element that is marked for skipping. You can also call `.skipOnTab()` on containers.

### Containers
When SkipOnTab is applied to html containers, like `<div>`, `<ul>` or `<fieldset>`, all skippable child elements are implicitly skipped. This applies to static html and subsequently added child elements.

### Disabling skipping
Skippable elements, or containers with skippable children, marked with class `.disable-skip-on-tab` or attribute `data-skip-on-tab=false` are never skipped. If skipping is disabled for the element when it receives focus, or any of its elements parents, it will not be skipped. Disabling skipping takes precedence over enabling skipping.

## Original purpose
Developed to skip less used form fields in a web application for registering and administering letters. Examples of skipped fields are dropdowns with sensible defaults, the second address line fields in address forms and buttons for seldom used actions.

## SkipOnTab versus tabindex
SkipOnTab does *not* rely on setting [`tabindex`](http://www.w3.org/TR/html4/interact/forms.html#h-17.11.1) on elements - it uses javascript events instead. Having some experience in trying to use and maintain a site using tabindex, I would not use it again.

Drawbacks when using tabindex

* It must be used consistently throughout:
 * [The entire form or you'll confuse the users](http://nickdenardis.com/2009/09/23/avoid-frustrating-users-with-tabindex/). Focus might end up somewhere unexpected in the form, or drop out of the form all toghether, when an element is missing a tabindex.
 * All forms in the entire page in order to avoid unexpected orders. This includes site-wide headers and footer.
 * Any re-used page components. Tabindex ranges cannot overlap, nor can they be used in different orders on different pages without re-calculating tabindex (perhaps by using a base value per component instance).
* Setting and keeping track of tabindex, be it statically or dynamically, can become a hassle when inserting/showing a new focusable element or the page layout is changed.
* An element skipped with tabindex cannot be reached by tabbing without
 * Tabbing through the reset of the form (if it has a tabindex just higher than the tabindex of the last focusable element in the form).
 * Tabbing through all the menus (since they are usually at the top, before the form) and links on the page.

Getting and setting tabindex [doesn't act consistently across browsers](http://fluidproject.org/blog/2008/01/09/getting-setting-and-removing-tabindex-values-with-javascript/), but the [Keyboard Accessibility Plugin](http://wiki.fluidproject.org/display/fluid/Keyboard+Accessibility+Plugin+API) should help with that and a lot of other useful things.

Drawbacks when using SkipOnTab

* It requires [javascript enabled](http://enable-javascript.com/) browsers.
* It requires [jQuery](http://jquery.com/).
* It only moves to the next focusable element in the form/page, not to a specific element.

SkipOnTab is fully dynamic in the way it detects and moves focus.

## Dependencies
SkipOnTab's only runtime dependencies is [jQuery](http://jquery.com/).

## Todo

* [jQuery UI](http://jqueryui.com/) has better code for [`:focusable`](https://github.com/jquery/jquery-ui/blob/master/ui/jquery.ui.core.js#L210)/[`:tabbable`](https://github.com/jquery/jquery-ui/blob/master/ui/jquery.ui.core.js#L214). Investigate how to implement it.
* Investigate [`[contenteditable]`](http://www.whatwg.org/specs/web-apps/current-work/#contenteditable).
* Investigate focusing/skipping non-input elements with [`[tabindex]`](http://www.w3.org/TR/html4/interact/forms.html#h-17.11.1) and negative values value.
* Break out reusable <kbd>tab</kbd> key `.focus()` emulator functions.
* Break out reusable key press functions from tests.
* Investigate how usable `data-skip-on-tab="#id-of-next-element-in-the-order"` would be.

## License
Developed for PTS by Joel Purra <http://joelpurra.se/>

Copyright (c) 2011, 2012, The Swedish Post and Telecom Authority (PTS)
All rights reserved.

Released under the BSD license.
