# Module - 2 : The Tuples with functions

*What was missing in tuples which we read in Module - 1?*

*They were like 'C' language structures without functions. A C++ object has functions in it. This module demonstrates how to add functions to the std::tuple*

### 2.1 : Adding functions in std::tuple

*The standard library std::functions<> can be part of a tuple and functions can be defined as lambdas.*
```
std::tuple<int, int, function<int(int, int)>> tWithFn { 10, 20, [](int a, int b) ->int { return a + b; } };

```
*Here __tWithFn__ contains two integers and a functions. The function defined in the tuple can be called as*

```
int result = get<2>(tWithFn)(10,20);  // Direct Call
auto TupleFn = get<2>(tWithFn); // Taking into std::function
int result = TupleFn(100, 200); // calling the function

// calling the function with tuple data members
int result = TupleFn(get<0>(tWithFn), get<1>(tWithFn));
```
### 2.2 : Using std::tuple data members in tuple functions

*Though we can call tuple functions, but there seems to be a serious limitations. In the last example above, we need to manually pass the parameters of tuples. However, many a times we'd like it to be implicit as happens with struct of classes*

*Well, in template meta programming, its not possible to refer a template argument N-1 at N. so we can't do it in place. However, its possible to do the same by changing the function definition after defining it while creating tuples. Here is how we'll do it*

* 1 - Define the tuple with function and provide the dummy implementation of the function
```
std::tuple<int, int, function<int(void)>> tWithFn{ 10, 20, []() ->int { return -1; } };
```
* 2 - Change function definition after that to reflect the actual usage of tuple members
```
get<2>(tWithFn) = []() ->int { return get<0>(tWithFn) + get<1>(tWithFn); }
// This code will not compile
```
* 3 - To compile the code, we need to specify pass by value in lambda capture list. we can pass individual variables or use __'='__ to denote all
```
get<2>(tWithFn) = [=]() ->int { return get<0>(tWithFn) + get<1>(tWithFn); };
```
* 4 - We can now use this function as we did earlier.
```
auto TupleFn = get<2>(tWithFn);
int result = TupleFn();
```

###2.3 : The function programming : Immutability of tuple data

*In the above example, we cannot modify the value of tuple unless we pass by reference __'&'__ , thereby it preserve the data from being modified, something what const will do a C++ struct or classes. However, this limitation could actually be bypassed by using mutable*



