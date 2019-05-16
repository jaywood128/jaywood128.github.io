---
layout: post
title:      "Improving UI with Join Models and Nested Forms"
date:       2019-05-16 17:55:08 +0000
permalink:  improving_ui_with_join_models_and_nested_forms
---


For my third project, I created a weightlifting app that allows users to track their progress from an intermediate weightlifter  to advanced. Most people struggle to get past intermediate due to poor planning and doing as many reps as possible for each exercise.

My first project was built using SInatra, a domain specific langauge written for Ruby, and user's would submit their workout routines on a single form_tag. Their were four `text_field_tag`s, so a user had to input all of their sets and reps in predefined fields. 

Enter the power of the nested form! Using the nested form, and the power of form_for, my user interface is visually more pleasing and makes more sense. As a user completes each set with their corresponding reps, then can input them. If they would like to start another workout_lift (bench press, squat, deadlift etc.), they can start a new one, and then come back to the other workout_lift when they need to.  A user simply clicks the 'Start Workout' button, and automatically start_time: DateTime.now is stored and displayed. A user then selects from each lift from a collection_selection. 

The block below is from a user's views/workout/workouts.html.erb page. This page iterates over all of their @workouts and each @workout's workout_lifts. A WorkOut includes a start_time: datetime and an end_time : datetime. A WorkoutLift is the join model that has an id column, workout_id, lift_id, and user_id. A Workout has_many :lifts, through: :workout_lifts and a workout_lift belongs to a workout_out. The Lift Model has an id and a name column. A Lift has_many workout_lifts and has many workouts through workout_lifts. 

```<h2> Add Exercise to Workout <h2> 
<%= form_for(@workout_lift) do |f| %> 
  <%= f.hidden_field :user_id, value: current_user.id %> 
  <%= f.**collection_select** :lift_id, Lift.all.reject{|l| @workout.lifts.include?(l)}, :id, :name %> 
    <%= f.hidden_field :workout_id, value: @workout.id %> 
    <%= f.**fields_for** :exercise_sets, @workout_lift.exercise_sets.build do |exercise_set| %> 
      <%= exercise_set.label "Weight" %> 
      <%= exercise_set.number_field :weight %> 
      <%= exercise_set.label "Reps" %> 
      <%= exercise_set.number_field :reps %> 
      <%= f.submit "Submit Weight and Reps" %> 
    <% end %><br> ```
		
A lot is happening here, so let's break it down. ```<%= f.collection_select :lift_id, Lift.all.reject{|l| @workout.lifts.include?(l)}, :id, :name %> ``` 

We are using a Rails Helper method, collection_select, to generate a <select> tag and a <option> tag. In this cast we want the user to be able to see a drop down of options. Since we are using a form_for, we don't need to specify the object -- the  |f| will do that for us. The method we would call on @workout_lift, because it's a Join, is lift_id. The workout_lift belongs to a lift, so by convention, the child will store the parent's _id in their table. 

**collection_select(object, method, collection, value_method, text_method, options = {}, html_options = {})

[Collection Select Documentation] (https://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/collection_select) 

Finally, we have our fields_for method. This method allows us to submit an associated model of @workout_lift. So @workout_lift.exercise_sets is valid because an exercise_set belongs to a workout_lift. The HARDEST part about using a nested form is updating your strong params. Mine ended up looking like this: 

```def params_workout_lifts 
    params.require(:workout_lift).permit(:user_id, :lift_id, :lift, :workout_id, exercise_sets_attributes: [:id, :weight, :reps])
  end ```
	
	To figure out what yours will look like, I recomend submitting your form and place a pry in your form's #create action inside the corresponding controller. In this case it's the #create action inside the WorkoutLift controller. 
	


