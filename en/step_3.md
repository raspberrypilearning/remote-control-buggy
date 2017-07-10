## An Android remote control

You can use the Android App, BlueDot, as a remote control for your robot buggy.

You'll need to do a little setup to begin with:

- Install the [BlueDot Android App from here](https://play.google.com/store/apps/details?id=com.stuffaboutcode.bluedot&hl=en_GB)
- On your Raspberry Pi, open a Terminal and install the Python dbus module and the BlueDot module

	```bash
	sudo apt-get install python3-dbus
	sudo pip3 install bluedot
	```

Have a look at the section below to learn the basics of using the BlueDot app with your Raspberry Pi.

[[[rpi-python-bluedot]]]

To remote control your buggy, here's what you will need to do:

- Import the `bluedot` and `gpiozero` modules
  ```python
  from bluedot import BlueDot
  from gpiozero import Robot

  bd = BlueDot()
  robot = Robot(left=(7, 8), right=(9, 10)) ##this may be different depending on your wiring
  ```
- Create a function called `move` that has `pos` as a parameter.
- The function should check if `pos.top` is True. If it is then `robot.forward()` can be used to move the robot forward.
- The function should also check `pos.bottom`, `pos.right` and pos.left`, and move the `robot` accordingly.
- Create a function called `stop`, that just stops the robot.
- When the bluedot is pressed or the finger moves, the `move` function should BA called.
- When the bluedot is released, the `stop` function should be called.

---hints--- ---hint---
Here's how you might start off your `move` function. Try and complete the rest of it on your own.

```python
def move(pos):
    if pos.top:
	    robot.forward()
```
---/hint--- ---hint---
Here's some of the code, with some key lines missing. Can you complete the code?
```python
def move(pos):
    if pos.top:
        robot.forward()
    elif pos.bottom:
        robot.backward()
    elif pos.right:
   
   
   

def stop():
   

bd.when_pressed = move
bd.when_moved = move
```
---/hint--- ---hint---
Here's a video taking you through the code writing process

---/hint--- ---/hints---
