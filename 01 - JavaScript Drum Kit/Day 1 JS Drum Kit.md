# Day 1 - JS Drum Kit

Created: Apr 13, 2020 10:36 AM
Reference: https://github.com/nitishdayal/JavaScript30/tree/master/exercises/01%20-%20JavaScript%20Drum%20Kit
Source: https://courses.wesbos.com/account/access/5e8f3e249edbdf363811ca25/view/194130650

# Goal Overview

An HTML page displays a collection of `div` elements, each containing a letter that corresponds with a key on the keyboard, and the name of the soundclip to be played when that button is clicked. When a user presses a key that matches one of the letters displayed in the `div` elements, the page should play the corresponding soundclip and place a visual feedback on the `div`.

# References

- `<kbd>` [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd)
- ***data attribute*** [https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)
- ***event target*** [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- ***event type*** [https://developer.mozilla.org/en-US/docs/Web/Events](https://developer.mozilla.org/en-US/docs/Web/Events)
- ***this*** [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

# Process

The starter code provides HTML and resources necessary to create this page.

- HTML `data-*` attributes: Introduced in HTML5, `data-*` attributes (where * can be anything you want to define) allow us to store *custom data* on any HTML element. Each `div.key` (`<div class="key" data-key="..."`) and `audio` element in the provided HTML file has a `data-key` attribute which corresponds with a keyboard button.
- CSS pre-defined styles: `playing` class defined the style of visual feedback.
- Adding JavaScript to control: The `script` will add and remove this class by detecting the corresponding key pressed.

## Step 1: Add an event listener to detect an interaction and add a callback to trigger behavior

Add an ***event listener*** to the entire window object that is listening for a `keydown` event; the function that we will provide as the ***callback*** will be defined next.

Set the target as a `window` to listen by adding `eventListener`

    window.addEventListerner();

Set an ***event type*** as `keydown` to listen for

    window.addEventListener('keydown');

Set a listener by adding a ***callback*** function which will give us an ***event***(`e` stands for) when that ***event***(`keydown`) detected

    window.addEventListener('keydown', function(e) {
    	// function will give us an event
    	console.log(e);
    	// test run
    });

Check out console to see what `e` delivers

![Day%201%20JS%20Drum%20Kit/Untitled.png](Day%201%20JS%20Drum%20Kit/Untitled.png)

![Day%201%20JS%20Drum%20Kit/Untitled%201.png](Day%201%20JS%20Drum%20Kit/Untitled%201.png)

Extract `keyCode` object

    window.addEventListener('keydown', function(e) {
    	// extract `keyCode` object
    	console.log(e.keyCode);
    });

![Day%201%20JS%20Drum%20Kit/Untitled%202.png](Day%201%20JS%20Drum%20Kit/Untitled%202.png)

## Step 2: Pick up the matching `audio` and play

Create a variable that can pick up the `audio` element corresponds to the representation of `keyCode`, in this case - `data-key`

In other word, I need a variable that stores the `audio` element with `data-key` which matches with `keyCode` from the ***event*** when the `window` detects the ***event listener*** for `keydown`.

    window.addEventListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	// use ES6 template string ${...}
    	console.log(audio);
    	// test run
    });

**Tip:** To match the HTML `data-key=".."` , add ***double quotation marks*** attribute selector.

![Day%201%20JS%20Drum%20Kit/Untitled%203.png](Day%201%20JS%20Drum%20Kit/Untitled%203.png)

Note: It return `null` when the unassigned keys are pressed.

Taking care of `null` case

    window.addEvenTListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	if (!audio) return; // stop the funtion from running all together
    });

Play `audio`

    window.addEventListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	if (!audio) return;
    
    	audio.play(); // when hit the same key consecutive times, "oh, why would I do that? I'm clearly already playing with that."
    });

Rewind the `audio` before invoking `.play` , even though the browser is playing the sound as an ***execution***, this will tell browser that the `audio` can be start again to play

    window.addEventListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	if (!audio) return;
    	
    	audio.currentTime = 0; // rewind to the starting point
    	audio.play();
    
    	console.log(audio);
    });

![Day%201%20JS%20Drum%20Kit/Untitled%204.png](Day%201%20JS%20Drum%20Kit/Untitled%204.png)

Note: It plays the sound as much as the key pressed - you can compare with the log

## Step 3: Pick up the matching `div` and manipulate the style

Create a variable that can pick up the `div.key` element corresponds to the representation of `keyCode`, in this case - `data-key` as well.

In other word, I need a variable that stores the `div.key` element with `data-key` which matches with `keyCode` from the ***event*** when the `window` detects the ***event listener*** for `keydown`.

    window.addEventListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    	if (!audio) return;
    	
    	audio.currentTime = 0; // rewind to the starting point
    	audio.play();
    
    	console.log(key); // test to see if it picks up the right element
    });

![Day%201%20JS%20Drum%20Kit/Untitled%205.png](Day%201%20JS%20Drum%20Kit/Untitled%205.png)

Add a class name to the `key` element when the function's invoking.

    window.addEventListener('keydown', function(e) {
    	const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    	const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    	if (!audio) return;
    
    	audio.currentTime = 0;
    	audio.play();
    	
    	key.classList.add('playing'); // add a class 'playing'
    
    	console.log(key);
    });

![Day%201%20JS%20Drum%20Kit/Untitled%206.png](Day%201%20JS%20Drum%20Kit/Untitled%206.png)

Note: a class name 'playing' has added

## Step 4: Add an event listener to detect when the transition ends on each `.key` elements to reset the style

Declare a variable that represent all `.key`s.

    const keys = document.querySelectorAll('.key');

![Day%201%20JS%20Drum%20Kit/Untitled%207.png](Day%201%20JS%20Drum%20Kit/Untitled%207.png)

Try `document.querySelectorAll('.key');` in the console > it returns an ***array***

Add an ***event listener*** for ***each*** `key`s

    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener());

- Note: why not `keys.addEventListener();`?

    The `addEventListener()` method is not working with an ***array***

Set an ***event type*** as `transitionend` to listen for

    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend'));

Set the ***listener*** by adding a ***callback*** function which will give us an ***event***(`e` stands for) when that ***event***(`transitionend`) detected

    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', function(e) {
    	// function will give us an event
    	console.log(e);
    	// test run
    }));

![Day%201%20JS%20Drum%20Kit/Untitled%208.png](Day%201%20JS%20Drum%20Kit/Untitled%208.png)

This returns all the `transitionend`s by different properties

Set the ***callback*** to deal with only one of them. In this case, `transform`

    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', function(e) {
    	if(e.propertyName !== 'transform') return;
    	// stop(skip) it if it isn't a `transform` property
    	console.log(e.propertyName);
    	// test to see which property the callback is dealing with
    }));

Remove a class name for style from the instance that called against it (in this case, the instance of the ***event target*** - `key`)

    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', function(e) {
    	this.classList.remove('playing');
    }));

Try `console.log(this)` to see what is called against the method. A cheap way to see what's equal to ***this***.

## Step 5: Detach callback functions

Detach all the ***callback*** functions from the ***event type*** and rearrange them

    window.addEventListener('keydown', function(e) {
      const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
      const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
      if (!audio) return;
      
      audio.currentTime = 0;
      audio.play();
    
      key.classList.add('playing');
    });
    
    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', function(e) {
      if (e.propertyName !== 'transform') return;
      this.classList.remove('playing');
    }));

    function playSound(e) {
      const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
      const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
      if (!audio) return;
      
      audio.currentTime = 0;
      audio.play();
    
      key.classList.add('playing');
    }
    
    function removeFeedback(e) {
      if (e.propertyName !== 'transform') return;
      this.classList.remove('playing');
    }
    
    const keys = document.querySelectorAll('.key');
    keys.forEach(key => key.addEventListener('transitionend', removeFeedback));
    
    window.addEventListener('keydown', playSound);
