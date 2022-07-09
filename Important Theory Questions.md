# Important Theory Questions

### Difference between abstract classes and interfaces

***ABSTRACT CLASSES***

In C++, a class becomes abstract if it has even one *pure - virtual function*. Virtual functions are equivalent to abstract classes. In java, *abstract* keyword is needed. We can have abstract classes and methods but not varibales. 
```
abstract class Colour{
    public : 
        int id;
        abstract void fillColor();
 };
 ```
 
 Features  :
  - Objects cannot be created.
  - ***final*** methods cannot be present since abstract & final will give error together. 
  > Final = impossible to inherit / override
  - Constructors are allowed. They will be called when an object of the child class is created.
  ``` 
  abstract class Base {
      public : 
          Base() {
              cout<<"Base constructor called";
           }
           void func();
  }
  
  class Child extends Base {
      public : 
          Child() {
              cout<<"Child contrcutor called";
          }
          void func(){
              cout<<"I am having fun!";
          }
   }
   
   void main() { 
        Child c;
        c.func();
   }
   ```
   
   On creating the object, the parent constructor will be called first and then the child. 
   
   ```
   Base constructor called
   Child contrcutor called
   I am having fun!
   ```
  - Static methods can be included
  > Static methods are part of the class definition and not the objects. They can be accessed with or even without objects. Most common use of static methods is to declare static variables.
  > Constructors are called immediately when an object is created but static has nothing to do with objects and can be called whenever we wish for it.

  - The class **HAS** to be decalred abstract if it consists of even a single abstract method.
  - Child classes have to **compulsorily** implement abstract methods.
  
  
***INTERFACES***
In Java, a mechanism to achieve abstraction. 
- only method declaration, no definition
- blueprint of class (what should a class do, not how)
- methods are abstract by default
- all methods **must** be implemented
- cannot instantiate 

![image](https://user-images.githubusercontent.com/107466664/178102756-e077702b-9d3b-476a-9ee0-dd68eba745e9.png)

