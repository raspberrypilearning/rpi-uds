## Using an ultrasonic distance sensor

In air, sound travels at a speed of 343 metres per second. An ultrasonic distance sensor sends out pulses of ultrasound which are inaudible to humans, and detects the echo that is sent back when the sound bounces off a nearby object. It then uses the speed of sound to calculate the distance from the object.

![Ultrasonic distance sensor](images/ultrasonic-distance-sensor.png)

### Wiring

The circuit connects to two GPIO pins (one for echo, one for trigger), the ground pin, and a 5V pin. You'll need to use a pair of resistors (330Ω and 470Ω) as a potential divider:

![wiring](images/wiring-uds.png)

### Code

To use the ultrasonic distance sensor in Python, you need to know which GPIO pins the echo and trigger are connected to.

--- task ---

Use the following code to read the `DistanceSensor`.

    ```python
    from gpiozero import DistanceSensor
    ultrasonic = DistanceSensor(echo=17, trigger=4)
    while True:
        print(ultrasonic.distance)
    ```

--- /task ---

--- task ---

The value should get smaller the closer your hand is to the sensor. Press **Stop** to exit the loop.

--- /task ---



### Ranges

As well as being able to see the distance value, you can also get the sensor to do things when the object is in or out of a certain range.

--- task ---

Use a loop to print different messages when the sensor is in range or out of range:

    ```python
    while True:
        ultrasonic.wait_for_in_range()
        print("In range")
        ultrasonic.wait_for_out_of_range()
        print("Out of range")
    ```

--- /task ---

--- task ---

Now wave your hand in front of the sensor; it should switch between showing the message "In range" and "Out of range" as your hand gets closer and further away from the sensor. See if you can work out the point at which it changes.

--- /task ---


--- task ---

The default range threshold is 0.3m. This can be configured when the sensor is initiated:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, threshold_distance=0.5)
    ```

    Alternatively, this can be changed after the sensor is created, by setting the `threshold_distance` property:

    ```python
    ultrasonic.threshold_distance = 0.5
    ```

--- /task ---

--- task ---

Try the previous loop again and observe the new range threshold.

--- /task ---

The `wait_for` functions are **blocking**, which means they halt the program until they are triggered. Another way of doing something when the sensor goes in and out of range is to use `when` properties, which can be used to trigger actions in the background while other things are happening in the code.

--- task ---

Create a function for what you want to happen when the sensor is in range:

    ```python
    def hello():
        print("Hello")
    ```

--- /task ---

--- task ---

Then set `ultrasonic.when_in_range` to the name of this function:

    ```python
    ultrasonic.when_in_range = hello
    ```

--- /task ---

--- task ---

Add another function for when the sensor goes out of range:

    ```python
    def bye():
        print("Bye")

    ultrasonic.when_out_of_range = bye
    ```

--- /task ---

Now these triggers are set up, you'll see "hello" printed when your hand is in range, and "bye" when it's out of range.

--- task ---

1m is the default maximum distance and can also be configured on setup:

    ```python
    ultrasonic = DistanceSensor(echo=17, trigger=4, max_distance=2)
    ```

Or after setup:

    ```python
    ultrasonic.max_distance = 2
    ```

--- /task ---

--- task ---

Try different values of `max_distance` and `threshold_distance`.

--- /task ---