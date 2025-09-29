---
tags:
  - Java
  - Spring-boot
---
## JFrame
`JFrame` class allow us create a basic windows to add our canvas.
```java
void main(){
	JFrame frame = new JFrame();
	frame.setSize(WIDTH, HEIGHT); // set default size
	frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // what to do when the user closes the windows (in some cases we don't want to close completely the process)
	frame.setTitle("My windows"); // set the top text in the window
	frame.setVisible(true); // show windows
}
```
# [[Canvas]]
# [[Events Jframe]]
# Main loop
To create a main loop we can use the canvas component
```java
// Canvas.java
class Canvas extends JComponent{
	@Override
	protected void painComponent(Graphics g){
		// here you can put any logic for the main loop
	}
}
// Main.java --> main()
Canvas canvas = new Canvas();
frame.add(canvas); // we need to attach the canvas
Timer timer = new Timer(delay, (e)->{
	canvas.repaint(); // update the current canvas (erase the current painted canvas)
});

timer.start();
```
