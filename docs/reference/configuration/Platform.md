<h2 id="configuration-platform">Platform</h2>

The following is the complete list of platform configuration parameters, as generated by `mbed compile --config -v`. Please see [the configuration system documentation](/docs/development/reference/configuration.html) for details on how you may use or override these settings.

```
Configuration parameters
------------------------

Name: platform.default-serial-baud-rate
    Description: Default baud rate for a Serial or RawSerial instance (if not specified in the constructor)
    Defined by: library:platform
    Macro name: MBED_CONF_PLATFORM_DEFAULT_SERIAL_BAUD_RATE
    Value: 9600 (set by library:platform)
Name: platform.force-non-copyable-error
    Description: Force compile time error when a NonCopyable object is copied
    Defined by: library:platform
    No value set
Name: platform.poll-use-lowpower-timer
    Description: Enable use of low power timer class for poll(). May cause missing events.
    Defined by: library:platform
    No value set
Name: platform.stdio-baud-rate
    Description: Baud rate for stdio
    Defined by: library:platform
    Macro name: MBED_CONF_PLATFORM_STDIO_BAUD_RATE
    Value: 9600 (set by library:platform)
Name: platform.stdio-buffered-serial
    Description: Use UARTSerial driver to obtain buffered serial I/O on stdin/stdout/stderr. If false, unbuffered serial_getc and serial_putc are used directly.
    Defined by: library:platform
    No value set
Name: platform.stdio-convert-newlines
    Description: Enable conversion to standard newlines on stdin/stdout/stderr
    Defined by: library:platform
    No value set
Name: platform.stdio-convert-tty-newlines
    Description: Enable conversion to standard newlines on any tty FILE stream
    Defined by: library:platform
    No value set
Name: platform.stdio-flush-at-exit
    Description: Enable or disable the flush of standard I/O's at exit.
    Defined by: library:platform
    Macro name: MBED_CONF_PLATFORM_STDIO_FLUSH_AT_EXIT
    Value: 1 (set by library:platform)
    ```
