## Power management

### Sleep

There is only one sleep function in Mbed OS:

```c++
void sleep();
```

This function invokes sleep manager, which selects the most appropriate sleep mode.

<span class="notes">**Note:** In most cases, you don't need to call `sleep()` directly. Mbed OS enters sleep mode automatically any time the system is idle. That is when all your threads are in a waiting state, for example waiting for an event or a timeout.</span>

#### Sleep modes

There are two available sleep modes:

1. Sleep mode

The system clock to the core stops until a reset or an interrupt occurs. This eliminates dynamic power that the processor, memory systems and buses use. This mode maintains the processor, peripheral and memory state, and the peripherals continue to work and can generate interrupts.

You can wake up the processor by any internal peripheral interrupt or external pin interrupt.

2. Deep sleep mode

This mode is similar to sleep but saves more power and has a longer wakeup time. It saves additional power by turning off the high-speed clocks. Because of this, you can only enter this mode when peripherals relying on high-speed clocks are not in use. Peripherals that do not rely on high-speed clocks include the LowPowerTicker, RTC and InterruptIn APIs. This mode maintains all state.

#### Sleep manager

The sleep manager provides an API and logic to control device sleep mode selection. Although standard sleep doesn't affect application execution, deep sleep might introduce some additional power savings that can affect the application, for instance high-speed clock-dependent drivers. To ensure correct operation of your application, sleep manager may disable deep sleep, in which case your board enters normal sleep, instead. This mechanism is mostly invisible to the user, but you should be aware that it may affect the power consumption of your hardware.

These Mbed OS drivers can lock the deep sleep:

- `Ticker`.
- `Timeout`.
- `Timer`.
- `SPI`.
- `I2C`.
- `CAN`.
- `SerialBase`.

#### Sleep tracing

Mbed OS can help you to understand the sleep patterns of your device, specifically who is holding a sleep locks preventing your board to enter the deep sleep. To enable the tracing, all you need to do is to define `MBED_SLEEP_TRACING_ENABLED` macro. You can do it by modifying your `mbed_app.json` config file or appending `-DMBED_SLEEP_TRACING_ENABLED` to `mbed compile` command.

Mbed OS will print sleep traces on the standard output, which by default is UART. Some of the events that we track:

* Locking deep sleep: `LOCK: <file name>, ln: <line in file>, lock count: <number of locks held>`
* Unlocking deep sleep: `UNLOCK: <file name>, ln: <line in file>, lock count: <number of locks held>`
* Entering sleep: Mbed OS will print a list of locks preventing the board from entering a deep sleep:

```
Sleep locks held:
[id: <file name 1>, count: <number of locks>]
[id: <file name 2>, count: <number of locks>]
```

Example trace can look like:

```
LOCK: mbed_rtx_idle.cpp, ln: 129, lock count: 2
Sleep locks held:
[id: mbed_wait_api_, count: 1]
[id: mbed_rtx_idle., count: 1]
UNLOCK: mbed_rtx_idle.cpp, ln: 131, lock count: 1
LOCK: mbed_rtx_idle.cpp, ln: 129, lock count: 2
Sleep locks held:
[id: mbed_wait_api_, count: 1]
[id: mbed_rtx_idle., count: 1]
UNLOCK: mbed_rtx_idle.cpp, ln: 131, lock count: 1
```

<span class="notes">**Note:** Sleep tracing is a debug feature and should only be enabled during development cycle. Its heavy use of UART can affect the device performance.</span>

#### Example

[![View code](https://www.mbed.com/embed/?url=https://os.mbed.com/teams/mbed_example/code/SleepManager_Example_1/)](https://os.mbed.com/teams/mbed_example/code/SleepManager_Example_1/file/e85412b4147e/main.cpp)

### System reset

Mbed OS provides a standardized call to power cycle the system:

```c++
void system_reset();
```

After the call the processor and most components will reset, but it will not affect the debug subsystem.

### Function reference

[![View code](https://www.mbed.com/embed/?type=library)](http://os-doc-builder.test.mbed.com/docs/development/mbed-os-api-doxy/group__platform__power__mgmt.html)

### Idle loop

Idle loop is a background system thread, which scheduler executes when no other threads are ready to run. That may happen when your application is waiting for an event to happen. By default, the idle loop invokes sleep manager to enter a sleep mode. You can overwrite this behavior by providing a different handler:

```c++
void new_idle_loop()
{
    // do nothing
}

void main()
{
    rtos_attach_idle_hook(&new_idle_loop);
}
```

#### Function reference

[![View code](https://www.mbed.com/embed/?type=library)](http://os.mbed.com/docs/v5.7/mbed-os-api-doxy/group__rtos___idle.html)

### Example

[![View code](https://www.mbed.com/embed/?url=https://os.mbed.com/teams/mbed_example/code/SleepManager_Example_1/)](https://os.mbed.com/teams/mbed_example/code/SleepManager_Example_1/file/e85412b4147e/main.cpp)