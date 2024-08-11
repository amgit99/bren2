This is all that I understand about this framework and all the things I feel are important to remember from the Udemy React Course.

React is a <span style="color:#e1db3d">frontend</span> JavaScript Framework, made for dynamic UIs. Its core idea is to define <span style="color:#e1db3d">elements of the DOM as components</span> and along with it, <span style="color:#e1db3d">some state(set of variables)</span> that it keeps track of, any change in the state triggers the replacement of the component thus imparting it its dynamic nature.

<span style="color:#e1db3d">Vite</span> is a tool that gives us templates for react projects, it has many other tools that make rendering fast, it has hot module replacement so that we can see the changes to our code in real time.
The `package.json` file holds the dependencies that are used in the project.

React introduces a new file format called `.jsx` which is essentially JavaScript with some HTML mixed in it, These files are later converted by React (jsx compiler) to the plain HTML/CSS/JS that a browser can understand and render.

Every component is a function, the name should have the first letter uppercase followed by lowercase letters. This is necessary to tell the React compiler that the HTML tag that is used is actually a react component and not a custom tag. If a component is called multiple times, even if the parameters are same the underlying function is called that many times.
<span style="color:#e1db3d">Styling</span> the components is also easy, one can save the CSS in a file with the name `{Appname}.module.css` and React will automatically give it a unique name and include it in the final css file. We can also do inline CSS by passing JS objects thats holds CSS attributes and their corresponding values.
```js
cosnt Post(props){ // inline CSS
	return (
		<div style = {{color: "red", ... }}>
			<Component />
			...
		</div>
	)
}

import classes from './Post.module.css'

//  if the className specified is present in the above imported file then
//  that styling is used as the default, before searching the global scope
//  for styles.

cosnt Post(props){
	return (
		<div className = 'post'}>
			<Component />
			...
		</div>
	)
}

/// in a css file we can have
.post{
	background_color: red,
}
```

<span style="color:#e1db3d">Props</span>: This is the name, by convention, given to an object that is received as an argument to the function that defines the component. This object can be composed of many fields that hold the actual parameters that one wishes to pass. The fields are passed in the component tag, not as a single object, but as a list of parameters, but the function receives a bundled up object made of those parameters.

<span style="color:#e1db3d">Event Listeners</span>: more about this in [[JavaScript]]. To handle them, functions are passed to the on{change/keydown...} prop of the component/html tag and functions that take an event object as a parameter handles the event.
<span style="color:#e1db3d">States</span>: This is the features that allows react to modify the DOM as a response to the actions performed by the user. The `useState` function takes in a parameter, the initial state of any variable we intend to track and the returns two things, a variable itself and a setter method for that variable, which takes in a new value that must replace the old one. When the setter is called the component function is called again and the DOM is rendered with the updated data.
<span style="color:#e1db3d">The Children Prop</span>: If we want some sort of effect, like a dimmed background, or a highlight effect, to happen whenever we hover or maybe click a component, we can create a wrapper component that wraps any component and then applies that effect to those components. 
```js
<Effect>
	<PostList />
	...
</Effect>

// in Effect.jsx file

export function Effect({childern}){
	return (
	<>
		... here we use {childern}
	</>
	)
}
```
The effect component has the job of having that highlighting effect on the components it encapsulates. But the effect component actually needs the components its going to apply the effect on. These are passed as default as "children" when we encapsulate any number of components with the tag.
<span style="color:#e1db3d">Lifting the State</span>: This problem occurs when a UI elements must be changed as a result of an event that occurs in a different component in a sibling subtree. So we save the state (data) of the component that changes(the descendant), not in the component itself but the common ancestor. The `useState` hook will give a setter that we wrap in a handler function and pass it to the component where the triggering event takes place. The component that changes gets the state values as usual via props.

The event can be captured at their Lowest Common Ancestor and then an Event Handler can be passed on as a prop all the way down the component argument chain to the component that must change.
One may send functions as props, these may have the responsibility of event handling.