## An Android remote control

### Requirements
For this stage, you will need an Android phone or tablet as well as your buggy.

### Instructions
You can use the Android app **Blue Dot** as a remote control for your robot buggy.

You'll need to do a little setting up first:

--- task ---
Download the [Blue Dot Android app from here](https://play.google.com/store/apps/details?id=com.stuffaboutcode.bluedot&hl=en_GB){:target="_blank"} and install it.
--- /task ---

--- task ---
On your Raspberry Pi, open a terminal window and install the `dbus` and `bluedot` Python modules.

```bash
sudo pip3 install bluedot
```
--- /task ---

Have a look at the section below to learn the basics of using the Blue Dot app with your Raspberry Pi.

[[[rpi-python-bluedot]]]

To remotely control your buggy, here's what you will need to do:

--- task ---
Open up **mu** from the **Programming** menu.
--- /task ---

--- task ---
Import the `bluedot` and `gpiozero` modules, and create `Robot` and `BlueDot` objects.
```python
from bluedot import BlueDot
from gpiozero import Robot

bd = BlueDot()
robot = Robot(left=(7, 8), right=(9, 10)) ##this may be different depending on your wiring
```
--- /task ---

--- task ---
Create a function called `move` that has `pos` as a parameter.
```python
def move(pos):
```
--- /task ---

--- task ---
The function should check if `pos.top` is `True`. If it is, then `robot.forward()` can be used to move the robot forward.
--- /task ---

--- task ---
The function should also check `pos.bottom`, `pos.right`, and `pos.left`, and move the `robot` accordingly.
--- /task ---

--- task ---
Create a function called `stop` that stops the robot.
--- /task ---

--- task ---
- When the blue dot is pressed or the finger moves, the `move` function should be called.
- When the blue dot is released, the `stop` function should be called.
--- /task ---

---hints--- ---hint---
Here's how you might start off your `move` function. Try and complete the rest of it on your own.

```python
def move(pos):
    if pos.top:
        robot.forward()
```
---/hint--- ---hint---
Here's the code with a few key lines missing. Can you complete it?
```python
def move(pos):
    if pos.top:
        robot.forward()
    elif pos.bottom:
        robot.backward()
    elif pos.right:
		## turn the robot to the right
	## What if the blue dot is touched on the left?
   
   
   

def stop():
	## stop the robot
   

bd.when_pressed = move
bd.when_moved = move
## What if the user takes her finger off the blue dot?
```
---/hint--- ---hint---
Here's a video taking you through the code writing process:
<video width="560" height="315" controls>
<source src="images/blue-dot-remote.webm" type="video/webm">
If your browser does not support WebM video, try Firefox or Chrome.
</video>

---/hint--- ---/hints---
