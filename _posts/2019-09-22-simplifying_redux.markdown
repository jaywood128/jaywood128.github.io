---
layout: post
title:      "Hooking up forms with redux  "
date:       2019-09-22 13:22:41 -0400
permalink:  simplifying_redux
---

 
 For my final blog post, I wanted to use my React project to crystalize the concepts and purpose of Redux. When learning Redux the first time on learn.co, I had a vague idea of what Redux does: it acts as a single centralized source of truth for our application's state and we update it by dispatching actions. 

This is only meant as a brief overview of how your components are connected to the store and one could begin to connect a form using Redux but it's important to understand the purpose of Redux before you begin to use it. 
 
 For my final blog post, I will use my project to crystalize the concepts and purpose of Redux. 

Before jumping into how to use it, let's discuss the 'why'. Having a single source of truth makes for easier state management, especially as our application grows in complexity, storing state in different containers is not optimal. With Redux, we never access the store directy. Instead we plug our presententional components into the store, and then dispatch actions that will eventually return an updated state. Then that state will me passed to our presentational components.
 
![](http://i.imgur.com/JGhwjSU.png)

1) First import the Provider, createStore, and your rootReducer. Be sure to import compose and applyMiddleware to like to use the develper extension. This is *really* important tool for being able to check if your Redux store's state has updated. The line is also important because we are assignning the value of store to rootReducer -- this combines our reducers. 
*
2) Be sure set your store equal to createStore. The store will then be passed as a JSX prop to your App so that you have access to is. 

Inside my React app, there is a SearchByDateContainer and a SearchByDate component. The container will end up being connected to the store. User's will submit a date to search NASA's Astronomy Picture of the Day API. That date will then be passed to fetchSearchedPicture. The container is responsible for fetching, so fetchSearchedPicture should be passed as a prop to SearchByDate so that it's triggered when the SearchByDate form is submitted. 


## Inside your Container component 

![](http://i.imgur.com/B1McOaG.png)

Inside my React app, there is a SearchByDateContainer and a SearchByDate component. The container will end up being connected to the store. User's will submit a date to search via the form to NASA's Astronomy Picture of the Day API. That date will then be passed to fetchSearchedPicture. The container is responsible for fetching, so fetchSearchedPicture should be passed as a prop to SearchByDate so that it's triggered when the SearchByDate form is submitted. 


Inorder to connect the store, we have to use the aptly named connect method -- `import { connect } from 'react-redux'`.

Since we will never directly access the store, we use mapDispatchToProps to name our dispatched actions and to mapStateToProps.It's important to note that whatever you name on this line inside your mapDispatchToProps, that is how you will call the action.
```
fetchSearchedPicture: (date)=> dispatch(fetchSearchedPicture(date)), 
```
So inorder for me to pass this action to my form I can simply pass fetchedSearchPicture={this.props.fetchSearchedPicture} to my SearchByDate component. 

mapStatetoProps is as simple as it sounds. You are directly accessing your store's state. Any time there are updates to the store, and mapStateToProps *is provided as an argument*, your wrapper component will update too. Now your container has access to that state with this.props.value. 





