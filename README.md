# 16x2-lcd-4bit-mode-raspberrypi-pico

import machine
import utime

d4=machine.Pin(2,machine.Pin.OUT) # datapin d4 pin of lcd
d5=machine.Pin(3,machine.Pin.OUT) # datapin d3 pin of lcd
d6=machine.Pin(4,machine.Pin.OUT) # datapin d4 pin of lcd
d7=machine.Pin(5,machine.Pin.OUT) # datapin d5 pin of lcd
rs=machine.Pin(6,machine.Pin.OUT) # rs pin of lcd
rw=machine.Pin(7,machine.Pin.OUT) # R/W pin of lcd
en=machine.Pin(8,machine.Pin.OUT) # enable pin of lcd


def lcd_data(data):
    
    #sending data higher nibble
    d4.value(0)
    d5.value(0)
    d6.value(0)
    d7.value(0)
   
   #sepration of individual bit
    if data&0x10==0x10:
        d4.value(1)
    if data&0x20==0x20:
        d5.value(1)
    if data&0x40==0x40:
        d6.value(1)
    if data&0x80==0x80:
        d7.value(1)
    
    #sending the lcd signal 
    rs.value(1)
    rw.value(0)
    en.value(1)
    utime.sleep(0.001)
    en.value(0)
    
    #sending the lower nibble 
    
    d4.value(0)
    d5.value(0)
    d6.value(0)
    d7.value(0)
    if data&0x01==0x01:
        d4.value(1)
    if data&0x02==0x02:
        d5.value(1)
    if data&0x04==0x04:
        d6.value(1)
    if data&0x08==0x08:
        d7.value(1)

   
    rs.value(1)
    rw.value(0)
    en.value(1)
    utime.sleep(0.001)
    en.value(0)
    
def lcd_cmd(cmd):
    
    d4.value(0)
    d5.value(0)
    d6.value(0)
    d7.value(0)
    
    if cmd&0x10==0x10:
        d4.value(1)
    if cmd&0x20==0x20:
        d5.value(1)
    if cmd&0x40==0x40:
        d6.value(1)
    if cmd&0x80==0x80:
        d7.value(1)
        
    
    rs.value(0)
    rw.value(0)
    en.value(1)
    utime.sleep(0.001)
    en.value(0)
    
    
    
    d4.value(0)
    d5.value(0)
    d6.value(0)
    d7.value(0)
    
    if cmd&0x01==0x01:
        d4.value(1)
    if cmd&0x02==0x02:
        d5.value(1)
    if cmd&0x04==0x04:
        d6.value(1)
    if cmd&0x08==0x08:
        d7.value(1)

    rs.value(0)
    rw.value(0)
    en.value(1)
    utime.sleep(0.001)
    en.value(0)


def lcd_print(st):
    i=0
    while(i<len(st)):
        lcd_data(ord(st[i]))
        i=i+1
        
def lcd_init():
    lcd_cmd(0x33)
    utime.sleep(0.001)
    lcd_cmd(0x32)
    utime.sleep(0.001)
    lcd_cmd(0x28)
    utime.sleep(0.001)
    lcd_cmd(0x01)
    utime.sleep(0.001)
    lcd_cmd(0x0c)
    utime.sleep(0.001)
    lcd_cmd(0x0e)
    utime.sleep(0.001)
    
def lcd_clear():
    lcd_cmd(0x01)
    utime.sleep(0.001)
    
def lcd_2line():
    lcd_cmd(0xc0)
    utime.sleep(0.001)
    
def lcd_1line():
    lcd_cmd(0x80)
    utime.sleep(0.001)
    lcd_cmd(0x0c)
    utime.sleep(0.001)

def shift_right_display():
    lcd_cmd(0x1c)
    utime.sleep(0.001)

def shift_left_display():
    lcd_cmd(0x1c)
    utime.sleep(0.001)


lcd_init()
# scrolling of "HELLO WORLD" text
while True:
   
    shift_right_display()
    lcd_print("HELLO WORLD")
    lcd_1line()
    utime.sleep(0.3)
    
        
        
    
    

