## Interfaces with ABCs

An interface is the concept of a set of methods that classes or objects all share. They enable *polymorphism* - the ability to treat objects of different types as interchangable. Let's see one way to use interfaces in Python with Abstract Base Classes. These are classes that cannot be instantiated, as they are *abstract*, and only subclasses of them can be *concrete*. Abstract methods of various kinds can be used to force concrete classes to define them (by raising the `TypeError`).

Let's try a simplified conceptual example. We have a canvas and want to draw objects onto it. Instead of supporting only specific classes, let's create an interface and allow any class to make itself available for drawing:
```python
from abc import ABC, abstractmethod

class Drawable(ABC):
    @abstractmethod
    def draw(self, canvas, position):
        pass
```

Note how only one method is defined. By including as few functions as possible, we make it simple and maintainable. Annotations are conspicuously missing to keep the example simple (the many ways to encourage and enforce type safety is a topic for another day). Now we can make our `Shape` subclasses drawable:
```python
class Circle(Drawable, Shape):
    def set_radius(self, radius):
        ...

    def draw(self, canvas, position):
        ...

class Triangle(Drawable, Shape):
    def get_angles(self):
        ...

    def draw(self, canvas, position):
        ...
```

Nice, even though circles and triangles are implemented very differently and behave differently, both can still share the quality of being drawable. But wait... Why not just put the `draw` method in the `Shape` base class? That would ignore supporting drawing images and not just shapes. And what about N-dimensional shapes that do not support being drawn on a 2D canvas?

Theoretically in Python there is little difference between inheritence and interfaces using ABCs. You could say it's all about planning the correct place to define functionality but that may be missing the point. Conceptually, interfaces care about behavior and not type:
```python

class BmpImage(Drawable, Image):
    def save(self, filepath):
        ...

    def draw(self, canvas, position):
        ...
```

Even though `BmpImage` has virtually nothing to do with `Triangle`, they can still be passed interchangeably as drawable objects. We have decoupled the ***behavior*** from the ***type***. There is much more to ABCs and MetaClasses, and we have only scratched the surface for the sake of demonstrating a design principle. Your milage may vary and you may find this is a stronger tool in strongly-typed languages.

References:
- https://docs.python.org/3.10/library/abc.html
- https://peps.python.org/pep-3119/
