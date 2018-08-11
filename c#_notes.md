# C# Notes

From Pluralsight stuff.

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