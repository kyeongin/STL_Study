# 함수 포인터 
 > 함수 포인터는 함수의 시작 주소를 저장하는 포인터 입니다.

 ```c
    void Print(int n){

    }
    int main(){
        void (*pf)(int); // 함수 포인터 선언
        pf = Print;

        Print(10);
        pf(10);  /*포인터를 이용한 함수 호출 1*/
        (*pf)(10); /*포인터를 이용한 함수 호출 2*/

    }
 ```
## 함수 포인터의 종류
 > 1. 정적 함수 호출(정적 함수) --> 그냥 일반 함수 호출
 > 2. 객체로 멤버 함수 호출(멤버 함수)
 > 3. 객체의 주소로 멤버 함수 호출(멤버 함수)


```c
    class Point
    {
        public:
        void Print(){

        }
    };

    int main(){
        Point pt;
        Point *p =&pt;

        pt.Print() /*2. 객체로 멤버함수 호출*/
        p->Print() /*3. 객체의 주소로 멤버함수 호출*/
    }
```

### 정적 함수 호출(namespace 내 전역 함수, static 멤버 함수)

```c
    void (*pf)(int); //static function pointer declaration

    Print(10); // 전역함수 호출
    A::Print(10); // namespace A의 전역함수 호출
    Point::Print(10); // Point class 전역함수 호출

    pf = Print;
    pf(10);
    pf = A::Print;
    pf(10);
    pf = Point::Print;

```
### 객체와 주소로 멤버 함수 호출
 > void Point::Point(int n)

 * void (Point::*pf)(int)로 선언
  
```c
    Point pt;
    Point *p = &pt;

    void (Point::*pf)() const // 멤버 함수 포인터 선언
    pf = &Point::Print();

    (pt.*pf)(); //객체의 함수 포인터 호출
    (p->*pf)(); 

```