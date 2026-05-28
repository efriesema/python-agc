# Python Apollo Guidance computer to GPS Comparison Project
### Author: Ed Friesema

The goal of this project is to  compare the Apollo guidance computer in the Cornell Project that mapped that code to a Raspberry Pi using Python

* Rebuild Cornell university's Py-AGC  project
https://courses.ece.cornell.edu/ece5990/ECE5725_Spring2023_Projects/2%20Friday%20May%2012/11%20Apollo%20Guidance%20Computer/W_csg83_nkr33/W_csg83_nkr33/index.html

* Test speed and memory requirements versus three standard python GPS libraries 
* pynmea2,https://github.com/Knio/pynmea2 
* pynmeagps -- pip install pynmeagps
* gps3 -- pip install gps3
* Compare results with virtual AGC https://github.com/virtualagc/virtualagc
###  Field Tests

Determine if the positional accuracy of the system in the field works as expected . Use an SDR confiugred as a GPS jammer  to see how the system performs using the AGC as as opposed to the GPS libraries and test which works better under ECM attack. 
* Is there a tradeoff in accuracy versus robustness?
* What do failure modes for the AGC look like?
* What do failure modes for the GPS libraries look like?


### UAV applications



1. Hardware Architecture -- You need two brains working together to safely fly and process data simultaneously:
* The Flight Controller (FC): Pixhawk 6c --The primary brain. It manages the sensors (IMU, gyro, barometer) and calculates the exact voltage to send to each Electronic Speed Controller (ESC).
* The Companion Computer (Raspberry Pi 4B): The secondary brain. Depending on your drone's size, you can use a Raspberry Pi 4B or Raspberry Pi 5 (for heavy tasks like machine learning and 8+ GB RAM) or a Raspberry Pi Zero 2 W (for lightweight applications like basic telemetry).

2. Physical Connections -- The Raspberry Pi must communicate with the FC via a serial connection:
* UART: crimp molex connector to join the Pi's TX (Transmit) and RX (Receive) pins to an available TELEM/serial port on your flight controller.
* Baud Rate: Match the baud rate on both devices in your setup software (typically 1152000 for MAVLink).
* Power: Do not power the Pi from the FC's 5V rail as the Pi's current draw fluctuates and can cause the flight controller to brownout. Power the Pi using an independent, dedicated BEC (Battery Eliminator Circuit).

3. Software Stack -- To enable high-level communication and drone control, you need a protocol to translate your Pi's commands into flight actions:
* MAVLink: The standard messaging protocol for drones. can laso communicate via a 925 Mhz radio telemetry link
* DroneKit-Python: A popular Python library that allows you to write scripts on the Pi to tell the drone to take off, move to specific GPS coordinates, or land. https://dronekit-python.readthedocs.io/en/latest/
* APM Planner or Mission Planner: Desktop ground station software used to configure your FC's UART ports before handing over control to the Pi.
* Ardupilot -  the flight stack software that runs the pixhawk flight controller talks with the Raspberry Pi, GPS antenna, RC radio Controller and Mission Planner software

4. Advanced Applications -- Once the connection is established, your Raspberry Pi turns a standard drone into a flying robot:
* Computer Vision: Use a Pi Camera module with OpenCV to track objects or perform automated precision landings.Autonomous 
* Navigation: Run onboard AI models (like YOLO) to identify objects during flight.
* Obstacle Avoidance: Interface your Pi with a LIDAR or depth camera (like an Intel RealSense) to calculate alternate paths dynamically.For a step-by-step breakdown on soldering the Pi to a flight controller and writing a Python script to spin the propellers
* Can the resulting software be ported on to a drone package to provide an AGC backp for traditional drone navigation?  

* If the SDR jammer is attached to the drone can we simulate barrage  and spot jamming attacks and see if the new system provides robustness in the face of ECM jamming?

* Raspberry Pi and FLight controller communication via MavLink  - https://www.youtube.com/watch?v=riWIg6Yx4KI
* Make your own Pixhawk raspberry pi - https://www.youtube.com/watch?v=kB9YyG2V-nA      ---  https://dojofordrones.com/raspberry-pi-drone/
