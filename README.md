## Preface

### Rule of Thumb

    1. Readability first. (Your code should be your documentation most of the time.)
    2. Crash/Assert early. Don't wait until the worst case happens to make the crash condition.
    3. Follow IDE's auto formatted style unless you have really good reasons not to do so. (Ctrl + K + D in VC++)
    4. Learn from existing code.

### References

    https://docs.google.com/document/d/1cT8EPgMXe0eopeHvwuFmbHG4TJr5kUmcovkr5irQZmo/mobilebasic?usp=gmail

## 1. Naming Conventions and Style

#### 1. Use Pascal casing for class and structs.

```cpp
class PlayerManager;
struct AnimationInfo;
```

#### 2. Use camel casing for local variable names and function parameters.

```cpp
void SomeMethod(const int someParameter)
{
	int someNumber;
}
```

#### 3. Use verb-object pairs for method names

```cpp
class SomeClass
{
public:
	// a. Use pascal casing for public methods
	void DoSomehting();

private:
	// a. Use camel casing for other methods
	void doSomething();
};
```

#### 4. Use ALL_CAPS_SEPARATED_BY_UNDERSCORE for constants and defines.

```cpp
constexpr int SOME_CONSTANT = 1;
```

#### 5. Use all lowercase letters for namespaces.

```cpp
namespace abc
{
}
```

#### 6. Prefix boolean variables with b.

```cpp
bool bFired;  // for local and public member variable
bool mbFired; // for private class member variable
```

#### 7. Prefix interfaces with I.

```cpp
class ISomeInterface;
```

#### 8. Prefix enums with e.

```cpp
enum class eDirection
{
	North,
	South
};
```

#### 9. Prefix class member variables with m.

```cpp
class Employee
{
protected:
	int mDepartmentID;

private:
	int mAge;
};
```

#### 10. Methods with return values must have a name describing the value returned.

```cpp
uint32_t GetAge() const
{
	return 0;
}
```

#### 11. Use descriptive variable names e.g, index or employee instead of i or eunless it is a trivial index variable used for loops.

#### 12. Capitalize every characters in acronyms only if there is no extra word after them.

```cpp
int OrderID;
int HttpCode;
```

#### 13. Always use setter and getters for class member variables.

```cpp
// Use:
class Employee
{
public:
	const string& GetName() const;
	void SetName(const string& name);

private:
	string mName;
};

// Instead of:
class Employee
{
public:
	string Name;
};
```

#### 14. Use only public member variables for a struct. No functions are allowed. Use pascal casing for the members of a struct.

```cpp
struct MeshData
{
	int32_t VertexCount;
};
```

#### 15. Use #include<> for external header files. Use #include "" for in-house header files.

#### 16. Put external header files first, followed by in-house header files in alphabetic order if possible.

```cpp
#include <undrdered_map>
#include <vector>

#include "AnimationInfo.h"
```

#### 17. There must be a blank line between includes and body.

#### 18. Use #pragma once at the beginning of every header file.

#### 19. Use Visual Studio default for tabs. If you are not using Visual Studio, use real tabs that are equal to 4 spaces.

#### 20. Declare local variables as close as possible to the first line where it is being used.

#### 21. Always place an opening curly braces({) in a new line.

#### 22. Add curly braces even if there's only one line in the scope.

```cpp
if (bSomething)
{
	return;
}
```

#### 23. Use precision specification for floating point values unless there is an explicit need for a double.

```cpp
float f = 0.5f;
```

#### 24. Always have a default case for a switch statement.

```cpp
switch (number)
{
case 0:
	break;
default:
	break;
}
```

#### 25. Always add predefined FALLTHROUGH for switch case fall through unless there is no code in the case statement. This will be replaced by [[fallthrough]] attribute coming in for C++17 after.

```cpp
switch (number)
{
case 0:
	DoSomething();
	FALLTHROUGH
case 1:
	DoFallthrough();
	break;
case 2:
case 3:
	DoNotFallthrough();
default:
	break;
}
```

#### 26. If default case must not happen in a switch case, always add Assert(false). In our assert implementation, this will add optimization hint for release build.

#### 27. Use consts as much as possible even for local variable and function parameters.

#### 28. Any member functions that doesn't modify the object must be const.

```cpp
int GetAge() const;
```

#### 29. Do not return const value type. Const return is only for reference and pointers.

#### 30. Names of recursive functions end with Recursive.

```cpp
void FibonacciRecursive();
```

#### 31. Order of class variables and methods must be as follows:

    a. list of friend classes
    b. public methods
    c. protected methods
    d. private methods
    e. protected variables
    f. private variables

#### 32. Function overloading must be avoided in most cases

```cpp
// Use:
const Anim* GetAnimByIndex(const int index) const;
const Anim* GetAnumByName(const char* name) const;

// Instead of:
const Anim* GetAnim(const int index) const;
const Anim* GetAnum(const char* name) const;
```

#### 33. Overloading functions to add const accessible function is allowed.

```cpp
Anim* GetAnimByIndex(const int index);
const Anum* GetAnumByIndex(const int index);
```

#### 34. Avoid use of const_cast. Instead create a function that clearly returns an editable version of the object.

#### 35. Each class must be in a separate source file unless it makes sense to group several smaller classes.

#### 36. The filename must be the same as the name of the class including upper and lower cases.

```cpp
class PlayerAnimation;

PlayerAnimation.cpp
PlayerAnimation.h
```

#### 37. When a class spans across multiple files, these files have a name that starts with the name of the class, followed by an underscore and the subsection name.

```cpp
class RenderWorld;

RenderWorld_demo.cpp
RenderWorld_load.cpp
RenderWorld_portals.cpp
```

#### 38. Platform specific class for "reverse OOP" pattern uses similar nameing convention.

```cpp
class Renderer;

Renderer.h   // all renderer interfaces called by games
Renderer.cpp // Renderer's Implementations which are to all platforms

Renderer_gl.h   // RendererGL interfaces called by Renderer
Renderer_gl.cpp // RendererGL implementations
```

#### 39. Use our own version of Assert instead of standard c assert.

#### 40. Use assert for any assertion you have. Assert is not recoverable. This can be replaced by compiler optimization hint keyword \_\_assume for the release build.

#### 41. Any memory allocation must be done through our own New and Delete keyword.

#### 42. Memory operations such as memset, memcpy and memmove also must be done through our own MemSet, MemCpy and MemMove.

#### 43. Generally prefer reference(&) over pointers unless you need nullptr for any reason.

#### 44. Use pointers for out parameters. Also prefix the function parameters with out.

#### 45. The above out parameters must not be null. (Use assert, not if statement)

```cpp
void GetScreenDimension(uint32_t* const outWidth, uint32_t* const outHeight)
{
	Assert(outWidth);
	Assert(outHeight);
}
```

#### 46. Use pointers if the parameter will be saved internally.

```cpp
void AddMesh(Mesh* const mesh)
{
	mMeshCollection.push_back(mesh);
}
```

#### 47. Use pointers if the parameter should be generic void\* parameter.

```cpp
void Update(void* const something)
{
}
```

#### 48. The name of a bitflag enum must be suffixed by Flags.

```cpp
enum class eVisibilityFlags
{
};
```

#### 49. Do not add size specifier for enum unless you need that specific size (e.g, for serialization for data members)

```cpp
enum class eDirection : uint8_t
{
	North,
	South
};
```

#### 50. Prefer overloading over default parameters

#### 51. When default parameters are used, restrict them to natural immutable constants such as nullptr, false or 0.

#### 52. Prefer fixed-size containers whenever possible.

#### 53. reserve() dynamic containers whenever possible.

#### 54. Always put parentheses for defined numbers

```cpp
#define NUM_CLASSES (1)
```

#### 55. Prefer constants over defines

#### 56. Always use forward declaration if possible instead of using includes.

#### 57. All compiler warnings must be addressed.

#### 58. Put pointer or reference sign right next to the type.

```cpp
int& number;
int* number;
```

#### 59. Shadowed variables are not allowed.

```cpp
class SomeClass
{
public:
	int32_t Count;

public:
	void Func(const int32_t Count)
	{
		for (int32_t count = 0; count != 10; ++count)
		{
			// Use Count
		}
	}
}
```

#### 60. Declare only one variable per line.

```cpp
// BAD:
int counter, index;
// GOOD:
int counter;
int index;
```

#### 61. Do not use const member variables in a struct or class simply to prevent modification after initialization. Same rule is applied to reference(&) member variables.

#### 62. Take advantage of NRVO, when you are returning a local object. This means you need to have only one return statement inside your function. This applies only when you return an object by value.

## 2. Modern Language Features

#### 1. override and final keywords are mandatory.

#### 2. Use enum class always.

```cpp
enum class eDirection
{
	North,
	South
};
```

#### 3. Use static_assert over Assert, if possible.

#### 4. Use nullptr over NULL.

#### 5. Use unique_ptr when a object lifetime is solely handled inside a class. (i.e. new in constructor delete in destructor)

#### 6. Range-based for are recommended where applicable.

#### 7. Do not use auto unless it is for a iterator or new keyword is on the same line, showing which object is created clearly.

#### 8. Do not manually perform return value optimization using std::move. It breaks automatic NRVO optimization.

#### 9. Move constructor and move assignment operator are allowed.

#### 10. Use constexpr instead of const for simple constant variables.

```cpp
// Use:
constexpr int DEFAULT_BUFFER_SIZE = 65536;

// Instead of:
const int DEFAULT_BUFFER_SIZE = 65536;
```
