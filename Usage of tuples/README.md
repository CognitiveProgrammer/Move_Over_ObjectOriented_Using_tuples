# Module - 1 : The basic usage of std::tuple

*Tuples (std::tuple) is a container which allow hetrogeneous data types. Though we can achieve the same with std::pair<>, but its limited to only 2 data members. std::tuple<> has no such limitations.*

## 1.1 C++ classes and structs

*In C++, __Structs__ and __Classes__ are used for creating used defined types a.k.a Objects.
An object in* **OOP** *contains state (represented by member variables) and corresponding Behaviours (represented by member functions).*

*The object also contains implicit member functions like  __constructors__ , __destructors__ , __copy constructors__ , and __assignment operators__ , unless written explicitly. for example, let create a user defined type as*

```
struct Udt {
    int integer;
    char character;
    float floatingpoint;
};
```
## 1.2 Introducing C++ Tuples

*The user defined type could also be created using the std::tuples*

```
typedef tuple<int, char, float>  Tp;
```
### 1.2.1 Initializing of Tuples
*The tuples can be initialized in multiple ways, it can also be initialized using standard library function specifically created for creating tuples.*

```
Tp tpOne(1,'c',2.5);
Tp tpTwo = make_tuple(10,'c',10.05);
Tp tpThree { 2, 'd', 2.6 };
```
### 1.2.2 Accessing tuple members
*Tuples can be accessed using* **std::get** *functions*

```
cout<<get<0>(tpOne)<<endl; // Accessing first member
cout<<get<1>(tpOne)<<endl; // Accessing second member
cout<<get<2>(tpOne)<<endl; // Accessing third member

```
*The access can also result in changing of values in the created tuples*
```
get<0>(tpOne) = 100;
get<1>(tpOne) = 'N';
get<2>(tpOne) = 100.95;
```
*The change in values in any tuples after creation can be prevented by using const*
```
typedef tuple<const int, const char, const float>  Tp;
Tp tpOne(1,'c',2.5);
// The following statement is not allowed and will provide compiler errors
get<0>(tpOne) = 100;
get<1>(tpOne) = 'N';
get<2>(tpOne) = 100.95;

```
# 1.3 Some benefits of using tuples over structs and classes
*With struct and classes, you need to know number of elements and types of elements, however with tuples, it can be known using standard library functions*
```
typedef tuple<int, char, float>  Tp;
Tp tpOne(1,'c',2.5);

cout<<"Number of elements in Tuple "<<tuple_size<Tp>::value<<endl;
cout<<"Type of First element "<<typeid(get<0>(tpOne)).name()<<endl;
cout<<"Type of First element "<<typeid(get<1>(tpOne)).name()<<endl;
cout<<"Type of First element "<<typeid(get<2>(tpOne)).name()<<endl;

```

## 1.4 Some things to take note of

*Tuples will not have Initializing functions like constructors and destructors, however initialization is must while creating tuples and C++ smart pointers like shared_ptr<> should be used for creating dynamic memory, for example*
```
struct Udt {
    Udt() { cout<<"Cons..."<<endl; }
    ~Udt() { cout<<"Dest.."<<endl; }
};
int main() {
  typedef tuple<int,shared_ptr<Udt>,float>  Tp;
  Tp tpOne = make_tuple(10,make_shared<Udt>(),10.05);
}
```
*The use of assignment operators can be used easily to copy the content of one tuple to another*
```
typedef tuple<int,shared_ptr<Udt>,float>  Tp;
Tp tpOne = make_tuple(10,make_shared<Udt>(),10.05);
Tp tpTwo = tpOne;
// or can be used as reference
Tp &tpThree = tpOne;
```
## 1.5 Usage of Tuples inside a Tuples
*The std::tuple can be used inside a tuple just like any other container. This can be used to achieve something similar to the inheritance mechanism as shown below*

```
// Tuples inheritance
typedef tuple<int, int> tpFirst;
typedef tuple<tpFirst, char> tpSecond;

// Creation of tuple
tpSecond tupleC  = make_tuple( make_tuple(1,2),'c' );

// Accessing individual members
cout<<get<0>(get<0>(tupleC))<<endl;
cout<<get<1>(get<0>(tupleC))<<endl;

cout<<get<1>(tupleC)<<endl;

```

## 1.6 Conclusion

*Tuples are a great way of creating better User Defined types. With its current functionality, it prevents us from
multiple problematic scenarios which comes when using the normal C++ structs and classes. Notably among knowing
the members and types.*

*Furthermore, tuples can also be part of other STL container, which make things easier further*
