# WSS-MR
 Code repo for WSS-MR LabVIEW code
 
 ## This repository serves as an archive for the code that I developed in LabVIEW for WorldSkills Singapore Mobile Robotics 2020
I will update this code over the course of the competition and publish it once the competition is over. 

# Post-competition reflection
Competing teams were very strong in both hardware and software department. They performed excellently in both core tasks and main tasks, which put them ahead significantly. Though both NP teams did not perform as well, I would have to say that Kai Ming, Nicholas and Brian all tried their best to execute the tasks, despite being put under a lot of pressure before and during the competition. 

## Software 
For the software side of things, I felt that TLB was a great help to improve workflow. To be honest, a lot of my algorithms were generated on the actual day itself, and the modular approach of TLB allowed me to simply chain the appropriate action cases together. However, there seems to be a bug whereby TLB would constantly enqueue elements repeatedly until myRIO ran out of memory and causing the entire deployment to fail. Moreover, I was still unable to get proper error handling/correction whenever i2c failed (elaborated more in the next section). 

### i2C
However a pain point during the last 1.5 years of development was perhaps the i2C interface. An examination of studica's i2c interface with the NAVX compass code revealed that it would render one entire MXP board useless despite i2c being capable of supporting up to 128 devices. It was also the source of many software crashes and errors during development, which forced us to troubleshoot, spending valuable time fixing an issue that shouldn't have been a problem. Moreover, poor error handling of device disconnects meant that a restart of the entire program was needed, and as such wasted a lot of time rebooting. 

### Ultrasonic 
The included code for the Parallax Ping Ultrasonic code had inherent flaws whereby there was **no delay** between the ping and waiting for the pulse in. This resulted in unreliable ultrasonic sensor readings at times, and caused serious issues in cases whereby we needed the ultrasonic sensor to be reliable. Alternatives were not an option as a manual generation of ultrasonic sensor code could not work when using the Studica FPGA.

### Limited I/O
Using the custom FPGA profile severely limited our I/O. Output pins were only availabe to Port C (8 outputs). Moreover, UART was disabled, meaning that some auxillary boards could not be used. 

### Webcam 
There seems to be a bug either in the LifeCam driver or NI Max for the exposure settings. This has led to a lot of frustration as we had to find out the right values to input or else the camera would just be a white screen. I later discovered that certain multiples of 5, shifted by a few decimal places were the correct values to be used. Though minor, using the LifeCam (which in my opinion was actually decent after discovering this bug) was a major painpoint during development. 

## Algorithms
To be honest, both teams were not prepared for how complex Task 9 would be, as we had assumed that the tables would be flushed against the wall and had marking lines. We were not expecting entirely random placements of the tables. This also meant that much of our time trying to fine tune an algorithm for task 9 was wasted. Our main tasks were actually coded 2 days prior to the competition, so it was to be expected that we could only accomplish part of the main task. 

## Hardware
For the hardware side of things, to be blunt, we were severely lacking in that regard. It was observed that NYP had fully pre-fabricated and pre-assembled parts. For our teams, we relied heavily on 3D printed parts, while versatile, took a great deal of time to print and would break very often. Both Brian and Nicholas were also relatively new to CAD and were thrown into fabricating and designing of parts without prior knowledge of anything mechanical. While the best effort was made to teach us the basics of CAD, our eventual needs of 3D printing were much more complex and needed a lot of R&D. It is observed in Brian and Nicholas painstakingly spending their hours researching, desiging and printing only to find that there were flaws in the design and had to repeat the process. 

### Robot base
Both robot bases while being vastly different in overall footprint, were similar in the regard that they were 3 wheel omni driven. This meant that robot movement was very versatile and had a wide range of motion. Nothing much to be commented here as many other teams also employed a 3 wheel omni drive. 

### Robot superstructure
The superstructure refers to the vertical protrusion from the robot base to give it height needed for the core and main tasks. For Brian and I, our superstructure was fairly simple as it used Tetrix C-channels. This meant that erecting one could be done in a matter of minutes assuming the base was ready. For Kai Ming and Nicholas, their superstructure was partially integrated into the robot base, which allowed them to assemble the robot quickly as well. However, it was noted that both NYP and TP had superstructures that were vastly taller, but had simpler designs. NYP in particular, had their superstrcuture partially fabricated and thus could assemble their robot very quickly. while height isn't a deciding factor of the functionality of the robot, it provides more flexibility in the design and a higher range of movement. 

### Picking mechanism/arm
The largest bottleneck in this entire project. We spent almost a full year developing a solution that in the end still had issues with picking up items. Kai Ming and Nicholas originally intended to have a 3 axis robot arm using stepper motors, which was then discontinued seeing that it did not meet the height requirements for depositing. They then transitioned to a simple Z-Axis picking mechanism that used stepper motors and a vacuum. However, Kai Ming then discovered an issue with using myRIO for the generation of pulses to the stepper driver and had to resort to using Arduino to do so. Their design would have been excellent, but due to varying lighting conditions, calibrations were off slightly and they could not pick up items reliably. 

Brian and I stuck with the same Z-Axis and Y-Axis mechanism almost for the entirety of the project. However, I spent a great deal of time developing code for the stepper motors and deploying it to Arduino. This also meant that a lot of time was spent trying to interface myRIO and arduino, which was supposedly easy, but with the implementaion of Studica's FPGA, I had a lot of debugging to do. Brian's claw design, while simple, sought to be compatible with all items. However, the inherent flaw of using a claw was that it could not pick items along the edges, which we were presented during the competition, and thus resulting in our failure of picking up an item. Brian was contemplating of adding claw rotation to make it more versatile, but COVID and school examinations (and our internship) halted our progress on R&D and we were forced to stick with what we had. 

Our use of stepper motors while accurate, proved to be siginificantly slower than continuous servos used by NYP or high current servos used by TP. The one thing that intrigued me was NYP's usage of continuous servos. They were able to accurately move to heights despite high accelerations and servo rotation speeds, which has led me to think that some sort of stopping mechanism was used.

### Storage
Both NYP and TP had some sort of object storage. While we were considering it, the implementation of storage on our current robot design did not seem feasible. As such, we put it off. 

## Project timeline and structure
As we were doing it alongside our studies, we could only work on the project when we had the time on empty days or the holidays. We also did not have a strict project timeline to follow and thus our progress could be observed to be slow. Phase 1 of COVID also severely impacted our momentum, and both teams had lost both momentum and drive to continue the project, though it was somewhat renwed towards the competition days. 

In addition, our testing arena was only made available in January 2020, 2 months prior to the original competition date. It would have been better if we got it earlier to test our other core tasks and algorithms, as prior to that both Kai Ming and I were testing the viability of our algorithms on a purely hypothetical basis, and did not take into account physical disturbences. One recommendation I have for the next batch would be that the current arena we have should be used as basic testing grounds. 

Both teams also had different agendas. While the same programming framework (TLB) was used, hardware and programming paradigms were vastly different. It was observed that both TP and NYP had similar designs, and perhaps that was what we could have done for the initial designing phase of the robot. 

## Final comments and conclusions
While we did not win anything, I think both teams are glad that we tried our best and learned a great deal along the way. It certainly would have been great to see success, but ultimately it was the process that was the most meaningful. I am greatful to both teams for putting all their effort and sacrificing their personal time to work on this project, and I hope that with our experience the next and subsequent WSS-MR generations could learn and improve and potentially succeed.
