---
layout: post
title:      "Simplifying Redux "
date:       2019-09-22 17:22:41 +0000
permalink:  simplifying_redux
---

 
 When learning Redux the first time on learn.co, I had a vague idea of each of the moving parts. State is our single source of truth for our application's state, and only way to acess it was through dispatch. 
 
 For my final blog post, I will use my project to crystalize the concepts and purpose of Redux. Before jumping into how to use it, let's discuss why we need it. The purpose of Redux is to have a single source of 'truth' for our state. State is where we will store user input, user information, any objects we create after fetching them etc. 
 
 As our application grows in complexity, storing state in different containers becomes more complex, and harder to track. With Redux, we never access the store directy. Instead we plug our presententional components into the store, and then dispatch actions that will eventually return an updated state. Then that state will me passed to our presentational components.
 
![](http://ibb.co/D1ypHY4)



