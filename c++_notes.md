# C++ Notes
### Stream I/O
```cpp
#include <iostream>

>> // sends something to a stream
<< // reads something from a stream
```
By default, C++ treats decimals as doubles.

### Functions
All functions have a return type.

`auto` changes the variable type based on its value.

```cpp
double add(double x, double y)
{
    return x + y;
}

double add(double a, double b, double c)
{
    return a + b + c
    // or return add(add(a, b), c);
}
```

`void` means the function has no return type!

### Header files
Long collections of declarations in many files have disadvantages.
* Solution - put the declarations in a separate file that is included into each file as you compile.
* Use one .h file and one .cpp file per class.

```cpp
#include "SomeHeaderFile.h"
```
Never use `using` namespace statements in header files! You can end up using ambiguous namespaces by accident. use `::` (scope resolution operator) in full in header files.

### Vectors
```cpp
#include <vector>
// ...
// Push back is similar to python's list.append()
vector<int> vi;
for (int i=0; i<10; i++)
{
    vi.push_back(i);
}
```

### Classes
Best practices:
* One header file per class just explains what is in the class.
* One .cpp file per class implements all the functions.
* Any code that uses the class includes the header.
    * So does the .cpp file that implements the class.

A constructor that takes no arguments is called the <b>default constructor.</b>

```cpp
Account acct; // Declares an object with a default constructor.

Account acct(); // Actually declares a function!
```

### Const keyword
```cpp
const int amount = 90;
```