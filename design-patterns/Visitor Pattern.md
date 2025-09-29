This pattern give us the way to add new methods in a abstract class implementation without touching the existing classes.
In simple words we use this pattern when we need to add functionality in a set of object structures but we don't want to touch the existing **structure** classes.
Example
```java
abstract class Animal {
	abstract void walk();
	abstract void eat();
}

class Dog extends Animal{
	void walk(){}
	void eat(){}
}

class Cat extends Animal{
	void walk(){}
	void eat(){}
}

```
Now this is a common example, the problem is if we add  a new method in the abstract class we need to modify the Dog and Cat implementations
```java
abstract class Animal {
	abstract void walk();
	abstract void eat();
	abstract void jump();
}

class Dog extends Animal{
	void walk(){}
	void eat(){}
	void jump(){} // we add the new method in this class
}

class Cat extends Animal{
	void walk(){}
	void eat(){}
	void jump(){} // we add the new method in this class
}

```
Now this shouldn't be a problem but if we need to have the Dog and Cat structure intact but we need to add functionality we can apply the visitor pattern.
```java
abstract class Animal{
	abstract void accept(AnimalVisitor visitor); // we specify a method to receive a visitor in place of defining each method
}
// this represent our rows in a normal implementation
interface AnimalVisitor {
	void visitDog(Dog dog);
	void visitCat(Cat cat);
}

class Cat extends Animal{
	@Override
	void accept(AnimalVisitor visitor){
		visitor.visitCat(this); // we specify what visit method should use
	}
}

class Dog extends Animal{
	@Override
	void accept(AnimalVisitor visitor){
		visitor.visitDog(this); //we specify what visit method should use
	}
}

// now to add a new functionality we need to implement the visitor, this represent the columns in a normal implementation

class WalkVisitor implements AnimalVisitor{
	@Override
	void visitDog(Dog dog){
		// dog walk logic
	}
	
	@Override
	void visitCat(Cat cat){
		// cat walk logic
	}
}

// now how can you see we can add new methods without touching the structure classes
class JumpVisitor implements AnimalVisitor{
	@Override
	void visitDog(Dog dog){
		// dog jump logic
	}
	
	@Override
	void visitCat(Cat cat){
		// cat jump logic
	}
}
```
How we can use this:
```java
final WalkVisitor walk = new WalkVisitor();
final Dog dog = new Dog();
final Cat cat = new Cat();

dog.accept(walk); // execute dog walk method
cat.accept(walk); // execute cat walk method
```
If we need to add a new structure class (a row) we need to add it in the Visitor interface
```java
class Horse extends Animal{
	@Override
	void accept(AnimalVisitor visitor){
		visitor.visitHorse(this);
	}
}

interface AnimalVisitor {
	void visitDog(Dog dog);
	void visitCat(Cat cat);
	void visitHorse(Horse horse); // we add the visit horse
}

// now we need to implement the visit horse in each action that we create
class WalkVisitor implements AnimalVisitor{
	@Override
	void visitHorse(Horse horse){
		// horse walk logic
	}
}

class JumpVisitor implements AnimalVisitor{
	@Override
	void visitHorse(Horse horse){
		// horse jump logic
	}
}
```
Now how can you we need to touch the existing action classes, but we don't need to modify the Dog and Cat classes itself, so that's why this pattern is useful.