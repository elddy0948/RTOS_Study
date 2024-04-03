# Hooks(Callbacks)

## IDLE Hook Function

idle task는 선택적으로 `idle hook`이라고 불리는 정의된 hook(or callback)함수를 호출할 수 있다.
idle task자체의 우선순위가 매우 낮으므로 실행할 수 있는 높은 우선순위의 task가 없을 때만 실행된다.

`configUSE_IDLE_HOOK`을 1로 설정해야하고, 설정하면 반드시 `void vApplicationIdleHook(void)`를 구현하여 hook function을 제공해야한다.
여기서 idle task가 실행중에 반복하여 호출되고, **block을 야기할 수 있는 어떠한 API 함수 호출도 하지 않는 것이 중요하다.**

또한 `vTaskDelete()`함수를 사용하는 앱이라면, idle task hook을 주기적으로 반환할 수 있도록 해줘야 한다. (Kernel에 의해 삭제된 task에 대한 자원 반환을 위해서.)

## Tick Hook Function

Tick interrupt는 `tick hook`이라고 불리는 정의된 hook(or callback)함수를 호출할 수 있다. 이는 timer에 어떤 기능을 제공하기에 아주 편리한 위치이다.

`configUSE_TICK_HOOK`을 1로 설정해야하고, 설정하면 반드시 `void vApplicationTickHook(void)`를 구현하여 hook function을 제공해야한다.
이때 이 함수는 ISR 내에서 실행되므로, **짧아야하고, 많은 Stack을 잡아먹으면 안된다.** 또한 `FromISR` 혹은 `FROM_ISR`로 끝나지 않는 API함수를 호출하면 안된다.
