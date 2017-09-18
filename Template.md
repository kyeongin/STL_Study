# Template
##  종류 
 > 1. function Template
    > > 함수를 만들어내는 함수 틀

 > 2. class Template
    > > 클래스를 만들어내는 틀

 * 템플릿 함수를 사용하면 컴파일러는 클라이언트(호출 측)의 함수 호출 인자 타입을 보고 템플릿 함수의 매개변수 타입을 결정하여 실제 함수인 템플릿 인스턴스 함수를 만들어 냅니다.

 > 함수 템플릿은 함수 앞에 ``template<T>`` 키워드를 사용, 타입 일반화(T type)

 > 함수 형식은 ``template<typename T> void Print(T a, T b);``

 > T는 템플릿 매개변수, 클라이언트에서 결정

 > 컴파일이 완료되면 함수 템플릿은 존재하지 않으며, 인스턴스화된 함수들이 있을 뿐입니다. 
 > > [예1]의 경우 template<int>Print<int>, ...
```cpp
/* 매개변수 type 1개 [예1]*/
    template<typename T>
    void Print(T a, T b)
    {
        cout << a << ", " << b < < endl;
    }

    int main(){
        Print(10,20);
        Print(0.123, 1.123);
        Print("ABC","abcde");

        return 0;
    }
/* 매개변수 type 2개 */
    template<typename T1, typename T2>
    void Print(T1 a, T2 b)
    {
        cout << a << ", " << b < < endl;
        
    }

    int main(){
        Print(10,20.44);
        Print(0.123, "Hello");
        Print(12.34,"abcde");

        return 0;
    }
```

## 배열 출력 함수 탬플릿
 > 배열을 출력하기 위해서는 매개변수를 우선 포인터 형태로 받아야 한다.

```cpp
/* 매개변수 type 2개 */
    template<typename T1,int size >
    void Print(T1* a)
    {
        cout << a << ", " << b < < endl;        
    }

    int main(){

        int arr = {1,2,3,4};

        Print<int ,sizeof(arr)>(arr); /*명시적으로 선언해줘야 한다.*/ 
        return 0;
    }
```
    > 명시적 선언은 배열때문이 아닌 정수때문이다. 컴파일러가 추론할 수 없으므로 명식적으로 호출해야 한다.


## 클래스 템플릿
 > 클래스 템플릿은 클래스를 만들어 내는 틀(메타 코드)

 * Array(int cap) : 생성자는 저장 가능한 최대 원소의 개수(cap)를 인자로 받아 메모리 생성
 * ~Array() : 소멸자는 생성한 메모리 제거
 * Add(int data) : 객체에 정수(data)를 추가(저장)
 * operator[](int idx): Array객체의 idx 인덱스 원소의 값을 반환
 * GetSize() : Array 객체의 원소 개수를 반환

```cpp
    class Array
    {
        int *buf;
        int size //원소 개수
        int capacity;
    public:
        Array(int cap = 100):buf(0),size(0),capacity(cap) {
            buf = new int[capacity];
        }
        ~Array(){ delete [] buf;}

        void Add(int data){
            buf[size] = data;
            size++;
        }
        int operatorp[] (int idx){
            return buf[idx];
        }
        int GetSize(){
            return size;
        }
    }

    int main()
    {
        Array array_;

        array_.Add(10);
        array_.Add(20);
        array_.Add(30);

        for(int i=0;i<array_.GetSize();i++){
            cout<<array_[i]<<endl;
        }
        return 0;
    }
```
- - - - - -
```cpp
/*class Template*/
    //  main.cpp
    //  helloworld_cpp
    //
    //  Created by 김경인 on 2017. 1. 8..
    //  Copyright © 2017년 김경인. All rights reserved.
    //

    #include <iostream>

    using namespace std;

    template <typename T = int>
    class Array
    {
        T *buf;
        int size; //원소 개수
        int capacity;
    public:
        Array(int cap = 100):buf(0),size(0),capacity(cap) {
            buf = new T[capacity];
        }
        ~Array(){ delete [] buf;}
        
        void Add(T data){
            buf[size] = data;
            size++;
        }
        T operator[] (int idx) const{
            return buf[idx];
        }
        int GetSize(){
            return size;
        }
    };

    int main()
    {
        Array<> array_;  //default int
        Array<int> i_array_;
        
        Array<float> f_array_;
        
        Array<string> s_array_;
        
        array_.Add(10.5);
        array_.Add('n');
        i_array_.Add(20);
        f_array_.Add(30.4);
        s_array_.Add("Naver");
        
        for(int i=0;i<array_.GetSize();i++){
            
            cout<<array_[i]<<endl; 
        }
        return 0;
}
```

# 템플릿을 이용한 제네릭 코드
 > 제네릭스란 쉽게말해서 ArrayList가 다룰 객체를 미리 명시해줌으로써 형변환을 하지 않고 사용하는 것입니다. 즉 ArrlayList가 사용할 객체의 타입이란 이야기 입니다. ---> ArrayList< T >


> 아래 코드는 해당 템플릿 클래스가 한개의 타입에 대해 템플릿화 되어 있다는 것을 의미 

* 템플릿은 타입을 <b><l>파라미터화</b></l> 한다.
    > 함수가 인자값을 '파라미터화'하는 것과 유사하다.
    > 함수에서 파라미터가 이름을 가지듯이, 템플릿 클래스에서도 파라미터화된 타입이 이름(T)을 가짐 

    * 템플릿 파라미터
        1. 데이터형 파라미터
        2. non type 파라미터
        3. 템플릿 템플릿 파라미터

```cpp
template <typename T>
class Grid {
public:
    explicit Grid(size_t inWidth = kDefaultWidth,
                  size_t inHeight = kDefaultHeight);
    virtual ~Grid();
    
    void setElementAt(size_t x, size_t y, const T& inElem);
    T& getElementAt(size_t x, size_t y);
    const T& getElementAt(size_t x, size_t y) const;
    
    size_t getHeight() const{return mHeight;}
    size_t getWidth() const{return mWidth;}
    static const size_t kDefaultWidth = 10;
    static const size_t kDefaultHeight = 10;

    
private:
    void inital();
    std::vector<std::vector<T>> mCells;
    size_t mWidth,mHeight;
};


```
## Grid template class method define

> methodm static data member를 정의시 클래스 이름은 'Grid< T>'

```cpp
template <typename T>
Grid<T >::Grid(size_t inWidth, size_t inHeight):mWidth(inWidth), mHeight(inHeight)
{
    initial();
}
```

* Type Aliases == Template Aliases [c++11]
  > Grid< int >와 같이 Grid의 템플릿 인자를 매번 나열하는 것이 불편하다면 Type Aliases를 이요해서 타입이름을 줄여쓸수 있다.  

  > typedef같은 역할

  - using IntGrid Grind< int >
  - void ProcessIntGrid(intGrid& inGrid){ }

## Type aliases
 * [참조 : cppregerence.com][id]

[id]: http://en.cppreference.com/w/cpp/language/type_alias 




## non type 템플릿 파라미터
 > non type 템플릿 파라미터는 int나 포인터 값 같이 일반적으로 함수나 메서드에서 사용할 수 있는 것들을 의미한다.

 > 생성자 인자 대신 템플릿 인자를 사용하면 컵ㅁ파일 타임에 크기 값이 알려지기 때문에 최적화에 좀더 유리한 장점이 있다. 

 ```cpp
template <typename T, size_t WIDTH, size_t HEIGHT>
class Grid {
public:
    Grid();  //바뀜
    virtual ~Grid();
    
    void setElementAt(size_t x, size_t y, const T& inElem);
    T& getElementAt(size_t x, size_t y);
    const T& getElementAt(size_t x, size_t y) const;
    
    size_t getHeight() const{return HEIGHT;} //바뀜
    size_t getWidth() const{return WIDTH;} //바뀜
    static const size_t kDefaultWidth = 10;
    static const size_t kDefaultHeight = 10;

    
private:
    T mCells[WIDTH][HEIGHT];
    
};
int main(){
    Grid<int,10,10> mGrid;
    myGrid.setElemnetAt(2,3,45);
}
```
* 주의 할점
```cpp
    size_t height=10;
    Grid<int,10,height> testGrid; // complie error

```

 * 파라미터 이기 때문에 Grid<int,10,10> 과 Grid<int,10,11>은 다른 타입이 된다.

## template class의 특수화

```cpp

// 템플릿 트구화를 할 때는 원본 템플릿이 스코프에 들어 있어야 한다.
// 여기서 사용된 #include는 원본 템플릿으로 접근을 보증하기 윈한 것이다.
template <>
class Grid<const char*> {
public:
    explicit Grid(size_t inWidth = kDefaultWidth,
                  size_t inHeight = kDefaultHeight);
    virtual ~Grid();
    
    void setElementAt(size_t x, size_t y, const char*& inElem); //바뀜
    T& getElementAt(size_t x, size_t y);
    const char* getElementAt(size_t x, size_t y) const; //바뀜
    
    size_t getHeight() const{return mHeight;}
    size_t getWidth() const{return mWidth;}
    static const size_t kDefaultWidth = 10;
    static const size_t kDefaultHeight = 10;

    
private:
    void inital();
    std::vector<std::vector<std::string>> mCells; //바뀜
    size_t mWidth,mHeight;
};

```

```cpp
template<>
class Grid<const char*>

```
 > 위 문법은 컴파일러로 하여금 해당 클래스가 Grid템플릿 클래스의 char* type특수화 라는 점을 알려준다.

 > 만약 class Grid가 정의되어 있다만 에러를 발생시킨다. 특수화를 통해서만 같은 이름 클래스를 만들수 있다.]

### 부분 특수화
```cpp
// 동일한 타입의 인자를 2개 받는 부분 특수화
    template <typename T> //1
    class MyClass<T, T>
    {
            ...
    };

// 두 번째 인자를 정수형으로 받는 부분 특수화
    template <typename T> //2
    class MyClass<T, int>
    {
            ...
    };

// 두 인자를 모두 타입의 포인터로 받는 부분 특수화
    template <typename T1, typename T2> //3
    class MyClass<T1*, T2*>
    {
            ...
    };

// 동일한 타입의 인자를 2개 받는 부분 특수화
    template <typename T>   //4
    class MyClass<T*, T*>
    {
            ...
    };

int main()
{
    MyClass<int, int> m;  //error가 안나는 템플릿 메소드는?
    MyClass<int*, int*> m; //error가 안나는 템플릿 메소드는?
}

```

> MyClass<int, int> 경우에는 MyClass<T, T> 와 MyClass<T, int> 로 모두   해석이 가능하기 때문에 모호성을 띄게 되고, MyClass<int*, int*> 역시     MyClass<T, T> , MyClass<T1*, T2*> 모두 가능하기 때문에 모호해집니다.

 * 클래스 템플릿의 멤버함수를 전체적으로 특화시킬 수 있지만, 부분적으로    특화시킬수 없습니다

 * 함수 템플릿를 부분적으로 특화시킬 수 없습니다 하지만, 함수의 오버로딩을 통해 부분적으로 특화시키는 것과 비슷한 효과를 낼수 있습니다


## 함수 템플릿은 특화 해서는 안된다.
 > specialization는 올바르게 이루어졌을 때만 좋은 것이다.
 
 > 함수 템플릿을 확장하고 싶은 경우일때는, 오버로드를 만들고 그 오버로드가 사용될 타입의 네임스페이스에 넣는것이 보다 좋다.






  


