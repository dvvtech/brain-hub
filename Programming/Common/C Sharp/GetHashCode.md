```cs
var p1 = new Person { Age = 1, FirstName= "123" };
var p2 = new Person { Age = 1, FirstName = "123" };
var v1 = p1.GetHashCode();
var v2 = p2.GetHashCode();
//v1 != v2

string s1 = "xyz";
string s2 = "xyz";
var v1= p1.GetHashCode();
var v2 = p2.GetHashCode();
//v1 == v2 из за того что указывают на одну и туже область памяти
```