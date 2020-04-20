# Day 2 - CSS + JS Clock

Created: Apr 14, 2020 4:38 PM
Reference: https://github.com/nitishdayal/JavaScript30/tree/master/exercises/02%20-%20JS%20%2B%20CSS%20Clock
Source: https://courses.wesbos.com/account/access/5e8f3e249edbdf363811ca25/view/194130581

# Goal Overview

An HTML page displays a collection of `div` elements which represent clock hands of hour, minute, and second.

It takes in the current time form the Javascript and manipulate the CSS value of  `transform(rotate)` to move them accordingly.

# References

- ***transform-origin*** [https://developer.mozilla.org/en-US/docs/Web/CSS/transform-origin](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-origin)
- ***transition-timing-function*** [https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function#Values](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function#Values)
    - cubic-bezier(p1, p2, p3, p4)
- ***Date() vs new Date() in JavaScript*** [https://stackoverflow.com/questions/9584719/date-vs-new-date-in-javascript](https://stackoverflow.com/questions/9584719/date-vs-new-date-in-javascript)
- ***Date instance & instance method*** [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#Date_instances](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#Date_instances)
- ***ElementCSSInlineStyle.style*** [https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style)

# Process

Position and transition of clock hands

- Edit CSS to align all of the hands.
- Set the pivot point of them by using `transform-origin`
- Add an animation effects

Get time and apply on position of clock hands

- Get current time(hour, minute, and second separately) and convert them into degree to manipulate the position every second
    1. Have a ***function*** that can do such work
        - Get the current time and assign to an instance
        - Get second, minute, and hour from the time instance
    2. Run the ***function*** every second by using `setInterval()`
- Note: Be mindful of the starting degree of the hand which sets by CSS as the base position (by original solution: `rotate(90degree)`)
- Better to have 3 variables to get the hour, minute, and second hands `div`

## Step 1: Set the starting position of all the clock hands

By default, the HTML has 3 thin-horizontal `div` with the `absolute` position which all are laying flat. Change this to thin-vertical `div` to avoid adding the default angle to calculate the degree of clock hands. Also, utilize `grid` layout for better stability of CSS rendering.

Edit the CSS ***positioning*** to use `grid-template-areas` layout and remove unnecessary properties.

    .clock-face {
      position: relative;
      width: 100%;
      height: 100%;
      transform: translateY(-3px); /* account for the height of the clock hands */
    }
    
    /* ----- modification ----- */
    
    .clock-face {
      display: grid;
    	grid-template-areas: "clock-face";
    	height: 100%;
    }

Set the horizontal layout of item by adding `justify-items`

    .clock-face {
      display: grid;
    	grid-template-areas: "clock-face";
    	height: 100%;
    	justify-items: center;
    }

Set the `grid-area` as the value of `grid-template-areas` of parent `div`. And add some ***modifiers*** for different sizes and colors.

    .hand {
      width: 50%;
      height: 6px;
      background: black;
      position: absolute;
      top: 50%;
    }
    
    /* ----- modification ----- */
    
    .hand {
      background-color: black;
    	grid-area: clock-face;
      height: 50%;
    }
    
    .hand--hr {
    	box-shadow: inset 0 50px 0 #f0f0f0;
    	width: 14px;
    }
    
    .hand--min {
      width: 6px;
    }
    
    .hand--sec {
    	background-color: red;
    	width: 2px;
    }

## Step 2: Add the pivot point and an animated movement

Set the pivot point by adding `transition-origin` in the CSS

    .hand {
      background-color: black;
    	grid-area: clock-face;
      height: 50%;
    	transform-origin: 50% 100%;
    }

Add `transform` value for an animated movement. Consider to set `transition-timing-function` as a `cubic-bezier` value for bouncy movement of the hands.

    .hand {
      background-color: black;
    	grid-area: clock-face;
      height: 50%;
    	transform-origin: 50% 100%;
    	transition: all 0.05s;
    	transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);
    }

## Step 3: Provide and fire a function to capture all the ticking

Create an empty ***function***. Give the relevant name.

    function getTime() {
    	// do something
    	console.log('I get time');
    }
    
    getTime(); // test run

![Day%202%20CSS%20JS%20Clock/Untitled.png](Day%202%20CSS%20JS%20Clock/Untitled.png)

Use `setInterval()` to run the ***function*** every second.

    function getTime() {
    	// do something
    	console.log('I get time');
    }
    
    setInterval(getTime, 1000); // 1000 milliseconds === 1 second

![Day%202%20CSS%20JS%20Clock/Untitled%201.png](Day%202%20CSS%20JS%20Clock/Untitled%201.png)

Note that it keeps counting the logs

## Step 4: Get current time and breakdown to hour, minute, and second

Start to build the ***function*** by adding a ***variable*** that's assigned for current time. Use `new Date()` to get the current date first.

    function getTime() {
    	// Get current date and assign to the variable
    	const now = new Date();
    }
    
    setInterval(getTime, 1000);

![Day%202%20CSS%20JS%20Clock/Untitled%202.png](Day%202%20CSS%20JS%20Clock/Untitled%202.png)

Try `console.log(now)` to see if this works accordingly

Get the current ***second*** from `now` and assign to a ***variable***. Use `getSecond()` ***getter instance method***

    function getTime() {
    	const now = new Date();
    	// Get the current second and assign
    	const second = now.getSeconds();
    }
    
    setInterval(getTime, 1000);

![Day%202%20CSS%20JS%20Clock/Untitled%203.png](Day%202%20CSS%20JS%20Clock/Untitled%203.png)

Try `console.log(second)` to see if this is working as expected

Add ***variables*** for ***minutes*** and ***hour*** as well.

    function getTime() {
    	const now = new Date();
    
    	const second = now.getSeconds();
    	const minute = now.getMinutes();
    	const hour = now.getHours();
    }
    
    setInterval(getTime, 1000);

![Day%202%20CSS%20JS%20Clock/Untitled%204.png](Day%202%20CSS%20JS%20Clock/Untitled%204.png)

Try `console.log(hour + ':' + minute + ':' + second)`

## Step 5: Convert the time into the rotation angle

Starting with the second, calculate the degree

Note: the full circle of the second(= 60 seconds)s is 360 degree. Divide the current numerical value of the clock hand by it's max possible value to get the rotation as a percentage, then multiply the result of that by 360 (each hand can rotate 360 degrees) to convert the value from a percentage to an integer.

    function getTime() {
    	const now = new Date();
    
    	const seconds = now.getSeconds();
    	const minutes = now.getMinutes();
    	const hours = now.getHours();
    
    	const secDegrees = (seconds / 60) * 360; // Seconds to rotate angle
    }
    
    setInterval(getTime, 1000);

![Day%202%20CSS%20JS%20Clock/Untitled%205.png](Day%202%20CSS%20JS%20Clock/Untitled%205.png)

Check with `console.log(secDegrees);` to see if it's calculated by 6 degree for every seconds til 360 degree.

Add degree calculation for minutes and hours

    function getTime() {
    	const now = new Date();
    
    	const seconds = now.getSeconds();
    	const minutes = now.getMinutes();
    	const hours = now.getHours();
    	
    	const secDegrees = (seconds / 60) * 360; // Seconds to rotate angle
    	const minDegrees = (minutes / 60) * 360; // Minutes to rotate angle
    	const hrDegrees = (hours / 12) * 360; // Hours to rotate angle
    }
    
    setInterval(getTime, 1000);

## Step 6: Get the each of `hand`s elements and manipulate the CSS

Get the each clock hands by using `querySelector()` and class names. Since these are the target of the ***function***, these can be declared outside of it.

    // Outside of the function
    const secHand = document.querySelector('.hand--sec');
    const minHand = document.querySelector('.hand--min');
    const hrHand = document.querySelector('.hand--hr');

![Day%202%20CSS%20JS%20Clock/Untitled%206.png](Day%202%20CSS%20JS%20Clock/Untitled%206.png)

`console.log(secHand, minHand, hrHand);`

Get into the function and add a line that can access to `style` of each elements to manipulate `transform(rotate)` value.

Note: use ES6 string template `${...}`. To use that, change `"` to backticks instead.

    function getTime() {
    	const now = new Date();
    
    	const seconds = now.getSeconds();
    	const minutes = now.getMinutes();
    	const hours = now.getHours();
    	
    	const secDegrees = (seconds / 60) * 360;
    	const minDegrees = (minutes / 60) * 360;
    	const hrDegrees = (hours / 12) * 360;
    
    	secHand.style.transform = `rotate(${secDegrees}deg)`;
    }
    
    setInterval(getTime, 1000);
