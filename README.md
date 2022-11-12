1st_assignment_RT1
================================

This Repository is for the first assigmnent of Research Track 1

Introduction
------------

The task we have to accomplish is **to let the robot deliver silver tokens to gold tokens respectively**.

Installing and Running
----------------------

The simulator requires a Python 2.7 installation, the [pygame](http://pygame.org/) library, [PyPyBox2D](https://pypi.python.org/pypi/pypybox2d/2.1-r331), and [PyYAML](https://pypi.python.org/pypi/PyYAML/).

If you can't install **Python-pygame**, this command will help you.

```bash
$ sudo pip2 install pygame
```

also, if you encount some errors when you try installing **pypybox2d**, check if python2-dev is installed properly.

```bash
$ sudo apt-get install python2-dev
```

Running
-------

To run the assignment in the simulator, use `run.py`, passing it the file name `assignment1.py`, as the following:

```bash
$ python2 run.py assignment.py
```

In the environment, you will find the components below.

1. the robot
   
   ![alt text](https://github.com/kazu610/1st_assignment_RT1/blob/main/sr/robot.png)
2. the silver tokens
   
   ![alt text](https://github.com/kazu610/1st_assignment_RT1/blob/main/sr/token_silver.png)
3. the gold tokens
   
   ![alt text](https://github.com/kazu610/1st_assignment_RT1/blob/main/sr/token.png)

Robot API
---------

The API for controlling a simulated robot is designed to be as similar as possible to the [SR API][sr-api].

### Motors ###

The simulated robot has two motors configured for skid steering, connected to a two-output [Motor Board](https://studentrobotics.org/docs/kit/motor_board). The left motor is connected to output `0` and the right motor to output `1`.

The Motor Board API is identical to [that of the SR API](https://studentrobotics.org/docs/programming/sr/motors/), except that motor boards cannot be addressed by serial number. So, to turn on the spot at one quarter of full power, one might write the following:

```python
R.motors[0].m0.power = 25
R.motors[0].m1.power = -25
```

In the code, there are two functions to move the robot.

| NAME | FEATURE |
|:---------------:|:----------------:|
| `forward(speed, seconds)` | to move forward for `seconds` at `speed` |
| `turn(speed, seconds)` | to turn for `seconds` at angular velocity based on `speed` |

### Grabber ###

The robot is equipped with a grabber, capable of picking up a token which is in front of the robot and within 0.4 metres of the robot's centre. To pick up a token, call the `R.grab` method:

```python
success = R.grab()
```

The `R.grab` function returns `True` if a token was successfully picked up, or `False` otherwise. If the robot is already holding a token, it will throw an `AlreadyHoldingSomethingException`.

To drop the token, call the `R.release` method.

Cable-tie flails are not implemented.

### Vision ###

To help the robot find tokens and navigate, each token has markers stuck to it, as does each wall. The `R.see` method returns a list of all the markers the robot can see, as `Marker` objects. The robot can only see markers which it is facing towards.

Each `Marker` object has the following attributes:

* `info`: a `MarkerInfo` object describing the marker itself. Has the following attributes:
  * `code`: the numeric code of the marker.
  * `marker_type`: the type of object the marker is attached to (either `MARKER_TOKEN_GOLD`, `MARKER_TOKEN_SILVER` or `MARKER_ARENA`).
  * `offset`: offset of the numeric code of the marker from the lowest numbered marker of its type. For example, token number 3 has the code 43, but offset 3.
  * `size`: the size that the marker would be in the real game, for compatibility with the SR API.
* `centre`: the location of the marker in polar coordinates, as a `PolarCoord` object. Has the following attributes:
  * `length`: the distance from the centre of the robot to the object (in metres).
  * `rot_y`: rotation about the Y axis in degrees.
* `dist`: an alias for `centre.length`
* `res`: the value of the `res` parameter of `R.see`, for compatibility with the SR API.
* `rot_y`: an alias for `centre.rot_y`
* `timestamp`: the time at which the marker was seen (when `R.see` was called).

For example, the following code lists all of the markers the robot can see:

```python
markers = R.see()
print "I can see", len(markers), "markers:"

for token in R.see():
        if token.dist < dist and token.info.marker_type is MARKER_TOKEN_SILVER:
            ...
            ...
```

Code Description
----------------



[sr-api]: https://studentrobotics.org/docs/programming/sr/