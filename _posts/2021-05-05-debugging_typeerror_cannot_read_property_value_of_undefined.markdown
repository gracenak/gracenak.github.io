---
layout: post
title:      "Debugging ‘TypeError: Cannot read property ‘value’ of undefined’"
date:       2021-05-05 09:25:27 +0000
permalink:  debugging_typeerror_cannot_read_property_value_of_undefined
---


This error message was the bane of my existence during my final Flatiron School React project build but helped me better understand lifecycle components in React.
Before delving further into this error message, it is important to understand state and props. State is an instance of an object in a component. It stores properties and values that belong to the component that can change over the lifetime of a component. It can only be stored in a class component. State can be mutated, providing ways to maintain and update info within a component without requiring the parent to send updated info. Thats where props come into play. Props allow us to pass data from a parent component to a child component. They help make components more dynamic and reusable.
State is initialized in the constructor, being the first method to run followed by render(). We update state in setState(), which runs asynchronously. We use props to send a callback function. This allows the callback to be part of a different component than the one that invoked the callback. After invocation, the callback can send or mutate data in the parent component.
So let’s review, when a parent component wants to send data to the child component, they pass props to their children. When the the child wants to send data to the parent, we invoke callbacks that were passed down by the parents to the children as props.
Now, let’s talk lifecycle components. There are lifecycle hooks and 3 lifecycle methods called 1)Before being created (mount), 2)After being created (update), 3)About to be deleted (unmount).
The 2 lifecycle hooks available are 1) static getDerivedStateFromProps and 2) componentDidMount. After the constructor is called, static getDerivedStateFromProps gets called, giving us access to any props and state which can be modified. Then render() gets called returning the state and returning JSX for React to insert into the DOM. Then componentDidMount is called after the first render, used to perform DOM manipulation or data fetching.
‘TypeError: Cannot read property ‘value’ of undefined’ is a vague error message that is overwhelming. I realized that it was generally telling me that even though I thought my object WAS defined, it was probably returning that error based on the first call where the initial state was undefined. Or perhaps I had forgotten to pass down props from a parent. Or that the prop that I passed down with an initial undefined or null needed to be defined in a ternary statement to retrieve the defined props. Or maybe it was a prop coming from store in Redux but was missing the value.
Using ‘&&’ syntax saved me a lot of headache after going around in circles for awhile.
```{this.props.recipe && this.props.recipe.id}
If it is in fact this object and it exists, return this object and its id.
This error message will continue to be a part of my life but I have a better grasp on reading and understanding the error message, giving me some ideas on where to retrace my steps and start debugging.
