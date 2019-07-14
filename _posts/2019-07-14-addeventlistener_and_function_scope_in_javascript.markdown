---
layout: post
title:      "addEventListener and Function Scope in JavaScript  "
date:       2019-07-14 20:41:51 +0000
permalink:  addeventlistener_and_function_scope_in_javascript
---


For my fourth FlatIron Project, I added dynamic features to my Ruby on Rails weightlifting application. I did this by incorporating JavaScript and a JSON API. WIthout really planning it, I ended up using 'VanilaJs' in my project. This is a hip way of saying that I didn't use any additional libraries like JQuery. So instead of using `$()` to select elements, I used `document.getElementById()` and  the Fetch API to request resources that could be rendered asynchronously -- without refrehsing the page. 

When adding my first event listener to a form, I had some issues with making sure my HTML elements were loaded so the EventListeners could be added to a thing that was loaded and not to a think that was null. My workoutLifts.js file, my main JS file for my project, looked something like this: 
`document.addEventListener('turbolinks:load', (e) => {
    
    
    var el = document.getElementById('new_exercise_set');

    
      el.addEventListener('submit', e => {
        e.preventDefault()
    
`
Adding this to my form resulted in an Uncaught Type Error: Cannot read property 'addEventListener' of null.  This basically means that the receiver of the method call addEventListener has no value when the listener is being attached. My work around for this was to use conditional logic to check to see if `el` had loaded, ie. did it have a truthy value. I simply wrapped the addEventListener with an ` if (el) { } `. 

Another issue that caused me a lot of wheel spinning was not having access to my onclick callback functions inside my addEventListener. 

When a user logs in, they see a list of all their workouts by start_time and date. On the workout #show page. the workout's workout_lifts are rendered using JSON and attached as JavaScript objects. Each WorkoutLift represents something like a Barebell Squat or a Bench press. The WorkoutLift model consturctor had a prototype method that called render: 
```
render() { 
   return `
      <div id="Workout_${this.id}"> ${this.name} </div>
      <button onclick="showExerciseSets(${this.id})" id ="exerciseSetIndex" data-workout_lift_id= "${this.id}"> Show sets </button> 

      ```
  }

	
	From here, I had lots of issues rendering a WorkoutLift's exercise_sets. The error message that haunted my dreams was: `19:1 Uncaught ReferenceError: showExerciseSets is not defined
    at HTMLButtonElement.onclick (19:1)` 
		The issue was ultimately WHERE I was defining the showExerciseSets. I was defining it inside the turbolinks:load addEventlistener at the top of my code. That is, I was defining ShowExerciseSets inside the body of a callback function inside the event listener.
		Any variable or function declared outside the turbolinks addEventListener belongs to the global scope. So by simply placing all my functions for onclick attributes OUTSIDE this I was able to have access to all my functions. 
		Therefore, when the Show Sets button was clicked, the showExerciseSets function was not accesible to the Window. According the the MDN, The Window is an interface that represents a window containing a DOM document; the document property points to the DOM itself. If your function is defined inside the body of another addEventListner, it will not be accesibled when the HTML button calls the method inside the DOM. If you are confused about what the Window is, think of it like a container that points to the document. Without that document, you wouldn't be able to select HTML elements.  Here's a sample of what my final working code: 
		
		```console.log("Loading??")
  document.addEventListener('turbolinks:load', (e) => {
    
    
    var el = document.getElementById('new_exercise_set');

    if(el){
      el.addEventListener('submit', e => {
        e.preventDefault()
    
        var workout_lift_id = document.querySelector("form #exercise_set_workout_lift_id").value 
        var token = e.target.querySelector('input[name=authenticity_token').value 
        var data = {exercise_set: {}}; 
        
        data["exercise_set"]["weight"] = e.target.querySelector("#exercise_set_weight").value 
        data["exercise_set"]["reps"] = e.target.querySelector("#exercise_set_reps").value 
        var exercise_set_url = `http://localhost:3000/workout_lifts/${workout_lift_id}/exercise_sets`
        
        fetch(`${exercise_set_url}`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json', 
            'Accept' : 'application/json',
            'X-CSRF-token': token
          }, 
          body: JSON.stringify(data)
        })
        .then(resp=> resp.json())
        .then(exercise_set => {
          displayCreatedExerciseSets(exercise_set)
        })
        .catch(e => {
          console.log(e);
          return e;
        });

      })
    }

  const workoutShow = document.querySelector(".workouts.show")
    
  if (workoutShow) { 
    getWorkout(e.data.url)
  } 
})
function showExerciseSets(id) {
  console.log(id)
  
  fetch(`http:localhost:3000/workout_lifts/${id}.json`)
   .then( resp=> resp.json())
  .then( get_exercise_set => showExerciseSetIndex(get_exercise_set))
  .catch( err => console.log(err))
}
```

The showExerciseSets is now accesible! Becareful where you declare your functions when writing JS. Another option for undefined methods is to add them inside a <script> tag on the page in which you are calling the methods. As challenging as it was to work through that error, it reinforced the concept of how to make something accesible in Global Scope and how function scope can prevent variables and methods from being accesible. 

Resources: 

[](http://stackoverflow.com/questions/26107125/cannot-read-property-addeventlistener-of-null)

[](http://developer.mozilla.org/en-US/docs/Web/API/Window)

[](http://https://sitepoint.com/demystifying-javascript-variable-scope-hoisting/)
	

