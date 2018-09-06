The Four Pillars of OOP
* Abstraction
* Encapsulation
* Inheritance
* Polymorphism

## Building Entity Classes

Shortcuts
```csharp
Customer customer = new Customer();
// Can be shortened to:
var customer = new Customer();
```
Objects are <b>Reference Types.</b>


Method / function signature - comprised of its name and type of each of its 
formal parameters
```csharp
public Customer Retrieve(int customerId)
//              ------------------------
//               This is the signature
//              ------------------------
// `Customer` is the return type.

``` 
Overloading : describing methods that have the same name, but different parameters.
* E.g. `Retrieve()` is called to return a list, whereas `Retrieve(1001)` returns a specific CustomerId.

Contract: The class makes a "promise" that it will return specific properties and methods to any other code that needs them. Also called the class interface.

Constructors: Use `ctor` snippet to create a constructor. A constructor with no parameters is called a default constructor.

Nullable type:
```csharp
public Decimal? CurrentPrice { get; set; }
```
Allows definition of the value or a null.

#### Collaboration
A "uses a" relationship. Objects use instances from one or more classes.

#### Composition
A "has a" relationship. Think of properties from other classes.

#### Ids
References an integer instead of specific objects. Simpler; does not have to reference individual object properties.

#### Inheritance
An "is a" relationship. Create base and derived classes. Can only inherit from one class at a time in C#. But supports inheritance chaining. 

Overriding base class functions. Recommend 
```csharp
public override Tostring() 
{
    // override here
}
```

#### Building Base Classes
* Abstract classes cannot be instantiated.
* Concrete classes can be instantiated.
* `sealed` classes cannot be extended through inheritance.
* `public` means any class can access the class.
* `private` means that only inherited classes can access the class.
* `protected` means only the base class and only the derived classes can access teh class.

A derived class cannot override a base class member unless the base class member is declared specifically to allow it to be overridden using the `abstract` or `virtual` keyword.

The derived member must then use the `override` keyword to explicityly indicate that the method is overridden.