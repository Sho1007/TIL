# explicit

## Header
NONE

## Usage
```c++
```


## Description
> explicit 키워드는 자신이 원하지 않은 형변환이 일어나지 않도록 제한하는 키워드이다.

##  Example
```c++
class A
{
public:
    A(int n) : num(n) {}
    void Print() {cout << num << endl;}
private:
    int num;
};

int main()
{
    A a;
    a = 10;   
    a.Print();
    // 출력 결과 10
}
```
위의 경우 a = 10; 에서 컴파일러가 할당연산자 우측의 RValue 10을 좌측의 LValue a에 넣어주기 위해 10을 A로 형변환을 하려한다.   
이 때 마땅한 형 변환 함수가 없기 때문에 A의 생성자 중 int를 매개변수로 갖는 생성자 A(int n)을 사용하게 된다.   
그러면 num을 10으로 갖는 또다른 A 인스턴스가 생성되어 a로 얕은복사가 이루어진다.
```c++
class A
{
public:
    A(int n) : num(n) {}
    void Print() {cout << num << endl;}
private:
    int num;
};

int main()
{
    A a;
    // a = 10; 에러 발생
    a = A(10);
    a.Print();
    // 출력 결과 10
}
```
하지만 위와 같이 생성자에 explicit을 붙이면 컴파일러단에서 자동 형변환이 일어나지 않는다.

(따라서 사용자가 직접 형 변환을 넣어줘야함 -> 이 경우는 RValue로 num을 10으로 갖는 객체를 생성한 후 기본 대입연산자를 이용하여 얕은복사를 하였다)


## Caution
> explicit 키워드를 사용하면 자동 형변환이 일어나지 않으므로 따로 형변환을 시켜줘야 한다는 점을 명시하자

## Reference
https://dydtjr1128.github.io/cpp/2019/07/13/Cpp-explicit-keyowrd.html