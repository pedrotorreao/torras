---
title: 'Object Oriented Programming (OOP)'
date: 2023-05-16T19:22:42-03:00
draft: false
author: 'torras'
tags: ['design-patterns']
---

## Basics

Programming paradigm which focuses on the organization of the software into reusable pieces of code called _`classes`_, based on the grouping of related data and behaviors. Theses classes are user-defined data types that can be thought of as general blueprints or recipes that define the structure for _`objects`_. Objects are concrete instances of classes. They usually have more specifically defined data and they can be used to model real-world objects and/or more abstract entities.

Classes are usually defined in terms of `attributes` and `methods`:

- Attributes represent the state of an object and they are used to store relevant data.
- Methods are functions defined inside a class that describe the behaviors of an object.

In the UML diagram below, we have a very basic `Person` class which contains a few private attributes and public methods:

![OOP_class_example_1](/images/oop_class_1.png)

Below is a C++ implementation of this class:

```cpp
class Person {
  private:
    string name;
    string country;
    string occupation;
    int age;
  public:
    // constructor:
    Person(string name, string country, string occupation, int age)
      : name{name}, country{country}, occupation{occupation}, age{age} {}
    // destructor:
    ~Person(){}
    // getters:
    string getName(){
      return this->name;
    }
    string getNationality(){
      return this->country;
    }
    // setters:
    void changeJob(const string &newJob){
      this->occupation = newJob;
    }
    ...
};
```

The terms _`public`_ and _`private`_ are called access specifiers. Besides these two, in C++ there is also _`protected`_. As the name implies, access specifiers are keywords which define the accessibility of class members, i.e. which class members can be accessed/viewed by the users (code outside the class which declared the attributes/methods) of the class and which ones are to be accessed/seen only internally by the class itself. See below a short description for the specifiers mentioned.

- _`public`_: access is allowed from outside the class;
- _`private`_: access is not allowed from outside the class, internal use only;
- _`protected`_: access is not allowed from outside the class, but inherited classes are allowed access.

In addition to objects, it is also possible to derive other classes from a given class. For cases like this, the original class, the class from which the others are derived, is often called _`base class`_ or _`superclass`_, while the derived classes are called _`subclasses`_. Subclasses inherit the states and behaviors from their _`base class`_ and define only attributes/methods which differ. They can also override the methods inherited, completely replacing the original behavior or just modifying it. A set of classes and subclasses can be represented by its class hierarchy.

To better understand these concepts, let's derive two classes from the `Person` class defined above, the `Teenager` and `Adult` classes. See below a UML diagram representing this class hierarchy.

![OOP_class_example_2](/images/oop_class_2.png)

Both subclasses `Teenager` and `Adult` inherit the methods and attributes from the `Person` class and each one extended the original functionality in their own way.

Below are basic C++ implementations of these classes. Keep in mind that, as done for the previous example, for the sake of simplicity, I've omitted important aspects like data validation.

```cpp
class Teenager : public Person {
  private:
    string schoolYear;
    string dreamJob;
  public:
    Teenager(string name, string country, string occupation, int age, string schoolYear, string dreamJob)
      : Person{name, country, occupation}, schoolYear{schoolYear}, dreamJob{dreamJob} {}

    ~Teenager(){}

    string getSchoolYear(){
      return this->schoolYear;
    }
    string getDreamJob(){
      return this->dreamJob;
    }

    void updateSchoolYear(string newSchoolYear){
      this->schoolYear = newSchoolYear;
    }
    void updateDreamJob(string newDreamJob){
      this->dreamJob = newDreamJob;
    }
    ...
};

class Adult : public Person{
  private:
    boolean married;
    int children;
  public:
    Adult(string name, string country, string occupation, int age, boolean married, int children)
      : Person{name, country, occupation}, married{married}, children{children} {}

    ~Adult(){}

    boolean isMarried(){
      return this->married;
    }
    string getChildrenNumber(){
      return this->children;
    }
    ...
};
```

Notice that, at least in C++, the `inherited` part of the subclass **MUST** be initialized **BEFORE** the subclass is initialized. When a subclass is created, the base class' constructor executes first, then the subclass' constructor is called. However, it's very important to mention the fact that a subclass **DOES NOT** inherit the base class' constructors, destructors, overloaded assignment operators and class `friend` functions. So, in order for the base class attributes to be initialized, the desired constructor must be directly invoked from the derived class. That's what we see in these parts of the code:

```cpp
...
Teenager(string schoolYear, string dreamJob)
      : Person(name, country, occupation), schoolYear{schoolYear}, dreamJob{dreamJob} {}

...
Adult(boolean married, int children)
      : Person(name, country, occupation), married{married}, children{children} {}

...
```

## Pillars

Object Oriented Programming (OOP) relies heavily on a set of four core principles. These are **Encapsulation**, **Abstraction**, **Inheritance** and **Polymorphism**.

### _Encapsulation_

This principle is all about controlling what information is exposed to external parties. In the previous section, I talked about `access specifiers`, here's where they shine.

Encapsulation represents the ability of an object to keep states and behaviors that should not be seen/modified by other objects hidden. This encapsulation is done by making these attributes and methods _`private`_. Thus, in order to be able to interact with the rest of the program, the object exposes only what is relevant to these interactions through _`public`_ methods and `interfaces`.This helps to increase security since it provides a way to avoid that outside parties mess with an object's data unless they are explicitly allowed to. It also makes it easier to collaborate with others without having to worry about compromising sensitive information by hiding all the inner workings.

### _Abstraction_

An abstraction is a model of a real-world object or event, limited to a specific context, focusing on the details which are relevant to said context while omitting the rest. As an example of abstraction, let's think of how we use a coffee machine. We don't really need to know all about its inner workings, its electronics, and the process they perform in order to make coffee. Most of the time it's just a matter of pushing a button. Thus, all the engineering and unnecessary details that are not relevant to the user are hidden, and only an interface (i.e. the button) is exposed, _"abstracting away"_ the complexity.

Similar to what was shown for encapsulation, abstraction uses classes to model real-world scenarios and hides the details behind _`private`_ attributes and methods, exposing only what is relevant through high-level mechanisms such as _`public`_ methods and `interfaces`.

### Inheritance

We've touched on this principle previously when discussing the basics of OOP. Inheritance represents the ability to create new classes from existing ones. The main benefit here is reusability, thus reducing the amount of duplicated code.

Objects may share a lot of similarities, maybe they have the same attributes or share the same logics. In this way, if the need arises for a class with similar class members to an existing one, instead of repeating the same code over and over, we may simply derive a _`subclass`_ from the existing class (_`parent/base class`_), extending its functionality by adding the missing pieces (new attributes and/or methods). By doing this, the common class members are kept in the _`parent`_ class, while the unique attributes/methods are kept in its _`subclasses`_, so each class adds only what is missing to it while reusing the common logic shared by the _`parent`_ class. This forms a class hierarchy, as seen before.

### Polymorphism

By making use of polymorphism, we can have class-specific behavior for the same inherited method (same function signature). This can be achieved by defining the _`parent`_ class as an _`abstract class`_ or _`interface`_. This interface is responsible for outlining the common methods which will be overriden by the _`subclasses`_ to implement their specific versions of it.

The example below shows a simple application of polymorphism. The parent class `Citizen` is an `interface`, created by adding a pure virtual function to a C++ abstract class. The pure virtual function `greet()` must be overriden by each subclass derived from `Citizen` if we want them to be concrete classes and not also interfaces.

```cpp
// abstract class
class Citizen {
    public:
      // pure virtual function, this is what make this class an
      // abstract class and it must be overriden by subclasses:
      virtual void greet(void) const = 0;
};

// each subclass below implement their own version of "greet()"
// by overriding the inherited method:
class Brazilian : public Citizen {
    public:
        virtual void greet(void) const override {
            std::cout << "Olá! Como você está?\n";
        }
};

class Spanish : public Citizen {
    public:
        virtual void greet(void) const override {
            std::cout << "¡Oye! ¿Cómo estás?\n";
        }
};

class German : public Citizen {
    public:
        virtual void greet(void) const override {
            std::cout << "Hallo! Wie geht es dir?\n";
        }
};


int main()
{
    Brazilian* br = new Brazilian();
    br->greet(); // calls the Brazilian class version of greet()

    Spanish* sp = new Spanish();
    sp->greet(); // calls the Spanish class version of greet()

    German* ge = new German();
    ge->greet(); // calls the German class version of greet()

    Citizen* cz = new Brazilian(); // a Brazilian is a Citizen
    cz->greet(); // calls the Brazilian class version of greet()

    // Since subclasses share an 'is-a' relationship with the parent
    // class (i.e. a German is a Citizen, etc), we can add any subclass
    // to the Citizen vector 'ctzs':
    std::vector<Citizen*> ctzs;
    ctzs.push_back(br);
    ctzs.push_back(sp);
    ctzs.push_back(ge);
    ctzs.push_back(cz);

    // Now, polymorphism makes sure that when iterating through all
    // elements of 'ctzs' and calling the "greet()" method, the correct
    // subclass version of it is called without us having to write specific
    // versions of this to each subclass:
    for(auto &ct : ctzs)
        ct->greet();

    return 0;
}
```

Thanks for reading, I hope you found this article useful!
