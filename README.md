# raspberrypiChompers
from time import sleep
import time
import threading import Thread
from sense_hat import SenseHat
sense = SenseHat()

a = 2 #player 1
x = 0 # player 1 y axis
y = 7 #player 2 y axis
b = 6 # player 2
r = [255, 0, 0] # red
bl = [0, 0, 255] # blue
w = [255, 255, 255] #white
bar = 4 #barrier
barX = 2 #barrier X-Axis
cl = [0, 0, 0] #clear color
playerA_position = [x,a]
barriers = [
    [cl, cl, cl, cl, cl, cl, cl, cl],
    [cl, cl, cl, cl, cl, w, w, w],
    [cl, cl, cl, cl, cl, w, cl, cl],
    [cl, cl, cl, cl, cl, cl, cl, cl],
    [cl, cl, w, cl, cl, cl, cl, cl],
    [cl, cl, w, cl, cl, cl, cl, cl],
    [cl, cl, w, cl, cl, cl, cl, cl],
    [cl, cl, cl, cl, cl, cl, cl, cl]
    ]

def playerA():
    sense.set_pixel(x, a, r) #pixel location
    
def playerB():
    sense.set_pixel(y, b, bl)

#builds barriers in white
#def barrier():
#     sense.set_pixel(barX, bar, w)
#    sense.set_pixel(barX, bar+1, w)
#     sense.set_pixel(barX, bar+2, w)
#     sense.set_pixel(barX+5, bar-3, w)
#     sense.set_pixel(barX+4, bar-3, w)
#     sense.set_pixel(barX+3, bar-3, w)
#     sense.set_pixel(barX+3, bar-2, w)


#PlayerA player movers 
def move_up(event):
    global a
    if event.action=='pressed'and a > 0:
        a -= 1
    print(event)

def move_down(event):
    global a
    if event.action=='pressed' and a < 7:
        a += 1   
    print(event)
    
def move_right(event):
    global x
    if event.action=='pressed' and x < 7:
        x += 1
    print(event)
    
def move_left(event):
    global x
    if event.action=='pressed' and x > 0:
        x -= 1
    print(event)

def time(prompt,timeout=10.0):
    timer = threading.Timer(timeout, thread.interrupt_main)
    astring = None
    try:
        timer.start()
        string = raw_input(prompt)
    except KeyboardInterruption:
        pass
    timer.cancel()
    return astring

def move_playerA(x, a, new_x, new_a):
    new_x = x
    new_a = a
    if a > 0:
        new_a -= 1
    elif a < 7:
        new_a += 1
    elif x > 0:
        new_x -= 1
    elif x < 7:
        new_x += 1
    x,y = check_barrier(x,y,new_x,new_y)
    return new_x,new_a


def check_barrier(x,a,new_x,new_a):
    if playerA[new_a][new_x] != w:
        return new_x, new_a
    elif playerA[new_a][x] != w:
       return x, new_a
    elif playerA[a][new_x] != w:
        return new_x, a
    return x,a

sense.stick.direction_up = move_up
sense.stick.direction_down = move_down
sense.stick.direction_right = move_right
sense.stick.direction_left = move_left

#makes it all appear on the board 
while True:
    sense.clear() #this clears the space after the player moves - if this isn't ear itleaves a light trail behind it so look into that
    #playerA(x,a,new_x,new_a)
    playerA()
    playerB()
    sense.set_pixels(sum(barriers,[]))
    #barrier()
