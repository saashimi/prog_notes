## C Notes
### For loops

```c
/* Bracing in K&R style */
for ( i = 1; i < 101; i++ ) {
    /* do something */
}
```

### Pointers
A pointer is a variable whose value is the address of another variable, i.e.,
the direct address of the memory location.

Note: Sometimes the notation is confusing, because different textbooks place the * differently.  The three following declarations are equivalent:
```c
int *p;
int* p; 
int * p; 
```

Note: The notation can be a little confusing.

- If you see the * in a declaration statement, with a type in front of the *, a pointer is being declared for the first time.
- AFTER that, when you see the * on the pointer name, you are dereferencing the pointer to get to the target.




```c
#include <stdio.h>

int main () {

   int  var = 20;   /* actual variable declaration */
   int  *ip;        /* pointer variable declaration */

   ip = &var;  /* store address of var in pointer variable*/

   printf("Address of var variable: %x\n", &var  );

   /* address stored in pointer variable */
   printf("Address stored in ip variable: %x\n", ip );

   /* access the value using the pointer */
   printf("Value of *ip variable: %d\n", *ip );

   return 0;
}
```
Returns:
```
Address of var variable: 60fef8
Address stored in ip variable: 60fef8
Value of *ip variable: 20
```

### Returning Arrays

Copied from [Stack Overflow.](https://stackoverflow.com/questions/1496313/returning-c-string-from-a-function)

your function signature needs to be:

```c
const char * myFunction()
{
    return "My String";
}
Edit:
```

Background:

It's been years since this post & never thought it would be voted up, because it's so fundamental to C & C++. Nevertheless, a little more discussion should be in order.

In C (& C++ for that matter), a string is just an array of bytes terminated with a zero byte-- hence the term "string-zero" is used to represent this particular flavour of string. There are other kinds of strings, but in C (& C++), this flavour is inherently understood by the language itself. Other languages (Java, Pascal, etc) use different methodologies to understand "my string".

If you ever use the Windows API (which is in C++), you'll see quite regularly function parameters like: "LPCSTR lpszName". The 'sz' part represents this notion of 'string-zero': an array of bytes with a null (/zero) terminator.

Clarification:

For the sake of this 'intro', I use the word 'bytes' and 'characters' interchangeably, because it's easier to learn this way. Be aware that there are other methods (wide-characters, and multi-byte character systems--mbcs) that are used to cope with international characters. UTF-8 is an example of a mbcs. For the sake of intro, I quietly 'skip over' all of this.

Memory:

What this means is that a string like "my string" actually uses 9+1 (=10!) bytes. This is important to know when you finally get around to allocating strings dynamically. So, without this 'terminating zero', you don't have a string. You have an array of characters (also called a buffer) hanging around in memory.

Longevity of data:

The use of the function this way:

```c
const char * myFunction()
{
    return "My String";
}
int main() 
{
    const char* szSomeString = myFunction(); // fraught with problems
    printf("%s", szSomeString);
}
```
... will generally land you with random unhandled-exceptions/segment faults and the like, especially 'down the road'.

In short, although my answer is correct -- 9 times out of 10 you'll end up with a program that crashes if you use it that way, especially if you think it's 'good practice' to do it that way. In short: It's generally not.

For example, imagine some time in the future, the string now needs to be manipulated in some way. Generally, a coder will 'take the easy path' and (try to) write code like this:

```c
const char * myFunction(const char* name)
{
    char szBuffer[255];
    snprintf(szBuffer, sizeof(szBuffer), "Hi %s", name);
    return szBuffer;
}
```
That is, your program will crash because the compiler (may/may not) have released the memory used by szBuffer by the time the printf() in main() is called. (Your compiler should also warn you of such problems beforehand).

There are two ways to return strings that won't barf so readily.

returning buffers (static or dynamically allocated) that live for a while. In C++ use 'helper classes' (eg. std::string) to handle the longevity of data (which requires changing the function's return value), or
pass a buffer to the function that gets filled in with information.
Note that it is impossible to use strings without using pointers in C. As I have shown, they are synonymous. Even in C++ with template classes, there are always buffers (ie pointers) being used in the background.

So, to better answer the (now modified question). (there are sure to be a variety of 'other answers' that can be provided).

Safer Answers:

eg 1. using statically allocated strings:
```c
const char* calculateMonth(int month) 
{
    static char* months[] = {"Jan", "Feb", "Mar" .... }; 
    static char badFood[] = "Unknown";
    if (month<1 || month>12) 
        return badFood; // choose whatever is appropriate for bad input. Crashing is never appropriate however.
    else
        return months[month-1];
}
int main()
{
    printf("%s", calculateMonth(2)); // prints "Feb"
}
```
What the 'static' does here (many programmers do not like this type of 'allocation') is that the strings get put into the data segment of the program. That is, it's permanently allocated.

If you move over to C++ you'll use similar strategies:

```c
class Foo 
{
    char _someData[12];
public:
    const char* someFunction() const
    { // the final 'const' is to let the compiler know that nothing is changed in the class when this function is called.
        return _someData;
    }   
}
```
... but it's probably easier to use helper classes, such as std::string, if you're writing the code for your own use (and not part of a library to be shared with others).

eg 2. using caller-defined buffers:

This is the more 'fool proof' way of passing strings around. The data returned isn't subject to manipulation by the calling party. That is, eg 1 can easily be abused by a calling party and expose you to application faults. This way, it's much safer (albeit uses more lines of code):

```c
void calculateMonth(int month, char* pszMonth, int buffersize) 
{
    const char* months[] = {"Jan", "Feb", "Mar" .... }; // allocated dynamically during the function call. (Can be inefficient with a bad compiler)
    if (!pszMonth || buffersize<1) 
        return; // bad input. Let junk deal with junk data.
    if (month<1 || month>12)
    {
        *pszMonth = '\0'; // return an 'empty' string 
        // OR: strncpy(pszMonth, "Bad Month", buffersize-1);
    }
    else
    {
        strncpy(pszMonth, months[month-1], buffersize-1);
    }
    pszMonth[buffersize-1] = '\0'; // ensure a valid terminating zero! Many people forget this!
}

int main()
{
    char month[16]; // 16 bytes allocated here on the stack.
    calculateMonth(3, month, sizeof(month));
    printf("%s", month); // prints "Mar"
}
```
### KS Implementation of this:
```c
#include <stdio.h>
#include <string.h>

void myFunction( char* pStr );

int main( void )
{
    char pStr[17]; /* pStr is a char pointer */
    myFunction( pStr );
    printf( "%s", pStr );
}

void myFunction( char* pStr ) /* Input var is a char pointer */
{
    const char str[20] = "This is a string"; /* Constant char array*/
    strcpy( pStr, str );
}
```

There are lots of reasons why the 2nd method is better, particularly if you're writing a library to be used by others (you don't need to lock into a particular allocation/deallocation scheme, 3rd parties can't break your code, you don't need to link to a specific memory management library), but like all code, it's up to you on what you like best. For that reason, most people opt for eg 1 until they've been burnt so many times that they refuse to write it that way anymore ;)

disclaimer:

I retired several years back and my C is a bit rusty now. This demo code should all compile properly with C (it is ok for any C++ compiler though).

### TODO:
- Mutators
- Structs
- Arrow notation ->
- Header files
- Multifile projects
