To identify a child component in react or preact, they give you a type attribute that you can use to compare to the function reference itself
Example:
```tsx
<Father>
	<Child/>
	<Child2/>
</Father>
// how can you identify is the children components are Child or Child2
// well you can use this
export const Father = ({children})=>{
	const [child1, child2] = children; // children is a list of components in this case
	
	if(child1.type === Child){
		//some logic
	}
	
	if(child2.type === Child2){
		// some logic
	}
	
	// we compare type with the Function reference itself.
}

```
