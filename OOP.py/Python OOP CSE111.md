# LAB 10

  

## Inheritance

  

<aside>

💡 Child class is also known as `subclass` or `derived class`

  

</aside>

  

<aside>

💡 Parent class is also known as `base class` or `superclass`

  

</aside>

  

Let's consider an example of single inheritance.

  

```python

# parent class

class Animal:

    def __init__(self, name):

        self.name = name

  

# child class

class Dog(Animal):

    pass

```

  

Here we have a base class `Animal` with the `__init__` method and a subclass `Dog` that inherits from the base class. The keyword `pass` allows us not to write anything in the definition of the child class.

  

Now that we've defined classes, we can create objects:

  

```python

cow = Animal("moo")  # instance of Animal

corgi = Dog("spike")   # instance of Dog

```

  

We haven't defined the `__init__` for the class `Dog` but since it's a child of `Animal`, it inherited its `__init__`. So if we tried to declare an instance of the class `Dog` in a different way, we would get an error:

  

```python

labrador = Dog()  # TypeError

```

  

```python

print(isinstance(cow, Animal))    # True

print(isinstance(corgi, Animal))  # True

  

print(isinstance(cow, Dog))    # False

print(isinstance(corgi, Dog))  # True

```

  

```python

print(issubclass(Dog, Animal))  # True

print(issubclass(Animal, Dog))  # False

  

print(issubclass(Dog, Dog))     # True

print(issubclass(corgi, Dog))   # TypeError

```

  

<aside>

💡 The definition of the parent class should precede the definition of the child class, otherwise, you'll get a `NameError`! If a class has several subclasses, its definition should precede them all. The "sibling" classes can be defined in any order.

  

</aside>

  

<aside>

💡 A derived class inherits all traits from its base class. Those traits are directly accessible in the derived class, unless they have been defined as private in the base class (with two underscores before the name of the trait).

  

</aside>

  

## Overriding

  

<aside>

💡 One important concept of object-oriented programming is **overriding.** Overriding is the ability of a class to change the implementation of the methods inherited from its ancestor classes.

  

</aside>

  

```python

class Parent:

    def do_something(self):

        print("Did something")

  

class Child(Parent):

    def do_something(self):

        print("Did something else")

  

parent = Parent()

child = Child()

  

parent.do_something()  # Did something

child.do_something()  # Did something else

```

  

Here, the method `do_something` is overridden in the class `Child`. If we hadn't overridden it, the method would have the same implementation as in the class `Parent`. The code `child.do_something()` would then print `Did something`.

  

```python

class Parent:

    def __init__(self, name):

        self.name = name

        print("Called Parent __init__")

  

class Child(Parent):

    def __init__(self, name):

        super().__init__(name)

        print("Called Child __init__")

```

  

```python

jack = Child("Jack")

# Called Parent __init__

# Called Child __init__

```

  

Let's take a look at a class named `Thesis` which inherits the `Book` class. The derived class *can* still call the constructor from the base class:

  

```python

class Book:

    """ This class models a simple book """

    def __init__(self, name, author):

        self.name = name

        self.author = author

  

class Thesis(Book):

    """ This class models a graduate thesis """

  

    def __init__(self, name, author, grade):

        super().__init__(name, author)

        self.grade = grade

```

  

The constructor in the `Thesis` class calls the constructor in the base class `Book` with the arguments for `name` and `author`. Additionally, the constructor in the derived class sets the value for the attribute `grade`. This naturally cannot be a part of the base class constructor, as the base class has no such attribute.

  

```python

if __name__ == "__main__":

        thesis = Thesis("Deep learning", "John Doe", 4)

  

    # Print out the values of the attributes

    print(thesis.name)

    print(thesis.author)

    print(thesis.grade)

```

  

## Child overrides parent method (Child can still call overridden methods)

  

Even if a derived class *overrides* a method in its base class, the derived class can *still* call the overridden method in the base class. In the following example we have a basic `BonusCard` and a special `PlatinumCard` for especially loyal customers. The `calculate_bonus` method is overridden in the derived class, but the overriding method calls the base method:

  

```python

class Product:

  

    def __init__(self, name, price):

        self.name = name

        self.price = price

  

class BonusCard:

  

    def __init__(self):

        self.products_bought = []

  

    def add_product(self, product: Product):

        self.products_bought.append(product)

  

    def calculate_bonus(self):

        bonus = 0

        for product in self.products_bought:

            bonus += product.price * 0.05

  

        return bonus

  

class PlatinumCard(BonusCard):

  

    def __init__(self):

        super().__init__()

  

    def calculate_bonus(self):

        # Call the method in the base class

        bonus = super().calculate_bonus()

  

        # ...and add five percent to the total

        bonus = bonus * 1.05

        return bonus

```

  

So, the bonus for a PlatinumCard is calculated by calling the overriden method in the base class, and then adding an extra 5 percent to the base result. An example of how these classes are used:

  

```python

if __name__ == "__main__":

    card = BonusCard()

    card.add_product(Product("Bananas", 100))

    card.add_product(Product("Milk", 200))

    bonus = card.calculate_bonus()

  

    card2 = PlatinumCard()

    card2.add_product(Product("Bananas",100))

    card2.add_product(Product("Milk", 200))

    bonus2 = card2.calculate_bonus()

  

    print(bonus)

    print(bonus2)

```

  

## **Access modifiers**

  

<aside>

💡 If a trait is defined as private in the base class, it is not directly accessible in any derived classes, as was briefly mentioned in the previous section. Let's take a look at an example. In the `Notebook` class below the notes are stored in a list, and the list attribute is private:

  

</aside>

  

```python

class Notebook:

    """ A Notebook stores notes in string format """

  

    def __init__(self):

        # private attribute

        self.__notes = []

  

    def add_note(self, note):

        self.__notes.append(note)

  

    def retrieve_note(self, index):

        return self.__notes[index]

  

    def all_notes(self):

        return ",".join(self.__notes)

```

  

```python

class NotebookPro(Notebook):

    """ A better Notebook with search functionality """

    def __init__(self):

        # This is OK, the constructor is public despite the underscores

        super().__init__()

  

    # This causes an error

    def find_notes(self, search_term):

        found = []

        # the attribute __notes is private

        # the derived class can't access it directly

        for note in self.__notes:

            if search_term in note:

                found.append(note)

  

        return found

```

  

```python

AttributeError: 'NotebookPro' object has no attribute '_NotebookPro__notes'

```

  

### Protected traits

  

<aside>

💡 Many object oriented programming languages have a feature, usually a special keyword, for *protecting* traits. This means that a trait should be hidden from the clients of the class, but kept accessible to its subclasses. Python in general abhors keywords, so no such feature is directly available in Python. Instead, there is a *convention* of marking protected traits in a certain way.

  

</aside>

  

<aside>

💡 Remember, a trait can be hidden by prefixing its name with two underscores:

  

</aside>

  

```python

def __init__(self):

    self.__notes = []

```

  

The agreed convention to *protect* a trait is to prefix the name with a *single* underscore. Now, this is *just* a convention. Nothing prevents a programmer from breaking the convention, but it is considered a bad programming practice.

  

```python

def __init__(self):

    self._notes = []

```

  

```python

class Notebook:

    """ A Notebook stores notes in string format """

  

    def __init__(self):

        # protected attribute

        self._notes = []

  

    def add_note(self, note):

        self._notes.append(note)

  

    def retrieve_note(self, index):

        return self._notes[index]

  

    def all_notes(self):

        return ",".join(self._notes)

  

class NotebookPro(Notebook):

    """ A better Notebook with search functionality """

    def __init__(self):

        # This is OK, the constructor is public despite the underscores

        super().__init__()

  

    # This works, the protected attribute is accessible to the derived class

    def find_notes(self, search_term):

        found = []

        for note in self._notes:

            if search_term in note:

                found.append(note)

  

        return found

# Create an instance of Notebook

my_notebook = Notebook()

  

# Add notes to the notebook

my_notebook.add_note("Meeting at noon")

my_notebook.add_note("Dentist appointment at 3 PM tomorrow")

my_notebook.add_note("Grocery shopping list: milk, eggs, flour")

  

# Retrieve a specific note by index

note_at_index_0 = my_notebook.retrieve_note(0)

print("Note at index 0:", note_at_index_0)

  

# Print all notes

all_notes = my_notebook.all_notes()

print("All notes:", all_notes)

  

# Create an instance of NotebookPro

my_notebook_pro = NotebookPro()

  

# Add notes to the enhanced notebook

my_notebook_pro.add_note("Python workshop next Saturday")

my_notebook_pro.add_note("Call with John about the new project")

my_notebook_pro.add_note("Remember to update the project documentation")

  

# Use the search functionality

search_result = my_notebook_pro.find_notes("project")

print("Notes containing 'project':", search_result)

  

```

  

Below we have a handy table for the visibility of attributes with different access modifiers:

  

![Untitled](LAB%2010%20b0898d1c7b8d42dda5ae0596f9e6f005/Untitled.png)