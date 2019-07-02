## AIY Projects Voice Kit remote

### Requirements
For this part of the project, you will need a Google AIY Projects Voice Kit and a second Raspberry Pi as well as your buggy.

### Instructions
You can use a second Raspberry Pi running the Google AIY Projects Voice Kit software to remotely control your Raspberry Pi buggy. This section assumes you have both a [built buggy](https://projects.raspberrypi.org/en/projects/build-a-buggy){:target="_blank"}, and that you have an assembled [AIY Projects  Voice Kit](https://projects.raspberrypi.org/en/projects/rpi-aiy-voice-assemble){:target="_blank"}.

To control the buggy, you can use a feature in the `gpiozero` module called **remote pins**. Have a look at the section below to learn more about how to remotely use GPIO pins.

[[[rpi-python-remote-pins]]]

--- task ---
Connect up your buggy and then open a terminal window on its Raspberry Pi. Once there, you can type the following so that the **pigpio daemon** will run on boot:

```bash
sudo systemctl enable pigpiod
```
--- /task ---

--- task ---
Next, type in `hostname -I` to show the Raspberry Pi's IP address. Make a note of it, as you will need it in a bit.
--- /task ---	

You can now leave the buggy, as all the other code you will be writing will be for the AIY kit.
	
### Import the modules and set up remotely controlled pins

--- task ---
Open the `action.py` file and, near the top, add code to import the neccessary modules to remotely control the robot.

```python
from gpiozero import Robot
from gpiozero.pins.pigpio import PiGPIOFactory
```

- Now you can set up the remote pins using the IP address of your buggy.

```python
factory = PiGPIOFactory(host="192.168.1.79")

```

- Then you can set up your robot with these pins.

```python
robot = Robot(left=(7, 8), right=(9, 10), pin_factory=factory))
```
--- /task ---
### Creating a voice command

--- task ---
Scroll to near the bottom of the `action.py` file and find the section with the following comments:

```python
# =========================================
# Makers! Add your own voice commands here.
# =========================================
```
Here is where you can write you custom command that will activate your robot. This might require some experimenting. Using the command 'robot' may not be a good idea, as the Google API might struggle to recognise the word. Play around with different command words to see which one works best.

```python
actor.add_keyword("red leader", ControlRobot(say))
```
--- /task ---

### Create you custom action

--- task ---
- Scroll back up in the file until you find the section with the following comments:

```python
# =========================================
# Makers! Implement your own actions here.
# ========================================
```

- Start by creating the class and initialising it with the `say` function.

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
--- /task ---


- If you followed our [AIY Projects Voice Kit resource](https://projects.raspberrypi.org/en/projects/google-voice-aiy){:target="_blank"}, then this should be fairly familiar to you. Within the `run` method you need to do the following:

  1. Convert the `voice_command` to lower case
  1. If the word `forward` is in the voice command, then send the robot forward
  1. If the word `backward` is in the voice command, then send the robot backwards
  
You can continue this logic to finish the class. Have a look at the hints below if you need a little bit of help.

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
- Here's the full class, but with some gaps in the code for you to fill in.
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
		  elif ##listen for 'right' in the command
			  robot.right(0.5)
			  self.say("You spin me right round, baby, right round")            
		  elif "left" in command:
			  ## drive the robot left
		  elif "stop" in command:
			  robot.stop()
			  self.say("emergency halt procedure executed")        
  ```
--- /hint --- --- hint ---
- Here's a video detailing how to code the Voice Kit, along with explanatory commentary:
<video width="560" height="315" controls>
<source src="images/aiy-remote.webm" type="video/webm">
If your browser does not support WebM video, try Firefox or Chrome.
</video>

--- /hint --- --- /hints ---
