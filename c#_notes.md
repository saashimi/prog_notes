# C# Notes

From Pluralsight stuff.

### General

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
`Main()` is a method. This method may be called from other places so that nested code can be executed and run. 

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
`c:\Windows\Microsoft.NET\Framework\v4.0.30319\csc.exe hello.cs` in the command line to compile hello.cs example above.

* Created a new project.
* Went over the <b>Solution Explorer</b> to add new files.

To run:
* Debug Menu -> Start without debugging. 
    * Will begin compiling and execute the program.

#### Debugging
* Set breakpoints in VisualStudio by clicking on the left hand margin of the Program.cs window.
* F10 to step over line.

To pass args when in the debugger, right click on `<program name>.cs` and click on properties.
* in debug settings, can enter command line arguments in the text box. 

Introduced `Console.Readline();`.

variables are <b>camelCase.</b>

### Objects

In C#, most programmers follow the convension that each class is stored in its own file. 

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

We needed `book.AddGrade(89.5f)` to force it as a floating point instead of a double.

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

`public` makes a class member publicly available. Can be accessiblie from outside the class.

But if not explicitly called out, the default behavior in C# is to make methods `private` (only useable inside the class).

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

