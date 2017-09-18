# 연관 컨테이너

 * 일정규칙에 따라 자료를 조직화하여 저장(set / map / multiset / multimap 등)
    * 자료를 정렬하여 저장하기 떄문에 검색 유리
        * 많은양의 자료 / 빠른검색

    * 입력에 있어서 느림
        * 균형 이진트리기 때문에 느림

# set Container

```cpp
template <class Key,   
    class Traits=less<Key>,   
    class Allocator=allocator<Key>
    >  

class set  

```

 * set은 push_back(), push_front(), pop_front(), pop_back()와 큰 원소의 참조 멤버함수 front(), back()을 제공하지 않는다.

 * 연관 컨테이너는 균형 이진 트리를 사용하므로 찾기 연산 에서 로그시간의 성능을 보이며, 삽입또한 로그시간 복잡도를 보인다.

 * 중복 원소를 허용하지 않는다.

```cpp

    std::set<int> s;
    
    s.insert(50);
    s.insert(30);
    s.insert(80);
    s.insert(40);
    s.insert(10);
    s.insert(70);
    s.insert(90);

    // 1. s iterator를 통해 순서대로 출력한다면 ?

    s.insert(50);
    s.insert(30);

   // 2. s iterator를 통해 순서대로 출력한다면 ?  



```

```cpp
    
    std::pair<std::set<int>::iterator, bool> pr;
    
    pr = s.insert(50);
    
    std::cout<<*pr.first<<" , "<<pr.second<<std::endl;

    // 3. 과연 출력값은?

    s.insert(pr.first, 85);
    
    std::set<int>::iterator iter;
    
    for (iter = s.begin(); iter != s.end(); ++iter) 
    {
        std::cout<< *iter <<", ";
    }

    // 4. 과연 출력값은?
```
* find

```cpp
    iter = s.find(30);
    //iter는 30의 key가진 node 를 가리키고 있다.

    // 만약 찾지 못한다면 무엇을 가리키고 있는가?
```
```
//find(iter) 구현부
!(*iter <30)&&!(30<*iter)

```

* lower_bound, upper_bound, erase

```cpp

int main ()
{
  std::set<int> myset;
  std::set<int>::iterator itlow,itup;

  for (int i=1; i<10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90

  itlow=myset.lower_bound (30);                // 어딜 가리키고 있는가?
  itup=myset.upper_bound (60);                 //                   ?

  myset.erase(itlow,itup);                     // 뭐가 지워 졌을까?

  std::cout << "myset contains:";
  for (std::set<int>::iterator it=myset.begin(); it!=myset.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

# multiset 컨테이너
 > 중복 원소를 컨테이너에 저장할 수 있다는 것 외에는  set과 다른것이 없다.


```cpp

// multiset::insert (C++98)
#include <iostream>
#include <set>

int main ()
{
  std::multiset<int> mymultiset;
  std::multiset<int>::iterator it;

  // set some initial values:
  for (int i=1; i<=5; i++) mymultiset.insert(i*10);  // 10 20 30 40 50

  it=mymultiset.insert(25);

  it=mymultiset.insert (it,27);    // max efficiency inserting
  it=mymultiset.insert (it,29);    // max efficiency inserting
  it=mymultiset.insert (it,24);    // no max efficiency inserting (24<29)

  int myints[]= {5,10,15};
  mymultiset.insert (myints,myints+3);

  std::cout << "mymultiset contains:";
  for (it=mymultiset.begin(); it!=mymultiset.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

# map

```cpp
    template < class Key,                                     // map::key_type
           class T,                                       // map::mapped_type
           class Compare = less<Key>,                     // map::key_compare
           class Alloc = allocator<pair<const Key,T> >    // map::allocator_type
           > 
           
    class map;
```

* insert

```cpp
    // map::insert (C++98)
#include <iostream>
#include <map>

int main ()
{
  std::map<char,int> mymap;

  // first insert function version (single parameter):
  mymap.insert ( std::pair<char,int>('a',100) );
  mymap.insert ( std::pair<char,int>('z',200) );

  std::pair<std::map<char,int>::iterator,bool> ret;
  ret = mymap.insert ( std::pair<char,int>('z',500) );
  if (ret.second==false) {
    std::cout << "element 'z' already existed";
    std::cout << " with a value of " << ret.first->second << '\n';
  }

  // second insert function version (with hint position):
  std::map<char,int>::iterator it = mymap.begin();
  mymap.insert (it, std::pair<char,int>('b',300));  // max efficiency inserting
  mymap.insert (it, std::pair<char,int>('c',400));  // no max efficiency inserting

  // third insert function version (range insertion):
  std::map<char,int> anothermap;
  anothermap.insert(mymap.begin(),mymap.find('c'));

  // showing contents:
  std::cout << "mymap contains:\n";
  for (it=mymap.begin(); it!=mymap.end(); ++it)
    std::cout << it->first << " => " << it->second << '\n';

  std::cout << "anothermap contains:\n";
  for (it=anothermap.begin(); it!=anothermap.end(); ++it)
    std::cout << it->first << " => " << it->second << '\n';

  return 0;
}

```

* map operator[]
```cpp
// map::insert (C++98)
#include <iostream>
#include <map>

int main ()
{
  
  mymap['a']="an element";
  mymap['b']="another element";
  mymap['c']=mymap['b'];

}
```

# multimap
 > 중복 key를 허용한다. 

* insert
```cpp
int main ()
{
  std::multimap<char,int> mymultimap;
  std::multimap<char,int>::iterator it;

  // first insert function version (single parameter):
  mymultimap.insert ( std::pair<char,int>('a',100) );
  mymultimap.insert ( std::pair<char,int>('z',150) );
  it=mymultimap.insert ( std::pair<char,int>('b',75) );

  // second insert function version (with hint position):
  mymultimap.insert (it, std::pair<char,int>('c',300));  // max efficiency inserting
  mymultimap.insert (it, std::pair<char,int>('z',400));  // no max efficiency inserting

  // third insert function version (range insertion):
  std::multimap<char,int> anothermultimap;
  anothermultimap.insert(mymultimap.begin(),mymultimap.find('c'));

  // showing contents:
  std::cout << "mymultimap contains:\n";
  for (it=mymultimap.begin(); it!=mymultimap.end(); ++it)
    std::cout << (*it).first << " => " << (*it).second << '\n';

  std::cout << "anothermultimap contains:\n";
  for (it=anothermultimap.begin(); it!=anothermultimap.end(); ++it)
    std::cout << (*it).first << " => " << (*it).second << '\n';

  return 0;
}

```



