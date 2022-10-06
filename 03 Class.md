# Class
A combination of data and functions.
Visibility: private(by default), protected, public
``` cpp
class Player 
{
public:
    int x, y;
    int speed;
    
    void Move(int xa, int ya)
    {
        x += xa * speed;
        y += ya * speed;
    }
};

int main() {
    Player player;
    player.x = 5;
    player.Move(1, -1);
}
```

# Comparison of class and struct
Struct in C++: maintain the compatibility with C.   
An example of struct in Plain Old Data(POD)
``` cpp
struct Vec2 
{
    float x, y;
    
    void Add(const Vec2& other)
    {
        x += other.x;
        y += other.y;
    }
};
```
Use class in inheritance.

# How to write a C++ class
``` cpp
#include <iostream>

class Log
{
public:
    const int LogLevelError = 0;
    const int LogLevelWarning = 1;
    const int LogLevelInfo = 2;
    
private:
    int m_LogLevel = LogLevelInfo;
    
public:
    void SetLevel(int level)
    {
        m_LogLevel = level;
    }
    
    void Error(const char* message)
    {
        if (m_LogLevel >= LogLevelError)
            std::cout << "[ERROR]" << message << std::endl;
    }
    
    void Warn(const char* message)
    {
        if (m_LogLevel >= LogLevelWarning)
            std::cout << "[WARNING]" << message << std::endl;
    }
    
    void Info(const char* message)
    {
        if (m_LogLevel >= LogLevelInfo)
        std::cout << "[INFO]" << message << std::endl;
    }
};

int main() 
{
    Log log;
    log.SetLevel(log.LogLevelWarning);
    log.Warn("Hello!");
    log.Error("Hello!");
    log.Info("Hello!");
}
```

# static in C++

1. static outside of struct/class: The linkage of the symbol that declared as static is internal, only visible to the defined translation unit.
2. static inside of struct/class: This variable shares memory with all of the instances of the class. Basically, across all the created instances of struct/class, only one instance of that static variable.

C++ cannot have two global variables with the same name. Solutions:
1. One of the variable define as static.
2. One of the variable set as the reference of another.
3. external linking/linkage: One of the variable not assigned with value and define with extern keyword.

For solution 3, if a variable is defined as static, then it cannot be found by extern variable.

# static inside of class/struct

For the following code, get **unresolved external symbols** error if x, y are stated as static.
``` cpp
#include <iostream>

struct Entity
{
    int x, y;
    
    void print()
    {
        std::cout << x << ", " << y << std::endl;
    }
};

int main()
{
    Entity e1;
    e1.x = 2;
    e1.y = 3;
    
    Entity e2;
    e2.x = 5;
    e2.y = 8;
    
    e1.print();
    e2.print();
}
```

x, y need to be declared outside of struct.
``` cpp
#include <iostream>

struct Entity
{
    static int x, y;
    
    void print()
    {
        std::cout << x << ", " << y << std::endl;
    }
};

int Entity::x;
int Entity::y;

int main()
{
    Entity e1;
    e1.x = 2;
    e1.y = 3;
    
    Entity e2;
    e2.x = 5;
    e2.y = 8;
    
    e1.print();
    e2.print();
}
```
static method cannot access non-static variable. (illegal reference to non-static member)
Reason: static method does not have a class instance.

# static in local scope
When defining a variable, consider
1. Lifetime: how long will it remain in memory before being deleted.
2. Scope: where can access that variable.

static local variable: declare a variable to have the lifetime of the entire program, but scope limited(in a function/class).

``` cpp
void function()
{
    static int i = 0;
    i++;
    std::cout << i << std::endl;
}
```

``` cpp
#include <iostream>

class Singleton
{
private:
    static Singleton*  sInstance;
public:
    static Singleton& get()
    {
        return *sInstance;
    }
    
    void hello()
    {
        std::cout << "hello" << std::endl;
    }
};

Singleton* Singleton::sInstance = nullptr;

int main()
{
    Singleton::get().hello();
}
```

#  enumeration
By default, A equals 0, B equals 1, C equals 2.
``` cpp
enum Example
{
    A,
    B,
    C
};

int main()
{
    Example value = A;
}
```

A equals 5, B equals 6, C equals 7.
``` cpp
enum Example
{
    A = 5,
    B,
    C
};
```

Can also be assigned with value.
``` cpp
enum Example
{
    A = 5,
    B = 1,
    C = 2
};
```

enum can be a certain type of integer.
``` cpp
enum Example : unsigned char
{
    A = 5,
    B = 1,
    C = 2
}
```

# constructor
Example:
```
class Entity
{
public:
    float X, Y;

    Entity()
    {

    }

    Entity(float x, float y)
    {
      X = x;
      Y = y;
    }
    void print() 
    { 
      std::cout << X << ',' << Y << std::endl; 
    }
};

int main() 
{
    Entity e(10.0f, 5.0f);
    e.print();
}
```

# Destructor
Destructor applies to both stack and heap allocate objects. Prevent memory leakage.
For heap, destructor is called when delete.  
For stack, destructor is called at the end of scope.  
``` cpp
class Entity
{
public:
    float X, Y;

    Entity()
    {
        X = 0.0f;
        Y = 0.0f;
        std::cout << "Created Entity!" << std::endl;
    }

    ~Entity()
    {
        std::cout << "Destroyed Entity!" << std::endl;
    }

    void print() 
    { 
        std::cout << X << ',' << Y << std::endl; 
    }
};

void Function()
{
    Entity e;
    e.print();
}

int main() 
{
    Function();
}
```

# inheritance

``` cpp
class Entity
{
public:
    float X, Y;

    Entity()
    {
        X = 0.0f;
        Y = 0.0f;
    }

    void move(float ax, float ay) 
    { 
        X += ax;
        Y += ay;
    }
};

class Player : public Entity
{
public:
    const char* name;

    void printName()
    {
        std::cout << name << std::endl;
    }
};
```

# virtual functions
B class derived from A calss, a function in A is marked as virtual, then can override this method in B class.


