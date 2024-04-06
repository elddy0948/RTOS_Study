# Delay APIs

## FreeRTOS task delay APIs

FreeRTOS의 task delay API들을 사용하면서 얻을 수 있는 이점은
- Processor를 방해하지 않고 Task를 Delay할 수 있다.
- Periodic task들을 구현할 수 있다.

Delay API를 사용하면, Task들은 `Blocked`상태에 들어가게 된다.

## vTaskDelay()

```C
void vTaskDelay( const TickType_t xTicksToDelay );
```

`INCLUDE_vTaskDelay`를 1로 설정하면서 사용할 수 있다. `xTicksToDelay`로 받은 값 만큼 Delay할 수 있다. vTaskDelay()는 **periodic task의 주기를 설정하기에는 좋은 메서드가 아니다. 이는 `vTaskDelay()`는 code의 경로, 다른 Task, interrupt등에 영향을 받기 때문이다.**

### Example

```C
/* Block for 500ms. */
const TickType_t xDelay = 500 / portTICK_PERIOD_MS;

for( ;; )
{
  vToggleLED();
  vTaskDelay( xDelay );
}
```

## vTaskDelayUntil

```C
void vTaskDelayUntil( TickType_t *pxPreviousWakeTime,
                      const TickType_t xTimeIncrement );
```

`INCLUDE_vTaskDelayUntil` 값을 1로 설정해야 한다.

`vTaskDelay()`와의 주요한 차이점 하나는, `vTaskDelay()`는 호출된 시점으로부터 Task가 unblock돼야 하는 **상대적인 시간(relative time)을 나타낸다.**
반면에, `vTaskDelayUntil()`은 Task가 unblock돼야 하는 **절대적인 시간(absolute time)을 나타낸다.**

즉 `vTaskDelay()`는 **고정된 실행 시간을 보장하기 힘들다.**

그리고 `vTaskDelayUntil()`은 이미 wake time이 지나가버린 경우, **Blocking 없이 즉시 return 한다.** 그래서 `vTaskDelayUntil()`을 사용하여 periodically execute 중이던 작업이 어떠한 이유에서 중단이 된다면, 이를 다시 실행하기 위해서는 **wake time을 다시 계산해야 한다.**

`portTICK_PERIOD_MS`가 tick rate를 구하는데에 사용되며, 이 함수는 `vTaskSuspendAll()`에 의해 RTOS scheduler가 `suspended`된 상태에 호출되면 안된다.

### Example

```C
 // Perform an action every 10 ticks.
TickType_t xLastWakeTime;
const TickType_t xFrequency = 10;

xLastWakeTime = xTaskGetTickCount();

for(;;)
{
  vTaskDelayUntil(&xLastWakeTime, xFrequency);
}
```
