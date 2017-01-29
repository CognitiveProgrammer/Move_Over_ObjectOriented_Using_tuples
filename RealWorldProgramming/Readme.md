# Module - 3 : Using Tuples in Real World Programming

## 3.1 Generic functions using Templates

_C++ Templates are used for generic programming, which means we can leave it to the C++ compilers to generate the code for us. one of the most widely used example, which is used to demonstrate the power of generic function is writing a_ __SUM__ _function which is used for summing multiple data types_

```
template<typename retType>
retType SUM (retType first, retType second) {
    return first + second;
}
int main() {
    cout<<SUM<int>(10,20)<<endl;
    cout<<SUM<float>(10.5,20.6)<<endl;
    cout<<SUM<char>(10,60)<<endl;  
    return 0;
}

```
_Similar generic functions can also be added as part of a C++ class as_
```
template<typename retType>
class GenClass {
    retType aVal;
    retType bVal;
public:
    retType SUM (retType first, retType second) {
        return first + second;
    }
};
int main() {
    cout<<GenClass<int>().SUM(10,20)<<endl;
    cout<<GenClass<float>().SUM(10.5,20.34)<<endl;
    cout<<GenClass<char>().SUM(71,26)<<endl;  
    return 0;
}
```
_However, what if we want to write generic functions for subtractions and multiplications._

_We need to add those functions in the class as_

```
template<typename retType>
class GenClass {
    retType aVal;
    retType bVal;
public:
    retType SUM (retType first, retType second) { return first + second; }
    retType SUB (retType first, retType second) { return first - second; }
    retType MUL (retType first, retType second) { return first * second; }
};
int main() {
    cout<<GenClass<int>().SUM(10,20)<<endl;
    cout<<GenClass<int>().SUB(56,90)<<endl;
    cout<<GenClass<int>().MUL(71,26)<<endl;    
    return 0;
}

```

_Std::tuple can't generate generic classes (as above (exception exists)), but its actually useful in the situations like above, when you need multiple functions. Remember, there is a limit to generics in the class above, if I instantiate the class with_ __int__ _the functions_ __SUM, SUB & MUL__ _will be instantiated with_ __int__

## 3.2 Generics Using std::tuple

_As with classes, std::tuple can also create user defined types with data types as well as functions. std::tuple can contain std::function where we can provide the function definition while instantiation.

For example, let's see the tuple below which is created for the same purpose we created_ `GenClass` _for_

```
typedef tuple <int,int,function<int(int,int)>> myTuple;
```
_Since all the functions like_ `SUM, SUB & MUL` _takes the same number of parameters and have same return value, we can create a function pointer (using C++11 std::function) to hold the pointer._

_With the arrival of lambdas, it possible to define the function while create an instance of tuple_ `myTuple` _we can also then use the values to create the SUM_

```
typedef tuple <const int,const int,function<int(int,int)>> myTuple;
int main() {
    myTuple mtAdd {100,200,[](int a, int b) { return a + b;}};
    cout<<get<2>(mtAdd)(get<0>(mtAdd), get<1>(mtAdd))<<endl;
    return 0;
}
```
_so we got the SUM, lets park the weird syntax for a while.

The way we've created SUM function, we can similarly create SUB and MUL_
```
myTuple mtSub {100,200,[](int a, int b) { return a - b;}};
cout<<get<2>(mtSub)(get<0>(mtSub), get<1>(mtSub))<<endl;

myTuple mtMul {100,200,[](int a, int b) { return a * b;}};
cout<<get<2>(mtMul)(get<0>(mtMul), get<1>(mtMul))<<endl;
```

## 3.3 Some Benefits of using tuple over classes in this case  

_Having assigned designated functions at the beginning, you can't use_ `mtAdd` _instance for subtractions or use_ `mtSub` _for additions. Same is true for multiplications.

However, it possible to use_ `mtMul` _for multiplying values of_ `mtAdd`
```
cout<<get<2>(mtMul)(get<0>(mtAdd), get<1>(mtAdd))<<endl;  
```

### So with std::tuple, we can bind different functions to different instances.

_Its also possible to change the function definition at later point of time. for example, in SUM function can be changed to add a constant later on._
```
typedef tuple <int,int,function<int(int,int)>> myTuple;

int main() {
  myTuple mtAdd {100,200,[](int a, int b) { return a + b;}};
  cout<<get<2>(mtAdd)(get<0>(mtAdd), get<1>(mtAdd))<<endl;

// Change the function definition at run time
  get<2>(mtAdd) = [](int a, int b) { return a + b + 100; };

  cout<<get<2>(mtAdd)(get<0>(mtAdd), get<1>(mtAdd))<<endl;

  return 0;
}

```
### So with std::tuple, We can also change the function definition at runtime

__NOTE: Same can be done with classes if we use function pointers instead of function definitions. However, tuple provides more control and access to it__

### Binding of values to the functions

_Since we can change the function definition at runtime, its possible to assign the tuple values. For example, we can change the add function to use the values of tuple._

```
typedef tuple <int,int,function<int()>> myTuple;

int main() {
    // First create a dummy implementation
    myTuple mtAdd {100,200,[]() { return -1;}};
    // then add the functions to use the value of tuple
    get<2>(mtAdd) = [=]() -> int { return get<0>(mtAdd), get<1>(mtAdd); };
    cout<<get<2>(mtAdd)()<<endl;
    return 0;
}
```
__NOTE: To use tuple variables inside lambdas, we need to provide lambda access specifier ('=' as pass by value)__

_What will happen if we change the value of variables in tuple and run the function again_
```
typedef tuple <int,int,function<int()>> myTuple;

int main() {
    // First create a dummy implementation
    myTuple mtAdd {100,200,[]() { return -1;}};
    // then add the functions to use the value of tuple
    get<2>(mtAdd) = [=]() -> int { return get<0>(mtAdd), get<1>(mtAdd); };
    cout<<get<2>(mtAdd)()<<endl;

    get<0>(mtAdd) = 5000;
    cout<<get<2>(mtAdd)()<<endl;
    return 0;
}
```
_Surprisingly, the output will remain same. This is because we've used '=' as lambda specifier which pass by value. if we want the change to reflect the same then we need to pass by reference as explained below_
```
get<2>(mtAdd) = [&]() -> int { return get<0>(mtAdd), get<1>(mtAdd); };

cout<<get<2>(mtAdd)()<<endl;

get<0>(mtAdd) = 5000;
// The new changes will reflect.
cout<<get<2>(mtAdd)()<<endl;

```
