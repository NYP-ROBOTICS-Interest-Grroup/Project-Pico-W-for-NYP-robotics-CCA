# Pico W Lesson Plan for NYP robotics CCA
<br>

# Chapter 3: Your First Program
<br>

```
import machine
led = machine.Pin("LED", machine.Pin.OUT)
led.toggle()
```

## Upgraded code<br>

```
import machine
from machine import Timer

led = machine.Pin("LED", machine.Pin.OUT)
timer = Timer()

def blink(timer):
    led.toggle()

timer.init(freq=2.5, mode=Timer.PERIODIC, callback=blink)
```