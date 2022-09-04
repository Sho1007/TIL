# UPROPERTY

## Constructor 와 BeginPlay 에서의 차이점
> Constructor()에서는 해당 코드 위까지 초기화된 값으로 사용     
> BeginPlay()에서는 Unreal Editor 에서 수정된 값으로 사용   
> (생성자에서 사용해놓고 왜 Unreal Editor 수정값이 적용이 안되는지 헷갈리지 말자)
### Example
```c++
>> 언리얼 에디터에서 Rows 값을 6으로 수정
// In .h
class MyObject()
{
public:
    UPROPERTY(EditDefaultsOnly)
    int Rows = 0;
}
// In .c++
MyObject::MyObject()
{
    Rows;   // 0 
    Rows = 5;
    Rows;   // 5
    // 여기까지는 초기화한 값이 적용된다.
}

MyObject::BeginPlay()
{
    // 여기서부터는 Editor에서 수정된 값이 적용된다. 
    Rows;   // 6
}
```