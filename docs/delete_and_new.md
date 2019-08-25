## Delete object correctly
> The **new** operator denotes a request for memory allocation on the **heap**; The **delete** operator denotes a request for memory deallocation on the **heap**.
> 
> Exception:
> ```cpp
> char ch[100];
> int main()
> {
>    int* ptr = new (ch) int[25];
>    delete [] ptr;
> }
> ```
> error for object 0x---------: pointer being freed was not allocated.

### Use correct new/delete pairs
- Non-array

   `new delete `
- Array

   `new []  delete []`

### Delete a polymorphic object with a virtual destructor

- Noncompliant code example
```cpp
struct Base { 
    virtual void f(); 
}; 
struct Derived : Base {}; 
void f() { 
   Base *b = new Derived(); 
   // ... delete b; 
}
```
> b is a polymorphic pointer type whose **static type** is Base * and whose dynamic type is **Derived**. 
```cpp
struct Base {
   virtual ~Base() = default; 
   virtual void f();
}; 
struct Derived : Base {}; 
void f() {
   Base *b = new Derived(); 
   // ... delete b;
}
```
> If a class has no user-declared destructor, a destructor is implicitly declared as defaulted. An implicitly declared destructor is an **inline public** member of its class.
