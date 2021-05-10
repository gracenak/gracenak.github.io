---
layout: post
title:      "Understanding Redux and Thunk"
date:       2021-05-05 05:32:28 -0400
permalink:  understanding_redux_and_thunk
---


I am at the end of my Flatiron School journey, concluding with the React module. The two middleware that are pertinent to understanding the wonders of React is Redux and thunk .
Redux is a predictable state container that helps manage state changes in a single store to exhibit consistent behavior on the client side. Redux allows us to avoid prop drilling by providing the ability to connect to store from the component.
To change state, we connect our store to the component, using the connect function to dispatch action creators. As soon as our action creator hits, dispatch gets called allowing us to return a function that calls dispatch and pass that action object straight to the reducer. The reducer is a function that updates the state in the store by matching the action type. It returns the updated state. The components connected to the store use mapStateToProps to access the current state from Redux as props. The parent containers then pass those props to the children containers, reflecting the updates on the DOM.
When Redux interacts with React, it virtually re-renders the DOM, then updates the DOM with minimal amount of changes required to ensure the performance of the app.
To configure store, we pass our reducer(a function that defines action types) in the createStore method which returns a store object. We pass this store object into the Provider component, the provided generic component through react-redux, rendered at the top level so that it can pass store to other components.
Lets take a look at an example:
This class component is connected to the store . We don’t need to access state from the store so we set the first argument to null. It takes in a second argument of dispatch to an action creator so that we can dispatch the action creator when the action creator function is invoked upon submission.

```
class ReviewInput extends Component {

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
                    <Field>
                        <input type="text" name="comment" placeholder="comment" value={this.state.comment} onChange={this.handleOnChange}/>
                    </Field>
                <RatingContainer>
                    <RatingLabel> Rate This Recipe</RatingLabel>
                    <Field>
                    <select name="rating" value={this.state.rating} onChange={this.handleOnChange}>
                        <option>5</option>
                        <option>4</option>
                        <option>3</option>
                        <option>2</option>
                        <option>1</option>
                    </select>
                    </Field>
                </RatingContainer>
                    <Field>
                        <input type="text" name="username" placeholder="Username" value={this.state.username} onChange={this.handleOnChange}/>
                    </Field>
                    <SubmitBtn type="submit">Create A Review</SubmitBtn>
                </form>
            </div>
        )
    }
}

export default connect(null, { addReview })(ReviewInput)

```

 

connect will return dispatch as a prop to (ReviewInput), connecting the Redux store to the component.
We hit the action creator, returning a function to call dispatch and invoking the fetch function to fetch data from the backend asynchronously. The fetch request returns a promise that we are waiting to be resolved. When the promise resolves, it returns a response converting to json and then allows us to call dispatch which takes in those action objects to be sent to the reducer.

```
export const addReview = (review) => {

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
                dispatch({type: 'ADD_REVIEW', payload: recipe})
            }
        })
    
    }
}

```


We hit the action creator, invoking the fetch function. The fetch request returns a promise that we are waiting to be resolved. When the promise resolves, it returns a response converting to json and then dispatching another action with the payload of the fetched data that gets sent to the reducer.

```
export default function manageRecipe(state = {recipes: []}, action) {
    switch(action.type) {
        case 'ADD_REVIEW':
            let recipes = state.recipes.map(recipe => {
                if (recipe.id === action.payload.id) {
                    return action.payload
                } else {
                    return recipe
                }
            })
            return {...state, recipes: recipes}
						        default:
            return state
    }
}
```
The reducer uses that action to make changes to the state so that the components can re-render with new data.

Thunk works in conjunction with Redux, handling asynchronous calls, incorporating async code in the Redux action creator. It allows us to return a function inside of our action creator. That function takes in the store’s dispatch function as an argument, allowing dispatch of multiple actions inside the returned function. Because it handles async calls, it allows us to take the time to call the dispatch, allowing the response request to finish before anything is dispatched to the reducer, unlike connect.
