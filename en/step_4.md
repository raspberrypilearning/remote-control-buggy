## Google Voice Kit Remote

You can use a second Raspberry Pi running the Google AIY Voice Kit software to remote control your Raspberry Pi buggy. This section assumes you have both a built buggy, and that you have followed the [AIY Voice Kit Resource](../rpi-python-google-aiy).

To control the buggy, you can use a feature in the `gpiozero` module called **Remote Pins**. have a look at the section below to learn more about how to use remote pins.

[[[rpi-python-remote-pins]]]

- Begin by connecting up your buggy and then opening a terminal on it's Raspberry Pi. Once there you can type the following into the terminal so that the PiGPIO daemon will run on boot:

	```bash
	sudo systemctl enable pigpiod
	```
	
- Next type `hostname -I` to reveal the Raspberry Pi's IP Address. Make a note of this as you will need it in a bit.

- You can now leave the buggy, as all the code you will be writing will be on the AIY Voice Kit.

- Here's what you will need to do:
    1. On the Voice Kit Raspberry Pi, in the `src` directory find the file called `action.py`
	1. Edit this file near the top, to import the neccessary components of `gpiozero`, so that you can use remote pins and the Robot class.
	1. Create a new `voice_command` that will trigger the robot action.
	1. Create a new `action`, that will move the buggy when it is issued with commands.

	
### Import the modules and set up remote pins

- With the `action.py` file open, near the top of the file, you can import the neccessary modules to remote control the robot.

	```python
	import gpiozero
	from gpiozero import Robot
	from time import sleep
	from gpiozero.pins.pigpio import PiGPIOFactory
	```
- Now you can set up the remote pins, using the IP Address of your buggy.

	```python
	factory = PiGPIOFactory(host="192.168.1.79")

	```
- Then you can set up your robot, using the remote pins.

	```python
	robot = Robot(left=(factory.pin(7), factory.pin(8)), right=(factory.pin(9), factory.pin(10)))
	```
### Creating a voice_command

- Scroll to near the bottom of the file and find the section with the following comments:

	```python
	# =========================================
	# Makers! Add your own voice commands here.
	# =========================================
	```
- Here is where you can write you custom command that will activate your robot. Some experimentation might be required here. using the command `robot` might not be a good idea, as the Google API might struggle to recognise the word. Play around with different words to see what works best.

	```python
	actor.add_keyword("red leader", ControlRobot(say))
	```

### Create you custom action

- Scroll back up the file, until you find the section with the following comments:

	```python
	# =========================================
	# Makers! Implement your own actions here.
	# ========================================
	```

- You're going to create a new `class` here called `ControlRobot`. The alogrithm works as follows:
- You start by creating the class an initialising it with the `say` function.

	```python
	class ControlRobot():
		def __init__(self, say):
			  self.say = say
	```

- Now you need a `run` method. This is where the real work is done.

	```python
	class ControlRobot():
		def __init__(self, say):
			  self.say = say

		def run(self, voice_command):
	```

- If you followed the [AIY Voice Kit Resource](../rpi-python-google-aiy), then this shuld be fairly familiar to you. Within the `run` method you need to do the folloiwng:
  1. Convert the `voice_command` to lowercase
  1. If the word `forward` is in the voice command, then send the robot forward.
  1. If the word `backward` is in the voice command, then send the robot backwards.
  
- You can continue this logic to finish the class. Have a look at the hints below if you need a little bit of help.

--- hints --- --- hint ---
- Here's how you could begin the `run` method:
  ```python
  def run(self, voice_command):
	  command = voice_command.lower()
	  if "forward" in command or "ford" in command:
		  robot.forward()
		  self.say("To infinity and beyond")
  ```
--- /hint --- --- hint ---
- Here's the full class, but with some of the code missing, for you to fill in.
  ```python
  class ControlRobot():
	  def __init__(self, say):
			self.say = say

	  def run(self, voice_command):
		  command = voice_command.lower()
		  if "forward" in command:
			  robot.forward()
			  self.say("To infinity and beyond")
		  elif "back" in command:
			  ## command to send the robot backwards
			  self.say("beep beep boop, going backwards")
		  elif ##listen for right in the command
			  robot.right(0.5)
			  self.say("You spin me right round, baby right round")            
		  elif "left" in command:
			  ## drive the robot left
		  elif "stop" in command:
			  robot.stop()
			  self.say("emergency halt procedure executed")        
  ```
--- /hint --- --- hint ---
- Here's a video detailing how to code the Voice Kit, along with explanatory commentary
<video width="560" height="315" controls>
<source src="images/aiy-remote.webm" type="video/webm">
Your browser does not support WebM video, try FireFox or Chrome
</video>

--- /hint --- --- /hints ---

