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
