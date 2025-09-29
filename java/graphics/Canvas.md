To create a "canvas" or a base component to start adding new components and print their graphics we can do this
```java
// Canvas.java
class Canvas extends JComponent{
	@Override
	protected void painComponent(Graphics g){
		
	}
}
```
## Rectangles
To create a rectangle we can use this.
```java
// Canvas.java
@Override
protected void painComponent(Graphics g){
	Graphics2D graphics = (Graphics2D)g; // we cast Graphics to Graphics2D to have more functions
	Rectangle2D.Double rectangle = new Rectangle2D.Double(x, y, width, height);
	
	// we use this to create a filled rectangle with the specified dimensions
	graphics.setColor(Color.BLACK); // we set the color of the pre figure
	graphics.fill(rectangle);
	
	Rectangle2D.Double rectangle2 = new Rectangle2D.Double(x, y, width, height);
	graphics.setColor(Color.BLUE);
	graphics.draw(rectangle2); // this draw only the outline not fill the figure
}
```

