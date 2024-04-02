# IDLE Task

## What is IDLE Task

*IDLE task*는 **`RTOS Scheduler`시작될 때 자동으로 생성**되어, 항상 실행 가능한 task가 하나 이상 있는지 확인한다.

`ready`상태에서 우선 순위가 높은 task들이 있는 경우 CPU time을 사용하지 않도록 가능한 제일 낮은 우선 순위로 생성된다.

IDLE task는 **Kernel에 의해 생성**된다. 그래서 *System task*라고도 부른다.

### What IDLE Task do?

RTOS에 의해 삭제된 작업에 할당된 메모리를 반환(Free)하는 역할을 가지고 있다.
그러므로 idle task에 처리 시간이 부족하지 않도록 하는 것이 `vTaskDelete()`을 사용하는 application에서 중요합니다.

## IDLE Task Hook

`idle task hook`은 idle task의 각 싸이클마다 호출되는 함수이다. 여기서 기능을 정의하여 사용할 수 있고, 방식에는 두가지가 있다.
- `idle task hook`에 기능을 구현하는 방법
- `idle priority task`를 기능 구현을 위해 생성하는 방법 (유연한 기능 구현이 가능하지만 그만큼 RAM 사용량이 늘어난다.)

`idle hook`을 생성하기 위해서는

- `FreeRTOSConfig.h`에서 `configUSE_IDLE_HOOK`을 1로 설정해준다.
- `void vApplicationIdleHook(void);` 함수를 구현한다.

보통 idle hook function은 CPU를 *power saving mode*로 만들기 위해 주로 사용한다.
