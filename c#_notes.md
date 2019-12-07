# C# Notes

### General

Think of methods as the OOP equivalent of functions in scripting languages.

#### Common Language Runtime 
* CLR manages your application.
* Memory management.
* Operating system and hardware independence.
* Language Independence.

#### Framework Class Library
* A library of functionality to build applications.

#### Basic Code Snippet Examples
```csharp
public static void Main()
{
    if (DateTime.Now.DayOfWeek == DayOfWeek.Monday)
    {
        Console.WriteLine("Another case of the Mondays!");
    }
}
```
`Main()` is a method. This method may be called from other places so that 
nested code can be executed and run. 

`DateTime` is a part of the DateTime class library.
* `.Now.DayOfWeek` and `.Monday` are methods.

```csharp
using System;

public class Program
{
    static void Main()
    {
        Console.WriteLine("Hello, World!");
    }
}
```
We ran 
`c:\Windows\Microsoft.NET\Framework\v4.0.30319\csc.exe hello.cs` in the command 
line to compile hello.cs example above.

* Created a new project.
* Went over the <b>Solution Explorer</b> to add new files.

To run:
* Debug Menu -> Start without debugging. 
    * Will begin compiling and execute the program.

#### Debugging
* Set breakpoints in VisualStudio by clicking on the left hand margin of the Program.cs window.
* F10 to step over line.

To pass args when in the debugger, right click on `<program name>.cs` and click
on properties.
* in debug settings, can enter command line arguments in the text box. 

Introduced `Console.Readline();`.

variables are <b>camelCase.</b>

### Objects

In C#, most programmers follow the convension that each class is stored in its 
own file. 

* In Solutions Explorer, right click on Project, and click add --> class.

Class members define
1. State
2. Behavior

```csharp
namespace Grades
{
    class Program
    {
        static void Main(string[] args)
        {
            GradeBook book = new GradeBook();
            book.AddGrade(91);
            book.AddGrade(89.5f);
        }
    }
}
```

We needed `book.AddGrade(89.5f)` to force it as a floating point instead of a 
double.

```csharp
namespace Grades
{
    class GradeBook
    {
        public void AddGrade(float grade)
        {
            grades.Add(grade);
        }
        List<float> grades = new List<float>();
    }
}
```

#### Constructors
`GradeBook book = new GradeBook();

```csharp
public GradeBook()
{
    // ... initialization code
}
```
`ctor + TAB` will cause Visual Studio to generate the constructor code.

Remember the differences between classes vs variables
* A class is a blueprint for creating objects.
* A class can also be used to type a variable
    * A variable can refer to an object of the same type.

#### Keywords

Encapsulation: enclosing or hiding details.

`public` makes a class member publicly available. Can be accessiblie from 
outside the class.

But if not explicitly called out, the default behavior in C# is to make methods
 `private` (only useable inside the class).

`static` are members of a class without creating an instance:
```csharp
public static float MinimumGrade = 0
public static float MaximumGrade = 100
```
`partial` classes in the same namespace can "see" each other--even though they're in different files. A class can span multiple files, too, but you need to use the partial keyword when you declare it. The _only_ way to break up a class into multiple files is to use the `partial` keyword.


```csharp
// SomeClasses.cs
partial class Cat {
    public void Meow() {
        // Some statements
    }
}

// SomeMoreClasses.cs
partial class Cat {
    public void Purr() {
        // statements
    }
}
```


```csharp
public GradeStatistics ComputeStatistics()
```
`ComputeStatistics()` returns a `GradeStatistics` object

`cw + TAB + TAB` will do a `Console.WriteLine()`
`testm + TAB + TAB` will insert a `[TestMethod]`.

### Types
`/* */` for comment blocks.

Pluralsight goes over pointers to objects for a little bit. Created a new folder in the Grades.Tests Project. Used Thunder's methodology of writing unit tests to understand the workings of the program. 

<b>Variables hold the value.</b>
* No pointers, no references.

Many built-in primitives are value types
* int, double, float.

How to tell the difference between value types and reference types?

`struct` definitions create value types.

You want to write a class by default. You use `struct` to represent a single value, like `DateTime.`

```csharp
public struct DateTime
{
    // ...
}
```
An enum creates a value type

```csharp
public enum PayrollType
{
    Contractor = 1,
    Salaried, 
    Executive,
    Hourly
}

if(employee.Role == PayrollType.Hourly) 
{
    // ...
}
```

Hovering over the object in source code and pressing `F12` will take us to the source code. The source code can reveal if the object is a value or reference type.

### Method Parameters

Parameters pass "by value"
* Reference types pass a copy of the reference.
* Value types pass a copy of the value. 

### Immutability

Value types are usually immutable. 
Strings behave like a value type.

```csharp
string name = " SomeName ";
name.Trim();
```
This is wrong. 

```csharp
string name = " SomeName ";
name = name.Trim();
```
Must set the variable to itself.

### Arrays
Manage a collection of variables. 
* <b>Arrays are a fixed size.</b> Lists can be grown.
* Always 0 index

```csharp
const int numberOfStudents = 4;
int[] scores = new int[numberOfStudents];

int totalScore = 0;
foreach(int score in scores)
{
    totalScore += score;
}
```

`int` is the type that the array will hold.

### Methods
* Methods define behavior (Think of them like Python / R) functions
* Every method has a return type
    * `void` if no value returned.
* Every method has zero or more parameters
    * use `params` keyword to accept a variable number of parameters
* Every method has a signature.
    * `Name` of method + parameters
    * Method overloading allows us to use methods with the same name, but differing parameters.

### Fields and Properties
* Fields are variables of a class (think Name Fields)
    * Fields are typically private and hold data.
    * some developers use `_underscores` to name private fields.

```csharp
public class Animal
{
    private readonly string _name;

    public Animal(string name)
    {
        _name = name;
    }
}
```
* A property can define a get and/or set accessor. 
    * Often used to expose and control fields.
    * Properties are typically <b>public.</b>
    * Properties are all about state.
* Capitalize Property names and Method names.

```csharp
private string _name;

public string Name
{
    get { return _name; }
    set
    {
        if (!String.IsNullOrEmpty(value))
        {
            _name = value;
        }
    }
}
```
* Some .NET libraries will only look or move properties vs fields.
    * Set accessor can provide validation and protect state.

### Events
* Allows a class to send notifications to other classes or objects.
    * Publisher raises the event.
    * One or more subscribers process the event.

### Delegates
To understand events, we must understand delegates.
* Delegates allow you to have a variable that references a method. 

```csharp
public delegate void Writer(string message);

// Elsewhere...
public class Logger
{
    public void WriteMessage(String message)
    {
        Console.WriteLine(message);
    }
}

// Elsewhere...
Logger logger = new Logger();
// Note no parentheses after logger.WriteMessage:
Writer writer = new Writer(logger.WriteMessage); 
writer("Success!!");
```

Can use `+=` to reference multiple methods.

### Events Revisited
* Convention is to pass the old values and the new ones as parameters.

`prop` + `TAB` + `TAB` to get property

### Control Flow
Can have ternary operators 

```csharp
string pass = age > 20 ? "pass" : "nopass";
```
* `switch` and `case` statements,
* `if`, `else if`, `else` statements.

### Iterating
```csharp
// For each item in an array.
// The preferred way to create a loop when possible.
int[] ages = {2, 21, 40, 72, 100}; // Array initialization
foreach (int value in ages)
{
    Console.WriteLine(value);
}
```

```csharp
// For a given number of iterations.
for (int i = 0; i < age; i++)
{
    Console.WriteLine(i)
}
```

```csharp
while(age > 0)
{
    age -= 1;
    Console.WriteLine(age);
}
```
```csharp
// Execute code at least once
do
{
    age++;
    Console.WriteLine(age)
} while (age < 100);
```

`for` + `Tab` + `Tab` to generate a for loop.

### Jumping
`break`, `continue`, `goto`, `return`, `throw`.
Typically don't use `goto` these days.

```csharp
foreach(int age in ages) {
    if (age==2) {
        continue;
    }
    if (age==21) {
        break;
    }
}
```
You can use return in a void method, but you can't return a value to the caller. 
```csharp
void CheckAges()
{
    foreach (int age in ComputerAges())
    {
        if (age == 21) return;
    }
}
```

### Throwing

Use `throw` to raise an exeption. Exceptions provide type safe and structured error handling in .NET.

```csharp
if (age == 21)
{
    throw new ArgumentException("21 is not a legal value");
}
```
Handle exceptions using a try block
```csharp
try
{
    ComputeStatistics();
}
catch(DivideByZeroException ex)
{
    Console.WriteLine(ex.Message);
    Console.WriteLine(ex.StackTrace);
}
```
Chaining Catch Blocks
* Place most specific type in the first catch clause
* Catching a System.Exception catches everything
    * Except for a few "special" exceptions.

```csharp
try{
    // ...
}
catch(DivideByZeroException ex)
{
    // ...
}
catch(Exception ex)
{
    // ...
}
```
Finally clause adds finalization code
* Executes even when control jumps out of scope

```csharp
FileStream file = new FileStream("file.txt", FileMode.Open);
try
{
}
finally
{
    file.Close();
}

// Elsewhere...
using(FileStream file1 = new FileStream("in.txt", FileMode.Open)
using(FileStream file2 = new FileStream("out.txt",         FileMode.Create)))
{
    // ...
}
```

`CTRL` + `.` to refactor highlighted code via `ExtractMethod`.

## Object Oriented Programming
### Encapsulation
Hiding complexity and building models. 

### Inheritance
One class inherits methods from another class.

```csharp
public class A
{
    public void DoWork()
    {
        // ... work!
    }
}

public class B : A // : denotes the inheritance
{

}

public class C : B
{

}
```

public, private, protected.

`protected` can be accessed by code from a derived class.

### Polymorphism
* One variable can point to different types of objects.
* Objects can behave differently depending on their type.

```csharp
public class A : Object
{
    public virtual void DoWork()
    {
        // ...
    }
}

public class B : A
{
    public override void DoWork()
    {
        // optionally call into base ...
        base.DoWork();
    }
}
```
`virtual` type and `override` type.

### Abstract Classes
Abstract classes cannot be instantiated.
* Can contain abstract members.

```csharp
public abstract class Window
{
    public virtual string Title {get ; set; }

    public virtual void Draw()
    {
        // ... drawing code
    }

    public abstract void Open();
}
```
An abstract method is implicitly a virtual method.

### Interfaces
Interfaces contain no implementation details.
* Defines only the signatures of methods, events, and properties.
* A type can implement multiple interfaces. 

```csharp
// Interfaces are defined with a leading capital I.
public interface IWindow
{
    string Title { get; set; }
    void Draw();
    void Open();
}
```
K. Scott Allen prefers interfaces over abstract base classes.

#### Important Interfaces
* `IDisposable`, Release resources 
* `IEnumerable`, Supports iteration
* `INotifyPropertyChange`, Raises events when properties change
* `IComparable`, Compares for sorting

#### Debugging
Right click on a line of code or click on the margin to insert a breakpoint. `F5` to start debugging.\
As soon as the program gets to the line of code that has the breakpoint, the IDE brings up the code editor and highlights the current line of code.
Right click on a variable and choose Expression -> Variable -> Add Watch to see the value of the variable progress as the program carries on. 
Press `F10` to step through code.

#### Notes on casing
camelCase is used for private fields and PascalCase for the public fields.