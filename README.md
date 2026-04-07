---
# Dhananjay_Mars_Task_1
---
## Detailing of My Mars Tasks and Mars Projects
---
### Task:1 Blinking LED with different time interval

This project demonstrates how to bypass the limitations of the delay() function in Arduino to achieve multitasking. Using three LEDs with distinct blinking intervals (500ms, 1000ms, and 1500ms), the firmware utilizes the millis() function to track elapsed time independently for each pin.

Link: https://www.tinkercad.com/things/kPosycJlizS-question-1-blinking-led-with-different-time-interval
---
### Task:2 Controlling colour of RGB LED

This project demonstrates the versatile use of a single analog input to control multiple asynchronous outputs. Using an Arduino Uno and a potentiometer, the system concurrently manages the color spectrum of an RGB LED and the blinking rate of a standard LED.

Link: https://www.tinkercad.com/things/8Nii45agjEV-question-2-controlling-colour-of-rgb-led?sharecode=HGkTNXOxJ9nP1MpadRZuY7_gZAuZBr-7_flWgAKu5Mw
---
### Task:3 Build a reaction time tester

This project is a high-precision reaction time tester designed to measure the latency between a visual stimulus (LED) and a physical response (button press). It utilizes the Arduino's internal timing crystal to provide millisecond-accurate feedback via the Serial Monitor.

Link: https://www.tinkercad.com/things/gaUkBOC1bK6-question-3-build-a-reaction-time-tester
---
### Project:1 Light Adaptive Servo Control System

What the project does:
This project creates a "Smart Hinge" system where the angular position of a servo motor is directly proportional to the intensity of light falling on a photoresistor (LDR). It mimics the behavior of a standard potentiometer but replaces manual mechanical input with environmental light data.

Why I chose it:
I chose this project to explore how analog environmental data can be translated into mechanical positioning. In an ECE context, this is the foundation for solar-tracking panels or automated light-shutter systems.

Components and Their Roles
- Arduino Uno: The central processing unit that converts the analog voltage from the LDR into a digital signal using its 10-bit ADC.
- Photoresistor (LDR): A semiconductor component that changes its resistance based on light intensity. It acts as the "eyes" of the system.
- Servo Motor: The output actuator that provides physical motion. It responds to PWM (Pulse Width Modulation) signals from the Arduino.
- 10k $\Omega$ Resistor: Used in a Voltage Divider configuration with the LDR. This is essential to convert the LDR's change in resistance into a measurable change in voltage at pin A5.
- Jumper Wires & Breadboard: Used for signal routing and creating a common ground between the sensor and the motor.

The Priority Logic Tree: The logic follows a simple Sense-Process-Act sequence that translates environmental light into mechanical movement:

- Sense (Analog Input): The Arduino reads the voltage from the Photoresistor (LDR) using analogRead(). Because of the 10k $\Omega$ voltage divider, more light creates a higher voltage ($0\text{V} \to 5\text{V}$), which the Arduino sees as a digital value from 0 to 1023.
- Process (Data Mapping): The code uses the map() function to scale that 10-bit sensor value into the servo's physical constraints. It translates the $0\text{-}1023$ range into a $0\text{-}180$ degree range.
- Act (PWM Output): The calculated angle is sent to the Servo Motor via the myServo.write() command. This generates a specific Pulse Width Modulation (PWM) signal that moves the motor's gears to the corresponding position.
- Repeat: A small delay(15) is used to allow the motor time to physically reach the new position before the next sensor reading is processed, ensuring smooth, jitter-free motion.

Logic Diagram: 

<img width="1316" height="195" alt="image" src="https://github.com/user-attachments/assets/c627a211-c12e-4f0a-b8d9-4439833d4e71" />

Challenge 1: Rough and Jerky Movement of the Servo
- The Problem: The code uses a two-step mapping process: converting 0–1023 (ADC) to 0–5 (Volts), and then 0–5 to 0–180 (Degrees). Because int variables truncate    decimals, mapping 1024 values down to just 6 (0, 1, 2, 3, 4, 5) caused the servo to move in large, jerky $36^{\circ}$ jumps.
- The Solution: While the two-step map is great for understanding voltage levels, for a smoother response, I realized a direct map from analogRead (0-1023) to servoAngle (0-180) would provide 1024 points of resolution, resulting in much more fluid motor motion.

Challenge 2: Power Fluctuations

- The Problem: Servos can draw significant current when moving, which sometimes caused the Arduino to reset or the LDR readings to fluctuate due to "noise" on the 5V rail.
- The Solution: I ensured the servo and sensor shared a solid common ground and added a small delay(5) in the loop to allow the ADC to stabilize between readings, preventing erratic jitter.

Link: https://www.tinkercad.com/things/gmBNIhXrn7Z-project-1-controlling-the-servo-with-the-photoresistor
---
### Project:2 Project Ares – Intelligence-Based Martian Airlock Control

What the project does:
Project Ares is a smart airlock system designed for a Martian habitat. It integrates human detection (PIR), environmental monitoring (Photoresistor), and mechanical actuation (Servo) to manage entry protocols. Unlike a standard door, this system uses a "Priority Logic Tree" that can override user requests based on exterior safety conditions, such as high-intensity dust storms.

Why I chose it:
I wanted to explore how Firmware Logic can handle conflicting inputs. On Mars, an astronaut wanting to enter is a priority, but a severe dust storm is a mission-ending threat. This project simulates the decision-making process required for autonomous off-world habitats.

Components and Their Roles
- Arduino Uno: The primary microcontroller executing the conditional logic.
- PIR Sensor (D2): Detects infrared radiation from moving astronauts to trigger the entry sequence.
- Photoresistor / LDR (A0): Monitors ambient light levels to calculate "Storm Intensity" based on semiconductor conductivity.
- Servo Motor (D9): Operates the airlock hatch, providing precise angular control ($0^{\circ}$ for Sealed, $90^{\circ}$ for Open).
- Piezo Buzzer (D8): Provides multimodal feedback—standard 3-beep entry tones, continuous warning beeps for emergencies, and high-pitched sirens for critical failures.
- 10k $\Omega$ Resistor: Acts as a pull-down resistor in a voltage divider circuit to stabilize analog light readings.

The Priority Logic Tree
The system evaluates conditions in order of Severity, not just detection.

- Level 1: Critical (Storm > 75%) – The hatch is locked immediately. Safety of the base overrides all entry requests.
- Level 2: Emergency Entry (Storm 50-75% + Motion) – If a storm is approaching but not critical, an astronaut is given a limited 10-second window to enter before the airlock seals.
- Level 3: Pre-emptive Lockdown (Storm > 50%) – If no person is present, the hatch remains sealed to prevent dust accumulation.
- Level 4: Standard Entry (Storm < 50% + Motion) – Normal operation with a 5-second entry window and a "welcome" 3-beep audio signal.
- Level 5: Idle – System remains in low-power monitoring mode.

Logic Diagram:

<img width="1029" height="693" alt="image" src="https://github.com/user-attachments/assets/ed53ff94-3014-42e6-b694-1c5a6501bdf8" />

Challenge 1: The Logic Conflict (Astronaut vs. Storm)
- The Problem: Determining when it is "too late" for an astronaut to enter safely.
- The Solution: I utilized Nested If-Else statements. By placing the "Critical Storm" check at the very top of the loop(), the code ensures that if the environment is too dangerous, the PIR sensor input is completely ignored, prioritizing the habitat's structural integrity.

Challenge 2: Audio Feedback Clarity
- The Problem: Differentiating between a "Welcome" and an "Emergency."
- The Solution: I programmed distinct frequency tiers. High frequencies ($2000\text{Hz}$) indicate immediate danger, while lower frequencies ($500\text{Hz}$) indicate a general warning. The 3-beep sequence for standard entry was achieved using a for loop to ensure a clear, recognizable user interface (HMI).

Link: https://www.tinkercad.com/things/04UTzWA1hFX-project-2intelligence-based-martian-airlock-control
---
