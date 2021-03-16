---
author: elfnor
date: '2015-07-03 22:00'
image: 'A4988_circuit.jpg'
layout: post
tags:  python micropython
title: 'Micropython - Stepper motor control with a A4988 carrier board'
permalink: micropython-stepper-motor-control-with-a-a4988-carrier-board.html
---

I have a [pyboard](https://micropython.org/) (I get cool birthday presents) and like the look of the micropython language. Python is my first language choice for data analysis at work and I\'ve used it writing add ons for [GIMP]({{ site.baseurl }}{% link _posts/2014-07-05-symmetry_tile_docs.md %}) and [Blender]({{ site.baseurl }}{% link _posts/2015-05-24-blender_pipe_generator.md %}). I can program the [Arduino]({{ site.baseurl }}{% link _posts/2015-01-19-arduino_breadmaker_details.md %}) in Wiring/Processing but being able to program a micro-controller in python is a big plus. The way the REPL prompt/terminal allows interactive scripting, putting a program on the board is just copying, and the ease of printing statements back to the terminal are all easier to use than the equivalent for the Arduino.

Arduino of course has a much bigger user base and a huge resource of code libraries and tutorials. Pick up a sensor or motor control board and you can easily find the code to copy and paste and get it going. The pyboard has some great examples on their [website](docs.micropython.org/en/latest/tutorial/index.html) and I\'ve found some good github projects, [dhylands examples](https://github.com/dhylands/upy-examples) and [mithru examples](https://github.com/mithru/MicroPython-Examples) but more beginner friendly stuff is needed.

I\'m working towards making a ball balancing robot potentially mashing this [electronics and code](https://github.com/jeffmer/micropython-upybbot) with this [hardware](http://xrobots.co.uk/BB8/). I\'m tackling this in small steps starting with the motors and will blog progress as I go.

This post covers three ways to drive bipolar stepper motors through the A4988 chip. This chip is available on carrier boards such as the stepstick and polulu. These are used a lot in home built 3D printers and a set of four can be bought for under \$12 on <http://www.dx.com/>.

## Connections

Connect the board to the stepper motor and pyboard as shown. The jumper between the `RESET` and `SLEEP` pins is important. So is the capacitor on the motor power supply. Put this close to the carrier board.

The `ENABLE` pin can be left unconnected. If the pin is unconnected or held low the motor will be braked when power is applied to the motor. If the enable pin is held high the motor is disabled or not braked. Keeping the motor braked draws power so setting the enable pin can be used to save power and reduce the heat of the board and motor.

The carrier board takes one step for each pulse sent on the `STEP` pin. The pin needs to be set high for at least 2 µs. The `DIR` pin determines motor direction. `HIGH` goes one way, `LOW` the other. The board can also be used to do microstepping but I won\'t cover that here.

The potentiometer on the carrier board can be used to control the current through the motor. Clockwise increases the current and heat anti-clokwise does the opposite. See the [reprap wiki](http://reprap.org/wiki/Pololu_stepper_driver_board) for details on adjusting this correctly.

### Simple operation

```python
"""
Stepper motor control example using a A4988 carrier board 
"""
import pyb

dir_pin = pyb.Pin('Y1', pyb.Pin.OUT_PP)
step_pin = pyb.Pin('Y2', pyb.Pin.OUT_PP)
enable_pin =pyb.Pin('Y3', pyb.Pin.OUT_PP)

enable_pin.high()  # high is stop
dir_pin.high()     # high is CCW looking down on shaft

enable_pin.low()
for i in range(1000):
        step_pin.high()
        pyb.udelay(2)
        step_pin.low()
        pyb.udelay(1000)

dir_pin.low()

for i in range(1000):
        step_pin.high()
        pyb.udelay(2)
        step_pin.low()
        pyb.udelay(1000)

enable_pin.high()

#Brake motor draws full current
enable_pin.low()
pyb.delay(5000)
enable_pin.high()
```

This code is about as simple as it gets. The motor should take 1000 steps one way then a 1000 steps the other way at approximately 1000 steps/second. The speed is set by the length of the delay in \`pyb.delay(1000). A larger delay means a slower speed. The enable pin is left low at the end so one can see the effect on braking the motor and the current draw from the motor power supply.

This code could be re-factored into a function with parameters for number of steps, direction and speed but its use is limited to moving a single motor at a time. The code cannot easily do anything like check for end stops while the motor is running.

### Timer operation

```python
"""
Stepper motor control example using a A4988 carrier board 
and a Timer
"""
import pyb

def disco(ms):
    """
    cycles LEDs on pyboard for ms milliseconds
    """
    leds = [pyb.LED(i) for i in range(1,5)]
    for l in leds:
        l.off()
    now = pyb.millis()
    n = 0
    try:
       while pyb.millis() <= now + ms:
          n = (n + 1) % 4
          leds[n].toggle()
          pyb.delay(50)
    finally:
        for l in leds:
            l.off()

def motor_cb1(t):
    step_pin.high()
    pyb.udelay(2)
    step_pin.low()

dir_pin = pyb.Pin('Y1', pyb.Pin.OUT_PP)
step_pin = pyb.Pin('Y2', pyb.Pin.OUT_PP)
enable_pin =pyb.Pin('Y3', pyb.Pin.OUT_PP)

enable_pin.high()  # high is stop
dir_pin.high()     # high is CCW looking down on shaft

tim1 = pyb.Timer(1, freq=1)

tim1.callback(motor_cb1)
tim1.init(freq = 1000)

enable_pin.low()
dir_pin.low()
disco(1000)
dir_pin.high()
disco(1000)
enable_pin.high()

tim1.deinit()
```

The next level of sophistication is to use a timer that is called periodically to make each step. This allows multiple motors to be run at the same time and other code to be run in the time between steps.

The above code uses a timer `tim1` and a callback function `motor_cb1`. The callback function makes a single step. The speed of the motor is set by the frequency of the timer in `tim1.init(freq = 1000)`. A larger frequency will give a faster speed.

To prove the program can do other things while turning the motor I\'m calling the function `disco` which cycles through the leds on the board.

To run more then one motor you need a separate timer and callback function for each motor.

### nemastepper.py library

The next step in refactoring this code to make a general library for stepper motors is to wrap the functionality up in a Class. Rather than do this myself I\'ve used jeffmer\'s `nemastepper.py` module available as part of [this github repository](https://github.com/jeffmer/micropython-upybbot).

```python
"""
Stepper motor control example using a A4988 carrier board 
and the nemastepper.py library available at 
https://github.com/jeffmer/micropython-upybbot
"""
import pyb

def disco(ms):
    """
    cycles LEDs on pyboard for ms milliseconds
    """
    leds = [pyb.LED(i) for i in range(1,5)]
    for l in leds:
        l.off()
    now = pyb.millis()
    n = 0
    try:
       while pyb.millis() <= now + ms:
          n = (n + 1) % 4
          leds[n].toggle()
          pyb.delay(50)
    finally:
        for l in leds:
            l.off()

from nemastepper import Stepper
motor1 = Stepper('Y1','Y2','Y3')

from pyb import Timer

def step_cb(t):
    motor1.do_step()
    
tim = Timer(8,freq=10000)
tim.callback(step_cb) #start interrupt routine

motor1.MAX_ACCEL = 1000
motor1.set_speed(1000)
disco(1000)  
motor1.set_speed(0)
motor1.set_speed(-1000)
disco(1000) 

motor1.set_speed(0)
motor1.set_off()

tim.deinit()
```

The `nemastepper` module has a Class Stepper which is initialized with the pin names for the pyboard pins connected to the dir pin, step pin and enable_pin (`motor1 = Stepper('Y1','Y2','Y3')`). Other motors connected to other pins can be initialized with more instances of `Stepper`.

The main program only needs one timer for any number of motors. The timer is set up with a frequency (f = 10000 Hz) much faster than the step rate. The callback function for this timer should call the `do_step` method for each `Stepper` instance. The speed (steps/second) of each motor is set with the `set_speed` method. The sign of the speed determines the direction. The `do_step` method uses the count and pulserate variables to decide when to take a step to achieve the required speed.

The `MAX_ACCEL` attribute of the `Stepper` class sets the maximum acceleration of the motor. `set_speed` will limit changes in speed to this value. I\'ve set the `MAX_ACCEL`to 1000 in the above example so the motor behavior is similar to the previous two examples. In a real application the desired maximum acceleration will depend on the load on the motor. Also in real application the motor speed will probably be ramped up and down in a loop rather than set to a single high value.

The next step in improving the code would be to provide commands for moving a given number of steps in a given direction implementing acceleration and deceleration. The [AccelStepper library](http://www.airspayce.com/mikem/arduino/AccelStepper/) for Arduino gives a good example of this approach but I don\'t know of such a library yet available for micropython.
