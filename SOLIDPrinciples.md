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

Continue with Better code exapmle in first principle, we must to update *Graphics* class when new *Circle* class is added 
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
## Interface Segregation Principle
## Dependency Inversion Principle

# References

https://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074

https://springframework.guru/solid-principles-object-oriented-programming/

https://rubygarage.org/blog/solid-principles-of-ood

http://www.oodesign.com/design-principles.html
