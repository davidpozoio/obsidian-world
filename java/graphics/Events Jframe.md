## MouseMotionListener
To attach an event listener we can do this, this event will work for the Jframe or JComponent context, not globally.
```java
frame.addMouseMotionListener(new MouseMotionAdapter(){
	// we can override different types of events for a mouse	
});
```
### MovedMouse
This method allow us to get the current mouse properties when we enter our cursor inside the jframe or jcomponent
```java
frame.addMouseMotionListener(new MouseMotionAdapter(){
	@Override
	public void movedMouse(MouseEvent e){
		System.out.println(e.getX() + " " + e.getY());
	}
});
```
### DraggedMouse
This method catch the properties of the cursor when we keep the click inside the jframe or jcomponent.
```java
frame.addMouseMotionListener(new MouseMotionAdapter(){
	@Override
	public void draggedMouse(MouseEvent e){
		System.out.println(e.getX() + e.getY());
	}
});
```
