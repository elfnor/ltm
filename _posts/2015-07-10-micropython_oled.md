---
author: elfnor
date: '2015-07-10 22:00'
image: 'wiring_02.png'
layout: post
tags:  python micropython
title: 'Micropython - OLED module with the ssd1306 chip'
---

Small OLED modules are [available](http://www.dx.com/p/waveshare-0-96-oled-b-ssd1306-display-screen-module-deep-blue-340467#.VZ9gRqGW4y8) with a resolution 128 x 64 pixels on a 22 x 11 mm display for under seven dollars. They are great for displaying graphics that can\'t be shown on a standard alphanumeric LCD. [OLED](https://en.wikipedia.org/wiki/OLED) screens work without a back light and produce bright high contrast displays with a wide viewing angle (\>160°) and low power consumption (~20 mA). The monochrome versions are available with a white or blue display on a black background, or as a version where the top quarter is yellow and the the rest blue. The frame rates are very good and the display can be updated by the pyboard as fast as 333 frames/second.

The driver chip included on these modules is a ssd1306 chip that can be set up to work with either a SPI or an I2C bus. Some modules come with only one bus wired up, other use a soldered jumper or resistor to select between the two bus types. I\'m using a 128 x 64 pixel display using an SPI interface.

There are some ssd1306 libraries available for micropython but I\'m going to start with a basic example using only the micropython SPI class. I find writing a very simple example like this is a good way to understand the hardware and how to get the micro controller to work with it. Once I\'ve got the simple example going I know my hardware and wiring are correct and I\'m happy to use someone else\'s library to write more complicated stuff.

The wiring for the OLED and pyboard are shown in the top diagram. The pyboard has two SPI ports, pins X5 to X8 on one side of the board and Y5 to Y8 on the other. The ssd1306 library defaults to the Y pins, but this can be changed in the module. The [SPI bus](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) works faster than the I2C bus and has a separate line for master to slave (MOSI) communication than for slave to master (MISO). The ssd1306 on the OLED display (slave) doesn\'t send any data back to the pyboard (master) so we don\'t need to connect this pin. The slave select (SS) or chip select (CS) line is not used by the micropython SPI class and can also be ignored.

## Basic Example code

```python
"""
Simple test of a 128 x 64 pixel OLED display with an ssd1306 driver chip on a SPI bus

|OLED |PYB |
|VCC  |3V3 |
|GND  |GND|
|NC   |-- |
|DIN  |Y8 MOSI |
|CLK  |Y6 SCK |
|CS   | --|
|D/C  |Y4 |
|RES  |Y3 |
"""

import pyb
             
# from adafruit arduino
init_cmds = [0xAE, 0xD5, 0x80, 0xA8, 0x3F, 0xD3, 0x0, 0x40, 0x8D, 0x14, 0x20, 0x00, 0xA1, 0xC8,
             0xDA, 0x12, 0x81, 0xCF, 0xd9, 0xF1, 0xDB, 0x40, 0xA4, 0xA6, 0xAF]
              
display_cmds = [0x21, 0, 127, 0x22, 0, 7]
              
## set up SPI bus              
rate = 8000000

spi = pyb.SPI(2, pyb.SPI.MASTER, baudrate=rate, polarity=0, phase=0)
dc  = pyb.Pin('Y4',  pyb.Pin.OUT_PP, pyb.Pin.PULL_DOWN)
res = pyb.Pin('Y3', pyb.Pin.OUT_PP, pyb.Pin.PULL_DOWN)

def write_command(cmd):
    """
    write single command byte to ssd1306
    """
    dc.low()
    spi.send(cmd)    

## power on
res.high()
pyb.delay(1)
res.low()
pyb.delay(10)
res.high()

## init display
for cmd in init_cmds:
    write_command(cmd)

## clear display
buffer = bytearray(1024)
for cmd in display_cmds:
    write_command(cmd)
dc.high()
spi.send(buffer)

## line grid
b = [1] * 1024  # 8 horizontal lines

# set every  16th byte to 255
# should give 8 vertical lines
for i in range(0, 1024, 16):   
    b[i] = 255

#color in the 1st block to see where it is
for i in range(0,16):   
    b[i] = 255
        
buffer = bytearray(b)

for cmd in display_cmds:
    write_command(cmd)
dc.high()
spi.send(buffer) 
```

There are two list of command bytes. The first list `init_cmds` sets up the screen and turns it on. The second set `display_cmds` is used before sending an array of bytes to fill the screen display. The baud rate or clock frequency for the SPI bus does not seem to matter. I can get it to work from at least 8 kHz to 16 MHz. Also the polarity and phase settings don\'t seem to matter here.

The `dc` pin is used to let the ssd1306 know whether the bytes coming down the `DIN` line are commands (`dc` high ) or data for the screen (`dc` low). The `rs` pin is used to reset the display when it is pulled low for 10 ms.

The display is 128 x 64 pixels = 8192 bits where for each bit a 1 represents a pixel on and 0 a pixel off. The buffer for the display is written to using a byte array of 1024 bytes (8192/8). Each byte represents 8 pixels arranged in a vertical block. The bytes/blocks are ordered in rows of 128 across the display. The position of the first byte can be changed with the 13th byte of `init_cmds`. Setting it to `0xA1` puts the 1st byte in the top left corner, `0xB0` puts the 1st byte in the top right.

The example code draws a grid of 8 horizontal and 8 vertical lines on the screen and colors in the first block to check orientation.

# ssd1306 library example

There are several versions and forks of a ssd1306 library for micropython on github. They all seem to be based on one originally written by [Kenneth Henderick](https://github.com/nvbn/micropython-drivers). I have used the version from the [upybot project](https://github.com/jeffmer/micropython-upybbot) which adds a basic text display capability. The screen and pixel setting commands are written so that the origin is in the bottom left.

I also found a graphics library for drawing lines, triangles, rectangles and circles on this [forum post](http://forum.micropython.org/viewtopic.php?f=5&t=195&p=873&hilit=lcd_gfx#p873) by polygontwist.

```python
# hello world on SSD1306
# uses version of SSD1306.py and font.py
# https://github.com/jeffmer/micropython-upybbot/blob/master/ssd1306.py
# graphics library lcd_gfx.py from 
# http://forum.micropython.org/viewtopic.php?f=5&t=195&p=873&hilit=lcd_gfx#p873

import pyb
from ssd1306  import SSD1306
import lcd_gfx

oled = SSD1306(pinout={'dc': 'X4',
                       'res': 'X3'},
               height=64,
               external_vcc=False)
              

oled.poweron()
oled.init_display()

oled.clear()  
## text
oled.text('Hello World!', 20, 55)
oled.display()
pyb.delay(2000)

## draw straight lines
for x in [0, 127]:
    for y in range(64):
        oled.pixel(x, y, True)
for y in [0, 47, 48, 63]:
    for x in range(127): 
        oled.pixel(x, y, True)
    
oled.display()
pyb.delay(2000)             

## whole buffer        
b = [1] * 1024
for i in range(0, 1024, 16):   
    b[i] = 255
for i in range(0, 16):
    b[i] = 255
    
oled.buffer = bytearray(b)
oled.display()

pyb.delay(2000)  
oled.clear() 

## graphics example
lcd_gfx.drawCircle(35, 50, 10, oled, 1)
lcd_gfx.drawFillCircle(91, 50, 10, oled, 1)
lcd_gfx.drawLine(40, 20, 63, 5, oled, 1) 
lcd_gfx.drawLine(63, 5, 85, 5, oled, 1) 
lcd_gfx.drawFillTrie(63, 50, 63, 20, 50, 20, oled, 1)
lcd_gfx.drawTrie(63, 50, 63, 20, 78, 20, oled, 1)
lcd_gfx.drawRect(0, 0, 5, 5, oled, 1)
lcd_gfx.drawFillRect(122, 58, 5, 5, oled, 1)
oled.display()
```

This code displays some text, waits 2 seconds then adds some lines. It then displays the grid as in the first example and finishes off with examples of the shapes available in the `lcd_gfx.py` library. Note that due to the way the method `pixel` is implemented, the origin for all the graphics and text positions is the bottom left. However setting the raw buffer shows the screen origin is at the top right.

After the text and graphics are set up using calls to `oled.text()`, `oled.pixel()`, or any of the `lcd_gfx` functions, `oled.display` is called to send the buffer to the display.

The code for all these examples is available on my [github repo](https://github.com/elfnor/micropython-blog-examples)

The ssd1306 library can also be used with an OLED wired to use the I2C bus.

------------------------------------------------------------------------
