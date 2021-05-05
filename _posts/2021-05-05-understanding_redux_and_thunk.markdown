---
layout: post
title:      "Understanding Redux and Thunk"
date:       2021-05-05 05:32:28 -0400
permalink:  understanding_redux_and_thunk
---


I am at the end of my Flatiron School journey, concluding with the React module. The two middleware agendas that are pertinent to understanding the wonders of React is Redux and thunk .
Redux is a predictable state container that helps manage state changes in a single store so that it exhibits consistent behavior. To change state, we:
1. Update the state in Redux by creating an action with a type key.
2. Pass that action as an argument when we call the reduce
3. The reducer produces a new state object. (Note: It does not update the previous state)

When Redux interacts with React, it virtually re-renders the DOM, then updates the DOM with minimal amount of changes required to ensure the performance of the app.
To configure store, we pass our reducer(a function that defines action types) in the createStore method which returns a store object. We pass this store object into the Provider component which is the provided generic component through react-redux, rendered at the top level so that it can pass store to other components.
Lets take a look at an example:
This class component is connected to the store . Whatever is currently stored in the state will be sent off to the reducer through the dispatched action

```class ReviewInput extends Component {
     state = {
        comment: '',
        rating: '5',
        username: '',
        recipe_id: this.props.recipeId
}
.
.
.
handleOnSubmit = (event) => {
  event.preventDefault()
  this.props.addReview(this.state, this.props.recipe.id)
  this.setState({
     comment: '',
     rating: '5',
     username: ''
    })
}
render() {
  return(
     <div>
       <form onSubmit={this.handleOnSubmit}>
.
.
.
.
export default connect(null, { addReview })(ReviewInput)```

connect will return dispatch as a prop to (ReviewInput). It’s a way of connecting that redux store to the component.

```export const addReview = (review) => {
     return (dispatch) => {                   
		 fetch(`http://localhost:3000/api/v1/recipes/${review.recipe_id}/reviews`, {
      method: 'POST',
      headers: {
         'Content-Type': 'application/json'
       },
      body: JSON.stringify(review)
    })
      .then(response => response.json())
      .then(recipe => {
      if (recipe.error) {
         alert(recipe.error)
      } else {
         dispatch({type: 'ADD_REVIEW', payload: recipe}
      }
    })
  }
}```

We hit the action creator, invoking the fetch function. The fetch request returns a promise that we are waiting to be resolved. When the promise resolves, it returns a response converting to json and then dispatching another action with the payload of the fetched data that gets sent to the reducer.

```export default function manageRecipe(state = {recipes: []}, action) {
   switch(action.type) {
      case "ADD_RECIPE":
         return {
            ...state, recipes: [...state.recipes, action.payload]
     }
     default:
       return state
     }
}```

The reducer uses that action to make changes to the state so that the components can re-render with new data.
Thunk works in conjunction with Redux, handling asynchronous calls, incorporating async code in the Redux action creator. It allows us to return a function inside of our action creator. That function takes in the store’s dispatch function as an argument, allowing dispatch of multiple actions inside the returned function. Because it handles async calls, it allows us to take the time to call the dispatch action creator, allowing the response request to finish before anything is dispatched to the reducer, unlike connect.
