# Day 3 - Playing with CSS variables and JS

Source: JavaScript30 by Wes Bos [CSS Variables](https://courses.wesbos.com/account/access/5e8f3e249edbdf363811ca25/view/194130480)

# Goal Overview

The HTML page has 3 `input`s that represent visual specification of an `img` element. The goal is to have JavaScript to collect the ***value*** of all `input`s and convert to CSS style using ***CSS variables***.

**Note:** same effect can be achieved by using inline CSS style ***ElementCSSInlineStyle.style***

# References

- ***CSS custom property (CSS variables)*** [https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care](https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care)
- ***NodeList va Array*** [https://gomakethings.com/nodelists-vs-arrays/](https://gomakethings.com/nodelists-vs-arrays/)
    - [https://developer.mozilla.org/en-US/docs/Web/API/NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)
- ***dataset*** [https://developer.mozilla.org/en-US/docs/Web/API/HTMLOrForeignElement/dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLOrForeignElement/dataset)
    - [https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)
    - [https://stackoverflow.com/questions/23596751/dataset-vs-data-difference](https://stackoverflow.com/questions/23596751/dataset-vs-data-difference)

# Process

Set the default ***CSS variables values*** in the the document level.

- Use `:root` to define the the document level selector
- Add ***CSS custom properties*** which match with the `name` attribute of the `input`s

Set the JavaScript to correspond with HTML `input`s

- Add an `eventListener` to each `input`s for ***updating values***
    - Consider to have a way to capture the value in real time
- Set a ***callback*** function that can pick up the `name` attribute from the corresponding `input` to specify which ***CSS property*** to update, and pass the ***value*** of `input` to the ***CSS variable***
    - Add the proper ***unit of the value*** according to the ***dataset***
        1. Get the value of the input
        2. Add the suffix of the value from HTML `data-*` attribute, in this case, we can use predefined `data-sizing`
        3. Grab the value of the `name` attribute of the element that called upon.
        4. Update inline style of the `:root`

## Step 1: Declare default CSS custom properties and apply

Declare a new style for the `:root` element and add 3 ***custom properties*** to have them accessible from anywhere of the ***CSS tree***. We can give them default style for now.

**CSS custom property**
Use `--` followed by custom name you want.
In this tutorial, we can use the value of HTML `name` attribute of the `input` for JavaScript use.

```css
:root {
	--base: #ffc600;
	--blur: 10px;
	--spacing: 10px;
}
```

Declare a new style for the `img` element and set the `background-color`, `filter`, and `padding` ***properties*** to the ***variables*** we defined at the `:root` element to apply the default style.

CSS variable
`var(—-propertyName)` to use previously defined CSS properties.

```css
img {
	background-color: var(--base);
	filter: blur(var(--blur));
	padding: var(--spacing);
}
```

In addition, we can declare a new style for the `.hi` class and set the `color` ***property*** to the `base`  ***variable***.

```css
.hi {
	color: var(--base);
}
```

## Step 2: Get the all of the `input` elements on the page in the JavaScript

Get the all of the `input` elements under the `.controls` element.

```jsx
const inputs = document.querySelectorAll('.controls input');
```

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled.png)

Try `document.querySelectorAll('.controls input');` to see all 3 `input`s has return in a ***NodeList*** (this is not an ***array)***

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%201.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%201.png)

Compare a ***NodeList*** to an ***array***

We can break the list down to each item by using `forEach` ***property*** which we will use the to add an `eventListener` to each `input` element to catch `change` event.

```jsx
inputs.forEach(input => input.addEventListener('change', function() {
	console.log(this.name, this.value);
}));
```

Try `console.log(this)` to see what is called against the method. A cheap way to see what's equal to ***this***.

We can detach the ***function*** independently which will handle the update that based on `change` ***event listening.***

```jsx
function handleUpdate() {
	console.log(this.name, this.value);
}

inputs.forEach(input => input.addEventListener('change', handleUpdate));
```

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%202.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%202.png)

The both codes above will return similar outcome.

This allows us to repeat the function for different events. For example, we can also add `mousemove` event as well.

```jsx
inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));
```

## Step 3: Add a recipe for `handleUpdate` function

Check out the `dataset` of the `input` element that called upon. Try `this.dataset`.

```jsx
function handleUpdate() {
	console.log(this.name, this.value, this.dataset);
}
```

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%203.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%203.png)

The `input` element with and `data-*` ***attributes***, will return an ***object*** that contains all `data-*` ***attributes** as properties**.***

> What if we have more than 1 `data-*` ***attributes***?

Update the style of the ***document*** (`:root` level) with the ***CSS property*** upon the `name` ***attribute*** with the ***value*** of the `input` that called against.

Note that we're going to use ***backslash***  for ES6 string template for variable

```jsx
function handleUpdate() {
	// access to 'document' into 'documentElement' into 'style' ...
	document.documentElement.style.setProperty(`--${this.name}`, this.value);

	// test run
	console.log(this.name + ': ' + this.value);
}
```

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%204.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%204.png)

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%205.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%205.png)

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%206.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%206.png)

Notice that the code is working and updating the ***document*** but still not doing much. Because that missing the ***unit*** of the values.

Get the suffix as the unit of the value from the elements' `data-sizing`. Consider the case that we don't get any returned `data-*` attributes.

Use ***dot notation*** to access to the ***property***.

```jsx
function handleUpdate() {
	// get the value of the data-* attribute and if you don't get any, keep it empty (or nothing)
	// with out 'or nothing', it will throw `undefined`
	const suffix = this.dataset.sizing || '';
	
	// test run
	console.log(this.name + ': ' + this.value + suffix);

	document.documentElement.style.setProperty(`--${this.name}`, this.value);
}
```

![Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%207.png](Day%203%20Playing%20with%20CSS%20variables%20and%20JS/Untitled%207.png)

It seems working.

```jsx
function handleUpdate() {
	const suffix = this.dataset.sizing || '';

	// update the value with `suffix`
	document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}
```
