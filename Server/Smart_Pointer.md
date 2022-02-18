# __Smart Pointer__
> 객체의 참조 횟수를 저장하여 언제 객체가 소멸해도 되는지 판단하는 포인터      


## __배경__
#
    서로 다른 두 쓰레드가 하나의 객체를 참조하고 있을 때,
    한 쓰레드에서 객체를 지우게 되면, 다른 쓰레드에서는 작업 도중 객체가 사라져서 CRASH 발생
    Q ) 언제 객체를 지워야 안전하게 지울 수 있을까?
    A ) 아무도 이 객체를 참조하지 않을 때 지우면 된다.
        - 참조 횟수를 저장하여 객체 소멸 가능 여부 판단


## __종류__
#
* unique_ptr
* shared_ptr
* weak_ptr


## __unique_ptr__
#
> 일반 포인터와 크게 다르지 않음
### __생성__
```c++
    unique_ptr<Class> upr = make_unique<Class>();
```
```c++
    // 불가능
    unique_ptr<Class> upr2 = upr;
```

* 복사 생성자를 막아서 여러번 참조되는 경우를 방지
```c++
    unique_ptr<Class> upr2 = std::move(upr); // upr은 소멸
```
* 소유권을 포기하고 이동은 가능 => 이 경우 이전에 가리키던 포인터는 소멸


## __shared_ptr__
#
> 일반적인 스마트 포인터

### __생성__
```c++
    shared_ptr<Class> var(new Class());
```
* [객체][RefCountingBlock]로 이루어짐   
(객체와 RefCountingBlock 각각 한번씩 총 2번 할당)

```c++
    shared_ptr<Class> var = make_shared<Class>();
```
* [객체 | RefCountingBlock] 으로 이루어짐   
객체를 _Ref_count_base 를 상속받아 생성   
객체와 RecCountingBlock 을 묶어서 한번에 할당

```c++
    shared_ptr<Class> spr = make_shared<Class>();
    shared_ptr<Class> spr2 = spr
```
* 위와 같이 복사 생성자로 생성하는 경우 두 공유 포인터가 동일한 객체와 동일한 RefCountingBlock을 가리키게 된다.

     

### __RefCountingBlock__
> _useCount_ 와 _weakCount_ 로 이루어짐   
* useCount : shared_ptr 가 참조하고 있는 횟수 (0이 되면 객체부분은 소멸) -> **강한 참조**
* weakCount : weak_ptr 가 참조하고 있는 횟수 (이 부분이 0이 되어야지만 refCountingBlock 부분까지 소멸) -> **약한 참조**


## __weak_ptr__
#
> 객체의 생명주기엔 관여하지 않고 단순 참조만 하고 싶을 때 (단독 사용 불가)
### __생성__
```c++
    weak_ptr<Class> wpr = spr;
```
* shared_ptr을 복사하여 생성 (이 때 _useCount_ 는 늘어나지 않고, _weakCount_ 만 증가)

### __사용__
```c++
    if (wpr.expired())
        // 객체 존재
    else
        // 객체 소멸
```
* 존재하는지 확인하고 사용 (그냥 사용은 불가) => 객체가 소멸되었을 수도 있기 때문에
```c++
    shared_ptr<Class> spr2 = wpr.lock();
```
* lock을 사용하여 shared_ptr로 casting해서 사용 가능
* useCount가 0인 객체 (소멸된 객체) 로 casting 할 경우 nullptr 을 반환