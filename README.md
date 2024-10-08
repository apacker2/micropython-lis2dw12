# MicroPython LIS2DW12 I2C driver

MicroPython library for accessing the [STMicroelectronics LIS2DW12](https://www.st.com/en/mems-and-sensors/lis2dw12.html) 3-axis accelerometer over
I2C. The LIS2DW12 is an ultra-low-power high-performance three-axis linear accelerometer belonging to the “pico” family. It has full scales of ±2g/±4g/±8g and is capable of measuring accelerations with output data rates from 10 Hz to 800 Hz.

This is a fork of [This repo](https://github.com/tuupola/micropython-lis2hh12) modified to hopefully work with the LIS2DW12 instead

## Usage

Simple test with never ending loop.

```python
import utime
from machine import I2C, Pin
from lis2dw12 import LIS2DW12

i2c = I2C(id=1, scl=Pin(26), sda=Pin(25))
sensor = LIS2DW12(i2c)

print("LIS2DW12 id: " + hex(sensor.whoami))

while True:
    print(sensor.acceleration)
    utime.sleep_ms(1000)
```

By default the library returns 3-tuple of X, Y, Z axis acceleration values in m/s^2 which is the SI standard. To get the acceleration values in g instead set the scale factor to `SF_G` in the constructor.

```python
from machine import I2C, Pin
from lis2dw12 import LIS2DW12, SF_G

i2c = I2C(id=1, scl=Pin(26), sda=Pin(25))
sensor = LIS2DW12(i2c, sf=SF_G)
```

More realistic example usage with timer. If you get `OSError: 26` or `i2c driver install error` after soft reboot do a hard reboot.

```python
import micropython
from machine import I2C, Pin, Timer
from lis2dw12 import LIS2DW12

micropython.alloc_emergency_exception_buf(100)

i2c = I2C(id=1, scl=Pin(26), sda=Pin(25))
sensor = LIS2DW12(i2c)

def read_sensor(timer):
    print(sensor.acceleration)

print("LIS2DW12 id: " + hex(sensor.whoami))

timer_0 = Timer(0)
timer_0.init(period=1000, mode=Timer.PERIODIC, callback=read_sensor)
```

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
