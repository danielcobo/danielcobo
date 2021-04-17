## Use custom CSS for image input

<a href="#solution">Skip to solution</a>.

## Problem

HTML supports image input and on mobile it will even do all the heavy lifting for the Camera API for you, providing a nice native experience. Here's what it looks like:

%[https://codepen.io/DanielCobo/pen/vYgZKxm]

But let's be honest, the design sucks. Why?

- It's too basic and kinda ugly (yes, tastes differ, but really? you like this?) 
- It's wildly inconsistent across web browsers and operating systems. 
- You __can't style it with CSS__.

I'd be OK if you could just add your own CSS, but it will only let you style the "box". The button, the message, etc. are all untouchable by CSS. It would have been so much better if they just let you manipulate those via  [pseudo-elements][1] .

<h2 id="solution">Solution</h2>

The solution to the problem is done in 3 steps:
1. Wrap the `<input>` in a `<label>` element
2. Hide the `<input>` element
3. Style the label as a button (Note: label text becomes the button text)

%[https://codepen.io/DanielCobo/pen/gOgRLLa]

## Next steps
The solution isn't perfect, but you can __solve both drawbacks mentioned below with some JavaScript__. If you have an interesting implementation or even better, if you came up with fixes that don't require JS, don't be shy and drop me a line.

### Make it keyboard accessible
Adding `tabindex="0"` to the label element will make it _tab-bable_, but pressing `ENTER`/`SPACE` doesn't activate it. What you could do is add a JS listener for the keypress. This defeats the purpose which is a _CSS only_ solution. 

However, if we were to use JS anyway we might as well be more semantic. Keep in mind clicking this control will activate the hidden input, so using a button would be much more semantic than a label element. 

### Add visual feedback for input value
Often it is useful to see if/which image is in the input. The HTML solution is to provide you with a filename or text saying that the value is empty. 

Again, we could solve this with some JS and listen to the value change. In this case I would also consider adding an image preview and a button to reset the input (set value to none). Using the built-in HTML input you can reset the input value too, but it's done in such an unintuitive way, doing a JS powered redesign is the way to go. 

[1]: https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements "CSS pseudo-elements"
