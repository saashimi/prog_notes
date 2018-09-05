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