# Automation Programmable Logic Controller Assignment

![Banner](https://github.com/user-attachments/assets/0dd915ff-b492-4453-b657-032d4e9d074a)

---


>[!NOTE]
>To run the application in development mode, Siemens Logo is required on the end host (your machine).

Introduction
This assignment consists of the practical application done in Siemens Logo Soft Comfort to solve the problem presented under the Introduction to Automation PLC practical assignment. The problem presented is the simulation of an apple packing plant that operates based on two conveyor lines. The system utilizes a Programmable logic controller, PLC, to do this process flow:
1.	Transfer boxes to a specific point on the conveyor line.
2.	Transfer 15 apples into each box.
3.	Signal feedback based on a Light emitting diode, LED, beacon tower.
4.	The beacon tower should switch on different lights based on different situations
5.	Error control using Stop buttons, counters, lights for signals and pre-emptive start or stop behaviour.
PLCs are industrially used to automate repetitive task sequences. PLCs correlate with various manufacturing processes that take input account the given input and outputs. The selected PLC software for this assignment is Siemens Logo Soft Comfort V8.4 because the PLC software is optimized for small, scaled projects such as this one.  The modular design allows for interchangeability amongst the components. The software offers an intuitive design that aligns with professional standards, automatic configuration of communication and display within the network view, completely programmable simulation of all function states, parameters as well as current values and it offers free scalable upgrades that supported by Siemens Logo. 

Block Diagram
The following block diagram represents the systematic process of the apple packaging plant. The diagram was constructed with the use of Draw.io as it offers an extensive list of diagram features to map out various architectural models.

![image](https://github.com/user-attachments/assets/3a22ccae-50f9-483e-a167-de713cb9bcaf)

Process Build
The section henceforth is the code snapshots of the apple packaging plant process demonstrated in Siemens Logo Soft Comfort V8.4. The circuit diagram is displayed in Ladder Logic as opposed to Function Block Diagrams (FBD).
The circuit consists of 16 unique individual components. These are:

| **Component**           | **Contact**         | **Address** | **Type**  | **Details**                  |
|--------------------------|---------------------|-------------|-----------|------------------------------|
| Apple Conveyor Start     | Normally Open      | I1          | Input     | Momentary Make Pushbutton    |
| Apple Conveyor Stop      | Normally Closed    | I2          | Input     | Momentary Break Pushbutton   |
| Box Conveyor Start       | Normally Open      | I3          | Input     | Momentary Make Pushbutton    |
| Box Conveyor Stop        | Normally Closed    | I4          | Input     | Momentary Break Pushbutton   |
| Part Sensor: SE1         | Normally Open      | I5          | Input     | Momentary Make Pushbutton    |
| Box Sensor: SE2          | Normally Open      | I6          | Input     | Switch                       |
| Apple Counter Reset      | Normally Open      | I7          | Input     | Momentary Make Pushbutton    |

| **Component**           | **Connection**      | **Address** | **Type**  | **Details**                  |
|--------------------------|---------------------|-------------|-----------|------------------------------|
| Apple Conveyor           | Driven by motor    | Q1          | Output    | None                         |
| Box Conveyor             | Driven by motor    | Q2          | Output    | None                         |
| Blue Light               | LED Indicator      | Q3          | Output    | None                         |
| Orange Light             | LED Indicator      | Q4          | Output    | None                         |
| Red Light                | LED Indicator      | Q5          | Output    | None                         |

| **Component**           | **Connection**               | **Address** | **Type**  | **Details**                  |
|--------------------------|------------------------------|-------------|-----------|------------------------------|
| Apple Counter            | Incremented by I5           | C001        | Counter   | Reset by I7                  |
| Apple Conveyor Latch     | Retains power for Q1        | SF003       | Latch     | Switched off by I2           |
| Overflow Counter         | None                        | C004        | Counter   | Additional Counter           |
| Box On-Delay Timer       | Timer to start Q2           | T002        | Timer     | None                         |


Process Simulation
Please note that all screenshots are of simulation and not an exhaustive process of the actual PLC process and conditions in every space. The description paragraphs refer to the image above them.

![image](https://github.com/user-attachments/assets/85e4da1b-7d10-4b23-b048-e47ffd2ded23)


In the initial state: both conveyors are off, the Apple Counter is equal to 0 and the Blue Light (Q3) is on since the Apple Counter is less than 15. A Momentary Make Pushbutton has been used for the Apple Conveyor Start button due to the requirements of making momentary contact. It is a make as the button is used to send the initial signal to the Apple Conveyor if all prerequisites are met: 
1.	There is a box in place to receive apples (Box Sensor is high).
2.	There are less than 15 apples in the box (Apple Counter <15).
3.	The Box Conveyor is off.

![image](https://github.com/user-attachments/assets/d428fc46-6b6f-4e6d-9f74-77164c972720)

The illustration above demonstrates that the Apple Conveyor is unable to turn on unless all the Conditions are met, in this case there is no box detected in place (Box sensor is low). I have used a Switch to toggle on and off the indication of a box since it is a fixed condition not a momentary one (a box will arrive and stay until all 15 apples are in).

![image](https://github.com/user-attachments/assets/8bb3e46d-5383-4175-9355-c33cd94f183c)


The sequence above is the first action when all conditions are met, (in this example the box is in place before any conveyor is on thus Box Sensor is high). The Apple Conveyor is connected to a reference input of the Apple Conveyor Latch which retains power only if all following conditions are false:
1.	The Apple Conveyor Stop button has been pressed.
2.	There are enough apples in the box (Apple Counter=15).
3.	The box that was in place is gone (Box Sensor low).
The Box Sensor in the 4th rung is a normally closed component as it reflects the opposite truth condition when compared to its counterpart in the 1st rung (The box is in place the apple conveyor can switch on vs the box is not in place the apple conveyor will switch off). The Apple Conveyor stop button is a momentary break contact as it disrupts the signal of Apple Conveyor once pressed in and not for a constant fixed term.

 ![image](https://github.com/user-attachments/assets/e685e43e-a0e3-4274-9541-fab27074255a)


Navigating into the 5th rung, it is evident that the Apple Conveyor Latch retains the power for the Apple Conveyor which is currently on in this example. The Part Sensor: SE1 which senses apples falling into the box is a Momentary Make Pushbutton as each falling apple will interact with the sensor for a moment then fall into the box so to simulate this, the assignment operator would press this button to add one apple to the box (In turn incrementing the Apple Counter). The reason why the Apple Counter is used as a reference input in the 1st and other respective rungs rather than the Part Sensor: SE1 is because the counter is on as long as the apple count is not 15 while the Part Sensor only indicates the presence of an apple momentarily. This figure showcases the Apple Counter starting at 0 thus the Blue LED light remains on while the other LED lights are off. 
![image](https://github.com/user-attachments/assets/c6130645-3ff7-4a42-81e5-43209c5472cb)


Now when the Apple Counter reaches 15 apples in the box, this allows for the Box Conveyor to be turned on (something that will be discussed later). In this diagram as indicated by the Output Qs, only Q4 (the orange LED light) is on due to the Apple Counter reaching 15 apples in turn switching off both the Blue LED light and the Apple Conveyor. This apple count can be reset using the Apple Counter Reset button which is a Momentary Make pushbutton due to sending a high input signal to the reset parameter of the Apple Counter.
![image](https://github.com/user-attachments/assets/3f2e31ef-5bb5-4ed4-9348-4e5647846e7d)


Once the Part Sensor:SE1 detects an additional apple after the 15 required are in the box, the Orange LED light turns off and the Red LED light flashes at 1Hz every time an apple is detected. An additional counter has been used to mimic the functionality of the Red and Orange light named Overflow Counter: The counter only turns on at 2 because the initial 1 will be the orange light condition (rung 10) and 2 (when it turns on, rung 11) is the Red light after the Apple Counter reaches 15. The Part Sensor: SE 1 is connected to the Overflow Counter as well as the Red LED light which sends a momentary make signal thus causing the flashing effect @ 1Hz.
![image](https://github.com/user-attachments/assets/8c7b031a-1dbd-46b4-a224-2fb32b5b2ada)
 
The last section of the apple packing plant is the box conveyor system. Following the task sequence of the apple conveyor system:
1.	The Apple Conveyor is off.
2.	The box has reached enough apples (Apple Counter=15).
These two parameters permit the Box Conveyor to turn. The Box Conveyor can only go on if the all the following conditions are false:
1.	The Apple Conveyor is on.
2.	The Apple counter is less than 15.
3.	All the above have been met but 5 seconds have not passed.

Once requirements 1 – 2 are met, the Box Conveyor Start Momentary Make pushbutton can start the process of turning on the Box Conveyor. In the above diagram The Box Conveyor Start button is a Momentary Make as it sends a momentary signal to the Box On-Delay Timer. An on-delay was utilized as the apple packing plant needs to reach all the prerequisites then wait 5 seconds before the Box Conveyor can turn on. The Box Conveyor is connected to a Normally Open reference input for the Box On-Deay Timer, only allowing power when the timer has reached 5 seconds. The Box Conveyor system can be stopped using the Box Conveyor Stop Momentary break contact, which is in this state as it is Normally closed thus it will disrupt the power signal or the Box Sensor is High which is Normally Open contact.

This assignment has illustrated the usage and importance of PLC systems in the industrial and automation industry. Automating process sequences allows for more work safety, less human power and in some cases faster production. The starting off cost can be expensive but for big plants such as the given example, the faster productivity generates more revenue for larger processing plants.

Technologies Used:
Draw.io – Block Diagram
Microsoft Word – Flowchart and Documentation
Siemens Logo Soft Comfort – PLC

References:
Ladder Logic Symbols: A Comprehensive Guide | Cassiano Ferro Moraes | https://www.wevolver.com/article/ladder-logic-symbols-a-comprehensive-guide
Logo Soft Comfort | Siemens | Logo Soft Comfort | Siemens | Logo Soft Comfort | Siemens | https://www.siemens.com/global/en/products/automation/systems/industrial/plc/logo/logo-software.html
What is a programmable logic controller | Tech Target | https://www.techtarget.com/whatis/definition/programmed-logic-controller-PLC
What is a block diagram and how to create one | Miro | https://miro.com/diagramming/what-is-a-block-diagram/
What is a flow chart | Miro | https://miro.com/flowchart/what-is-a-flowchart/

