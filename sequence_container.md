 # <strong><font color='navy'>시퀀스 컨테이너</font> </strong> #

> 시퀀스 컨테이너 : 저장원소가 삽입 순서에 따라 상대적인 위치를 갖는 컨테이너


## <strong> Vector 컨테이너 </strong>

> 1. Vector는 대표적인 시퀀스 컨테이너다. 
> 2. 다른 컨테이너의 주요 멤버도 vector와 별반 다르지 않으므로 자세히 알아두면 좋다.
> 3. Vector는 단점은 삽입, 삭제가 느리다. 


### <strong> Q : size와 capacoty의 차이는 ?</strong> 

>  Drag here

<font color='white'> 
size는 원소의 개수
capacity는 할당된 메모리의 크기가 <- vector에만 있다.
</font>

>  Drag here


### <strong> Q : 왜 느릴까요 ?</strong> 

>  Drag here

<font color='white'> 
Vector는 크기가 가변적이기 때문에, <br>
할당된 메모리 공간보다 더 많은 값 삽입시 메모리 공간을 재할당한다.<br>
재할당은 비용이 비싼 작업<br>
순서 : 사이즈 체크 , 새로운 메모리공간 부여 (큰사이즈), 기존벡터 삽입, 기존벡터 소멸, 값 넣어주기<br>

새로운 메모리공간의 크기는 컴파일러 마다 조금씩 다르다, (ex: 이전 메모리 1/2, 2배 etc..)
</font>

>  Drag here

### <strong> Vector의 템플릿 형식 </strong>
 ```c++
template<typename T, typename Allocator = allocator<T>>
class vector 
 ```

* T는 벡터에 저장되는 요소의 타입이며 벡터는 이 타입의 집합을 관리한다. 
* 두 번째 인수 Allocator는 내부적인 메모리 관리에 사용되는 할당기인데 디폴트가 제공되므로 생략 가능하다. 
* 특별한 경우가 아닌 한은 디폴트 할당기를 사용하므로 벡터의 인수로는 주로 요소의 타입만이 전달된다.



<strong> Vector 생성자 </strong>
 ```c++
vector v 
// 빈 컨테이너다

vector v(n) 
// 기본값으로 초기화된 n개의 원소를 갖는다.

vector v(n,x)
// x 값으로 초기화된 n개의 원소를 갖는다.

vector v(v2)
// v2 컨테이너의 복사본

vector v(b,e)
// 반복자 구간 [b,e) 로 초기화된 원소를 갖는다.
// 반복자 타입을 사용

 ```
 ### <strong> Q1 : 각 Vector의 내부는 ?</strong> 
 ```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;
 
void main()
{
     int ar[]={1,2,3,4,5,6,7,8,9};
    
     vector<string> v1;
     vector<double> v2(10);
     vector<int> v3(10,7);
     vector<int> v4(v3);
     vector<int> v5(&ar[2],&ar[5]);
     vector<int> v6(ar[2],ar[5]);
     vector<int> v7(v3.begin(),v3.end());
}


 ```
>  Drag here

<font color='white'> 
 v1 = <br>
 v2 = 0000000000<br>
 v3 = 7777777777<br>
 v4 = 7777777777<br>
 v5 = 345<br>
 v6 = 666<br>
 v7 = 7777777777<br>
</font>

>  Drag here

 ### <strong> Q2 : Vector는 객체를 어떻게 생성할까? </strong> 
 ```c
 vector<int> v3(10,7);
 ```
>  Drag here

<font color='white'> 
객체 생성 후 capacity를 확보한 후 반복하여 값을 삽입

속도를 높이기 위하여 최초 하나의 원소를 생성 후 나머지 n-1는 복사생성자를 호출하여 생성한다.
</font>

>  Drag here


 ### <strong> Q3 : 다음 구문 수행시 출력될 값은? </strong> 
 ```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;



void main()
{
	int ar[]={1,2,3,4,5,6,7,8,9};

	vector<int> aa(&ar[0],&ar[9]);

	cout << aa.size() << endl;
	cout << aa.capacity() << endl;

	vector<int>::iterator bb = aa.erase(aa.begin());
	cout << *bb << endl;

	cout << aa.size() << endl;
	cout << aa.capacity() << endl;
    
    /* something more Q */
    cout << *aa.begin()+10 <<endl;


    /* something more Q */
    // aa.clear() 하면 size와 capacity는 ? 

}
 ```
>  Drag here

<font color='white'> 
9
9
2
8
9

11
</font>

>  Drag here

 ### <strong> Q4 : 다음 구문 수행시 출력될 값은? </strong> 
 ```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

void main()
{
	int ar[]={9,2,3,4,5,6,7,8,9};
	int ar2[]={9,3};

	vector<int> aa(&ar[0],&ar[9]);

	vector<int> bb(&ar2[0],&ar2[9]);

	if(aa<bb)
	{
		cout<<"is big?"<<endl;
	}
}
 ```
>  Drag here

<font color='white'> 
is big?

문자열 비교처럼 대소 비교를 함<br>
원소를 하나씩 비교하여 조건을 만족하는 값이 있으면 true/false를 return

</font>

>  Drag here



 ### <strong> Q5 : 다음 구문 수행시 출력될 값은? </strong> 
 ```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

void main()
{
	int ar[]={1,2,3,4,5,6,7,8,9};

	vector<int> aa(&ar[0],&ar[9]);

	
	cout << aa.size() << endl;
	cout << aa.capacity() << endl;

    aa.resize(5);

	cout << aa.size() << endl;
	cout << aa.capacity() << endl;
    

	
}
 ```
>  Drag here

<font color='white'> 
9<br>
9<br>
5<br>
9<br>
</font>

>  Drag here


 ### <strong> Q6 : (SWAP) 다음 구문 수행시 출력될 값은? </strong> 
 ```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;


void main()
{
	int ar[]={9,2,3,4,5,6,7,8,9};
	int ar2[]={9,3};

	vector<int> aa(&ar[0],&ar[9]);

	vector<int> bb(&ar2[0],&ar2[9]);
	cout << aa.size() << endl;
	cout << aa.capacity() << endl;
	
	
	aa.swap(bb);

	cout << aa.size() << endl;
	cout << aa.capacity() << endl;

	cout << aa[2]<<endl;

}
 ```
>  Drag here

<font color='white'> 
9<br>
9<br>
9<br>
9<br>
쓰레기값
</font>

>  Drag here

 ### <strong> Q7 : [] 와 at() 의 차이점은? </strong> 

```
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void main()
{
	int ar[]={9,2,3,4,5,6,7,8,9};
	vector<int> aa(&ar[0],&ar[9]);

    try
    {
        /* mini Q */
        // 둘중 try, catch에 잡히는 출력문은?
        cout << aa.at(-1) <<endl;
        cout << aa[-1] <<endl;
    }

    catch (out_of_range e)
    {
        cout << e.what() << endl;
    }

}


```
>  Drag here
<font color='white'> 

기능이나 결과는 같지만,
범위 점검을 하느냐 하지 않느냐의 차이가 있다.<br>
[]는 점검을 안하고
at()은 범위 점검을 해서 0<= i <size 면 값을 받환, 아니면 out_of_range 예외 발생
</font>

>  Drag here


 ### <strong> Q8 : 각 멤버함수별 차이점 </strong> 


```
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void main()
{
	int ar[]={1,2,3,4,5,6,7,8,9};
	vector<int> aa(&ar[0],&ar[9]);

    aa.front();
    aa.begin();
    aa.back();
    aa.end();

}


```
>  Drag here

<font color='white'> 

첫번째 원소의 값<br>
첫번째 원소를 가리키는 반복자<br>
마지막 원소의 값<br>
끝을 나타내는 반복자를 반환<br>
</font>

>  Drag here


 ### <strong> Q9 : 일반 반복자와 상수 반복자의 차이점 </strong> 


```
#include <iostream>
#include <vector>

using namespace std;


void main()
{
	int ar[]={9,2,3,4,5,6,7,8,9};
	vector<int> aa(&ar[0],&ar[9]);


	vector<int>::iterator iter  = aa.begin();
	vector<int>::const_iterator citer  = aa.begin();

}


```
>  Drag here

<font color='white'> 

똑같이 동작하지만, 가리키는 원소의 값을 변경할 수 없음.
const * 와 비슷함
읽기전용으로 데이터를 안전하게 사용할 수 있음
</font>

>  Drag here


 ### <strong> Q10 : const 응용 </strong> 


```
#include <iostream>
#include <vector>

using namespace std;


void main()
{
	int ar[]={9,2,3,4,5,6,7,8,9};
	vector<int> aa(&ar[0],&ar[9]);

    // 다음 반복자의 차이점은 ? 
	vector<int>::iterator iter  = aa.begin();
	vector<int>::const_iterator citer  = aa.begin();
    const vector<int>::iterator iter_const  = aa.begin();
	const vector<int>::const_iterator citer_const  = aa.begin();
    
}


```
>  Drag here

<font color='white'> 

1. 원소이동 가능, 원소변경 가능
2. 원소이동 가능, 원소변경 불가
3. 원소이동 불가, 원소변경 가능
4. 원소이동 불가, 원소변경 불가

</font>

>  Drag here

 ### <strong> Q11 : insert함수의 결과는? </strong> 


```
#include <iostream>
#include <vector>

using namespace std;


void main()
{
    int ar[]={1,2,3,4,5,6,7,8,9};
    vector<int> aa(&ar[0],&ar[9]);


    // aa = 1,2,3,4,5,6,7,8,9
    vector<int>::iterator iter  = aa.begin()+2;
    vector<int>::iterator iter2;

    aa.insert(iter,100);
    // aa = ?
    /* mini Q */
    // aa.insert의 return 값은?    
}


```
>  Drag here

<font color='white'> 

aa= 1,2,100,3,4,5,6,7,8,9<br>
mini Q : 삽입한 값의 위치를 가리키는 반복자를 return함

</font>

>  Drag here



 ### <strong> Q12 : assign함수의 결과는? </strong> 


```
#include <iostream>
#include <vector>
using namespace std;


void Print (const vector<int>& v){
	//vector<int> v;
	for (int i=0; i<v.size();i++){
		cout << v[i] <<" ";
	}
	cout << endl;
}

void main()
{
	int ar[]={1,2,3,4,5,6,7,8,9};
	vector<int> aa(&ar[0],&ar[9]);

	Print(aa);
	cout<<"capacity : " << aa.capacity()<< endl;
	cout<<"size : " << aa.size()<< endl;

	aa.assign(1,1);

	Print(aa);
	cout<<"capacity : " << aa.capacity()<< endl;
	cout<<"size : " << aa.size()<< endl;


}

```
>  Drag here

<font color='white'> 

1 2 3 4 5 6 7 8 9 <br>
capacity : 9<br>
size : 9<br>
1<br>
capacity : 9<br>
size : 1<br>


</font>

>  Drag here

## <strong> Deque 컨테이너 </strong>

> 1. Deque는 vector와 기능과 동작이 가장 비슷한 컨테이너다.
> 2. Vector의 단점을 몇가지 보완하는 몇 가지 특징을 갖는다.
> 3. Vector와 마찬가지로 배열 기반 컨테이너다.
> 4. Vector와 마찬가지로 임의 접근 반복자를 사용한다. ( + , -, +=, -= ... 사용가능)
 ### <strong> Q13 : deque와 vector는 뭐가 달라요? </strong> 
>  Drag here

<font color='white'> 

1. 메모리 할당 정책이 다릅니다. <br>
vector는 연속된 메모리 공간을 가져야하지만,<br>
deque의 경우 새로운 메모리 공간이 필요할 경우 새로운 메모리 블럭을 할당하여 원소를 추가합니다.

2. push_front()를 사용할 수 있습니다.


</font>

>  Drag here


## <strong> List 컨테이너 </strong>

> 1. 노드 기반 컨테이너로 원소가 노드 단위로 저장되며, 이중 연결 리스트로 구현됩니다.
> 2. vector나 deque와 유사하지만 list만의 특징적인 함수들이 있다.
> 3. 양방향 반복자를 제공한다 ( ++, --를 활용하여 원소를 탐색할 수 있다. )
> 4. insert, erase를 하여도 상수 시간 복잡도를 갖는다, 또한 insert와 erase 동작시 반복자를 무효화 시키지 않습니다.

 ### <strong> Q14 : insert, erase를 하여도 상수 시간 복잡도를 갖는 이유 </strong> 
>  Drag here
<font color='white'> 

배열 기반 컨테이너처럼 원소를 밀어내지 않고, 
노드만 서로 연결해주면 되기 때문.


</font>

>  Drag here


 ### <strong> Q15 : insert와 erase 동작시 반복자를 무효화 시키지 않는 이유 </strong> 
>  Drag here
<font color='white'> 

반복자가 가리키는 원소 자체가 제거되지 않는 한 반복자가 가리키는 원소는 그대로 존재합니다.

vector, deque에서 무효화되는 이유는 삽입과 제거 동작이 발생하면 메모리가 재할당되거나 원소가 이동할 수 있으므로...


</font>

>  Drag here


 ### <strong> Q16 : list가 가진 remove 함수 </strong> 
 ```c
 #include <iostream>
#include <list>
using namespace std;



void main()
{
    int ar[]={1,2,1,4,1,6,1,8,1};
    list<int> aa(&ar[0],&ar[9]);

    aa.remove(1);

    list<int>::iterator i = aa.begin();

    for(; i != aa.end(); ++i)
    {
     	cout << *i << endl;
    }

}
 ```
>  Drag here
<font color='white'> 

2 4 6 8 

</font>

>  Drag here


 ### <strong> Q17 : list가 가진 remove_if 함수 </strong> 
 ```c
#include <iostream>
#include <list>
using namespace std;

bool for_remove_if(int n) 
{
	return 1<= n && n <=3;
}

void main()
{
	int ar[]={1,2,1,4,1,6,1,8,1};
	list<int> aa(&ar[0],&ar[9]);



	aa.remove_if(for_remove_if);

	list<int>::iterator i = aa.begin();
	for(; i != aa.end(); ++i)
	{
		cout << *i << endl;
	}

}
 ```
>  Drag here
<font color='white'> 

4 6 8 

</font>

>  Drag here


* ###   remove_if 정의
```c
template < class ForwardIterator, class Predicate > 
ForwardIterator remove_if ( ForwardIterator first, ForwardIterator last, Predicate pred ) 
{ 
	ForwardIterator result = first; 
	for ( ; first != last; ++first) 
		if (!pred(*first)) 
			*result++ = *first; 
	return result; 
}

```


 ### <strong> Q18 : list예제 모음 </strong> 
 ```c
#include <iostream>
#include <list>
using namespace std;

bool Predicate(int first, int second){
	return second-first >=0;
}

int main(){

	list<int> lt1;
	list<int> lt2;

	lt1.push_back(10);
	lt1.push_back(20);
	lt1.push_back(30);
	lt1.push_back(40);
	lt1.push_back(50);

	lt2.push_back(100);
	lt2.push_back(200);
	lt2.push_back(300);
	lt2.push_back(400);
	lt2.push_back(500);

	list<int>::iterator iter;
	cout << "lt1 : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	cout << "lt2 : ";
	for (iter = lt2.begin(); iter != lt2.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl << "==================" << endl;


	iter = lt1.begin();
	++iter;
	++iter;        //30 원소의 위치를 가리킴

	lt1.splice(iter, lt2); // iter가 가리키는 위치(30)에lt2의 모든 원소를 잘라서 붙인다.

	cout << "lt1 : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	cout << "lt2 : ";
	for (iter = lt2.begin(); iter != lt2.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl << "==================" << endl;

	lt1.reverse();    // 리스트 역순 뒤집기

	cout << "lt1 reverse : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl << "==================" << endl;

	lt1.push_front(50);
	lt1.push_front(50);
	lt1.push_back(10);
	lt1.push_back(50);

	cout << "lt1 : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	lt1.unique();        // 인접한 중복 원소를 합친다.

	cout << "lt1 unique : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	lt1.unique(Predicate);    // Predicate에 따라 다음 원소가 크거나 같으면 제거한다.

	cout << "lt1 unique Predicate: ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl << "==================" << endl;

	lt1.sort();    // 오름차순으로 정렬한다.

	cout << "lt1 sort : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl << "==================" << endl;

	lt2.push_back(30);
	lt2.push_back(60);
	lt2.push_back(10);
	lt2.push_back(99);
	lt2.sort();

	cout << "lt1 : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	cout << "lt2 : ";
	for (iter = lt2.begin(); iter != lt2.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	lt1.merge(lt2);    // lt1에 lt2를 오름차순 정렬 순서로 합병한다.

	cout << "lt1 merge : ";
	for (iter = lt1.begin(); iter != lt1.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	cout << "lt2  merge : ";
	for (iter = lt2.begin(); iter != lt2.end(); ++iter){
		cout << *iter << ' ';
	}
	cout << endl;

	return 0;
}


 ```
>  Drag here
<font color='white'> 

lt1 : 10 20 30 40 50

lt2 : 100 200 300 400 500

=================

lt1 : 10 20 100 200 300 400 500 30 40 50

lt2 :

=================

lt1 reverse : 50 40 30 500 400 300 200 100 20 10

=================

lt1 : 50 50 50 40 30 500 400 300 200 100 20 10 10 50

lt1 unique : 50 40 30 500 400 300 200 100 20 10 50

lt1 unique Predicate : 50 40 30 20 10

================

lt1 sort : 10 20 30 40 50

================

lt1 : 10 20 30 40 50

lt2 : 10 30 60 99

lt1 merge : 10 10 20 30 30 40 50 60 99

lt2 merge :


</font>

>  Drag here


 ### <strong> Q17 : list의 merge와 splice의 차이점 </strong> 

>  Drag here
<font color='white'> 

splice는 다른 리스트의 원소를 잘라 붙이고<br>
merge는 합병하면서 정렬을 수행한다. <br>
만약 합병할 list가 정렬기준이 틀렸거나,<br>
정렬되어 있지 않으면 오류를 발생한다.<br>



기본은 오름차순 less<type><br>
내림차순을 수행하고 싶으면, list1.merge(list2, greater<type>()); 을 수행<br>
</font>

>  Drag here


<strong> </strong>


