# Memroy Model
    프로그램 작동 시 쓰레드에서 메모리를 어떻게 관리할 것인지에 대한 정책
인텔과 AMD의 경우 CPU 차원에서 순차적 일관성을 보장
* seq_cst를 써도 별다른 부하가 없음
* 하지만 ARM의 경우는 꽤 유의미한 차이가 있다고 함

#

## 1. Sequentially Consistent
    var.store(value, memory_order::memory_order_seq_cst);
* 가장 엄격한 정책
    *  컴파일러 최적화의 여지가 적음
    * 직관적
* 가시성, 코드 재배치 문제 바로 해결

#

## 2. Acquire-Release
    var.store(value, memory_order::memory_order_release);
    var.load(memory_order::memory_order_acquire);
* 중간 정도의 염격함
* release 명령 이전의 메모리 명령들이, 해당 명령 이후로 재배치 되는 것을 방지 (절취선)
* 그리고 acquire 로 같은 변수를 읽는 쓰레드가 있다면 release 이전의 명령들이 acquire 하는 순간 관찰 가능 (가시성)

#

## 3. Relaxed
    var.store(value, memory_order::memory_order_relaxed);
* 가장 자유로운 정책
    * 컴파일러 최적화의 여지가 많음
    * 직관적이지 않음
* 코드 재배치도 멋대로 가능
* 가시성 해결 NO
* 가장 기본 조건 (동일 객체에 대한 동일 수정 순서)만 보장

