# S.O.L.I.D Principles of Object Oriented Programming

1. [Single Responsibility Principle](#single_responsibility_principle)
2. [Open Closed Principle](#open_closed_principle)
3. [Liskov Substitution Principle](#liskov_substitution_principle)
4. [Interface Segregation Principle](#interface_segregation_principle)
5. [Dependency Inversion Principle](#dependency_inversion_principle)

## Single Responsibility Principle

> Single Responsibility Principle => SRP

### The Definition

> A class should have a single responsibility. A class should have only one reason to change.

In other words, the complicated classes (modules) should be divided into smaller classes and each small class only has a particular behavior. It will easier to understand and maintain your code.

### Example

**Bad**
```java
class Rect {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }    
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
    
    public void render() { ... } // Violation of the SRP
}
```

The class **Rect** looks quite correctly. However, it handles two tasks: *init and render*. In the future, we can expand target of *render* function to Desktop GUI, Game, Canvas, ...

Following the SRP principle, we should divide the **Rect** class into two classes.

**Better**
```java
class Rect {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }    
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
}

class GraphicsRender {
    public void renderRect(Rect rect) { ... }
}
```

### Conclusion
* Keep your code simple.
* Debugging would be easier.
* Easier to upgrade and expand.

## Open Closed Principle

> Open Closed Principle => OCP

### The Definition

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

Respecting OCP, will mostly respect SRP also.

The open–closed principle helps developers achieve a flexible system architecture.

### Example

**Bad**

Continue with Better code example in the first principle, we must update Graphics class when new Circle class is added.

```java
class Rect {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }    
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
}

class Circle {
    private int radius;
    
    public void setRadius(int radius) { ... }    
    public int getRadius() { ... }
}

class GraphicsRender {
    public void renderRect(Rect rect) { ... }
    public void renderCircle(Circle circle) { ... }
}
```

The *GraphicsRender* class will become big class when we have a lot of type shapes. So, how to *GraphicsRender* is not changed when a new shape class is added?

In better code, we use abstract render() method in *GraphicsRender* class for drawing objects, while moving the implementation in the concrete shape objects.

```java
class Shape {
    abstract void render();
}

class Rect extends Shape {
    ...
    
    public void render() {
        // render the rectangle
    }
}

class Circle extends Shape {
    ...
    
    public void render() {
        // render the circle
    }
}

class GraphicsRender {
    public void renderShape(Shape s) {
        s.render();
    }
}
```

In example above, Rect and Circle class violate SRP because each class has more one reason to change. So, the better code would be:

**Better**
```java
class ShapeRender {
    abstract void render();
}

class Rect {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }    
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
}

class RectRender extends ShapeRender {
    private Rect rect;
    
    public void render() {
        // render the rectangle
    }
}

class Circle {
    private int radius;
    
    public void setRadius(int radius) { ... }    
    public int getRadius() { ... }
}

class CircleRender extends ShapeRender {    
    private Circle circle;
    
    public void render() {
        // render the circle
    }
}

class GraphicsRender {
    public void renderShape(ShapeRender s) {
        s.render();
    }
}
```

### Conclusion

OCP will save you a lot of time and effort.

OCP should be applied in those area which are most likely to be changed.

There are many design patterns that help us to extend code without changing it:
* Decorator pattern help us to follow Open Close principle. 
* Factory Method or the Observer pattern might be used to design an application easy to change with minimum changes in the existing code.

## Liskov Substitution Principle

> Liskov Substitution Principle => LSP

### The Definition

The Liskov substitution principle, written by Barbara Liskov in 1988.

> Child classes should never break the parent class' type definitions.

> Let q(x) be a property provable about objects x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.

### Example

In geometry, a *square* is a particular form of *rectangle*. So we could try to implement a Square class that extends a Rectangle class.

**Bad**
```java
class Rect {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }    
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
    
    public int getArea() { return width * height; }
}

class Square extends Rectangle {
    public function setWidth(int value) {
        this.width = value;
        this.height = value;
    }
    
    public function setHeight(int value) {
        this.width = value;
        this.height = value;
    }   
}
...
Rectangle rect = new Rectangle();
rect.setWidth(5);
rect.setHeight(10);
System.out.println(rect.getArea()); // Output is 50

Rectangle square = new Square();
square.setWidth(5);
square.setHeight(10);
System.out.println(square.getArea()); // Output is 100
```

**Better**
```java
interface ShapeInterface {
    public int getArea();
}

class Rect implements ShapeInterface {
    private int width;
    private int height;
    
    public void setWidth(int width) { ... }
    public int getWidth() { ... }
    
    public void setHeight(int height) { ... }    
    public int getHeight() { ... }
    
    @Override
    public int getArea() { return width * height; }
}

class Square implements ShapeInterface {
    private int length;
    public function setLength(int value) { ... }
    public int getLengt() { ... }
    
    @Override
    public int getArea() { return length * length; }
}
...
Rectangle rect = new Rectangle();
rect.setWidth(5);
rect.setHeight(10);
System.out.println(rect.getArea()); // Output is 50

Rectangle square = new Square();
square.setLength(10);
System.out.println(square.getArea()); // Output is 100
```

### Conclusion

Make sure that new derived classes are extending the base classes without changing their behavior.

## Interface Segregation Principle

> Interface Segregation Principle => ISP

### The Definition

The Liskov substitution principle, written by Barbara Liskov in 1988.

> A client should never be forced to implement an interface that it doesn't use or clients shouldn't be forced to depend on methods they do not use.

### Example

Assume we need add new class *Cube* with volume method, example below violate ISP if we implement new class from *ShapeInterface*

**Bad**
```java
interface ShapeInterface {
    public int getArea();
    public int getVolumn();
}

class Rect implements ShapeInterface {
    private int width;
    private int height;
    
    ...
    
    @Override
    public int getArea() { return width * height; }
    
    @Override
    public int getVolumn(); // Violation of the ISP
}

class Cube implements ShapeInterface {
    ...
    
    @Override
    public int getArea() { ... }
    
    @Override
    public int getVolumn() { ... }
}
```

Following it's the code supporting the ISP. By splitting the *ShapeInterface* interface in 2 different interfaces: *ShapeAreaInterface* and *ShapeVolumnInterface*

**Better**
```java
interface ShapeAreaInterface {
    public int getArea();
}

interface ShapeVolumnInterface {
    public int getVolumn();
}

class Rect implements ShapeAreaInterface {
    private int width;
    private int height;
    
    ...
    
    @Override
    public int getArea() { return width * height; }
}

class Cube implements ShapeAreaInterface, ShapeVolumnInterface {
    ...
    
    @Override
    public int getArea() { ... }
    
    @Override
    public int getVolumn() { ... }
}
```

### Conclusion

The Interface Segregation Principle is about business logic to clients communication. It produces a flexible design.

## Dependency Inversion Principle

# References

https://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074

https://springframework.guru/solid-principles-object-oriented-programming/

https://rubygarage.org/blog/solid-principles-of-ood

http://www.oodesign.com/design-principles.html
