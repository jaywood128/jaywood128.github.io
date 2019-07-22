---
layout: post
title:      "addEventListener and Function Scope in JavaScript  "
date:       2019-07-14 16:41:52 -0400
permalink:  addeventlistener_and_function_scope_in_javascript
---


Adding dynamic features to my Ruby on Rails weightlifting application was made possible to incorporating JavaScript and a JSON API. Without really planning it, I ended up using 'VanilaJs' - this is a hip way of saying no additional libraries ,like JQuery, were used. So instead of using `$()` to select elements, I used document.getElementById() and  the Fetch API to request resources that could be rendered asynchronously -- without refreshing the page. 

When adding my first event listener to a form, I had some issues with making sure my HTML elements were loaded so the EventListeners could be added to a thing that was loaded and not to a think that was null. My workoutLifts.js file, my main JS file for my project, looked something like this: 
```
`document.addEventListener('turbolinks:load', (e) => {
    
    
    var el = document.getElementById('new_exercise_set');

    
      el.addEventListener('submit', e => {
        e.preventDefault()
    
```

Adding this to my form resulted in an Uncaught Type Error: Cannot read property 'addEventListener' of null.  This basically means that the receiver of the method call addEventListener has no value when the listener is being attached. My work around for this was to use conditional logic to check to see if `el` has a truthy value. I simply wrapped the addEventListener with an  if (el) { } . 

Another issue that caused me a lot of wheel spinning was not having access to my onclick callback functions inside my addEventListener. 

When a user logs in, they see a list of all their workouts by start_time and date. On the workout #show page, the workout's workout_lifts are rendered using JSON and attached to some containers as JavaScript objects. Each WorkoutLift represents something like a Barebell Squat or a Bench press. The WorkoutLift model consturctor had a prototype method that called render, which is called to create a button that invokes showExerciseSets when clicked: 

```
render() { 
   return 
      <div id="Workout_${this.id}"> ${this.name} </div>
      <button onclick="showExerciseSets(${this.id})" id ="exerciseSetIndex" data-workout_lift_id= "${this.id}"> Show sets </button> 

  }
```

	
From here, I had issues rendering a WorkoutLift's exercise_sets in my <ul> on the page after making the fetch request and creating the JavaScript objects. Here's the error message that will forever haunt my dreams:
	
19:1 Uncaught ReferenceError: showExerciseSets is not defined
	at HTMLButtonElement.onclick (19:1)

		
The issue was ultimately WHERE I was defining the showExerciseSets function. It was defined inside the turbolinks:load addEventlistener --which doesn't have Global Scope. That is, I was defining ShowExerciseSets inside the body of a callback function inside the event listener. Obviously, you shouldn't declare a function inside of another function, if you need to call that function outside of a function, but it took me awhile to catch my error. 
		
Any variable, or function declared outside the turbolinks addEventListener belongs within the Global Scope. Additioanlly, when the Show Sets button was clicked, the showExerciseSets function was not accesible to the Window. According the the MDN, The Window is an interface that represents a window containing a DOM document; the document property points to the DOM itself. If your function is defined inside the body of another addEventListner, it will not be accesibled when the HTML button calls the method inside the DOM. So by simply placing all my functions in my js file for onclick attributes OUTSIDE the addEventListener, I was able to have access to all my functions and get rid of my Uncaught ReferenceError. To test where the global scope is, add a console.log somewhere in your code and see if it gets logged to your console with ( cmd + opt + j). 
		
If you are confused about what the Window is, think of it like a container that points to the document. Without that document, you wouldn't be able to select HTML elements.  
		
	
The showExerciseSets is now accesible! Becareful where you declare your functions when writing JS and be sure to use addEventListeners to elements that aren't loaded asynchronously. Another option for undefined methods is to add them inside a <script> tag on the page in which you are calling the methods. 

As challenging as it was to work through that error, it reinforced the concept of how to make something accesible in Global Scope and how function scope can prevent variables and methods from being accesible. 

Resources: 

[Cannot Read Property of addEventListener - Stack Overflow](http://stackoverflow.com/questions/26107125/cannot-read-property-addeventlistener-of-null)

[MDN Window Documentation](http://developer.mozilla.org/en-US/docs/Web/API/Window)

[Javascript variable and function scope](http://https://sitepoint.com/demystifying-javascript-variable-scope-hoisting/)
	

