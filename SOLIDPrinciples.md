# S.O.L.I.D Principles of Object Oriented Programming

1. [Single Responsibility Principle](#single_responsibility_principle)
2. [Open Closed Principle](#open_closed_principle)
3. [Liskov Substitution Principle](#liskov_substitution_principle)
4. [Interface Segregation Principle](#interface_segregation_principle)
5. [Dependency Inversion Principle](#dependency_inversion_principle)

## Single Responsibility Principle

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
    
    public void render() { ... } // Violation of the Single Responsibility Principle
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

class Drawer {
    private Rect rect;
    
    Drawer(Rect rect) {
        this.rect = rect;
    }
    
    public void render() { ... }
}
```

### Conclusion
* Keep your code simple.
* Debugging would be easier.
* Easier to upgrade and expand.

## Open Closed Principle
## Liskov Substitution Principle
## Interface Segregation Principle
## Dependency Inversion Principle

# References

https://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074
https://springframework.guru/solid-principles-object-oriented-programming/
https://rubygarage.org/blog/solid-principles-of-ood
http://www.oodesign.com/design-principles.html
