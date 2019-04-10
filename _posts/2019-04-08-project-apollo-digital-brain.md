---
layout: post
title:  "Project Apollo's Digital Brain"
date:   2019-04-08 9:32:01
---

The word software wasn't used in NASA's contract for the Apollo Guidance Computer (AGC), and the word programming only appears twice in the original statement of work. Software wasn't in the Apollo schedule or budget. Yet software played an essential role in the Apollo Project; binding the disparate systems of the Lunar Module, Command Module, and Saturn V booster together.

In this post, we'll examine the software that ran on the AGC, and even test it in an emulator. Our goal is to better understand what caused the 1201 and 1202 alarms during the Apollo 11 landing by reproducing the environment in which they occurred. But first, you'll need a basic overview of the system and the best place to start is where it all began: Cambridge, MA.

## Software at the Center of Apollo

Unlike other parts of the Apollo program, the contract for the AGC wasn't awarded to a company. Instead, it went to a university: the Massachusetts Institute of Technology (MIT) in Cambridge, MA. More specifically, the contract was given to MIT's Instrumentation Laboratory (IL), which was known for developing aeronautic instruments during World War II and using gyroscopes in navigation systems.

Underpinning these instruments was the team that created Whirlwind, one of the first-ever digital computers. Whirlwind was designed as a flight simulator but it eventually pioneered programming concepts like parallel processing, real-time processing, and compilers. According to Don Eyles, it was the direct predecessor of the AGC. Despite the remarkable innovations of Whirlwind, computer software wasn't given a lot of respect at the time. When Dick Battin, who led the design of the AGC, told his wife that he would be in charge of Apollo's software, she joked "please don't tell our friends." But the work his team did between 1961 and 1969 would change that perception.

The IL group was tasked with designing a general-purpose digital computer small enough to fit inside a spacecraft. At the time, the only thing a computer had ever fit in was a room. The contract also required a console for the astronauts to interact with and a number of guidance instruments, including a space sextant and a gyroscope. Tying these components together was the guidance software, which took measurements from the instruments and performed calculations that were used to steer the spacecraft.

The computer itself, however, was nothing more than a dumb machine. The brain of the ship was in the software, which was capable of turning raw sensor data into meaningful instructions that throttled the engines and controlled the craft. Its importance was never more evident than during the Apollo 11 lunar landing, when the computer's operating system continued to perform critical guidance calculations in the face of numerous alarms. In the previous post in this series, we retraced those events. Now let's get to the bottom of what caused them.

## We ready to go P63?

In the official Apollo 11 mission transcripts, Buzz Aldrin asks Neil Armstrong "We ready to go P63?" shortly before they begin their descent to the lunar surface. Aldrin is referring to the command he must enter into the computer to start the Powered Descent Ignition (PDI) phase of the flight.

P63 was one of many programs that ran on the AGC. Aldrin would enter a series of commands into the computer at predetermined points in the mission to start each one. We can replicate Aldrin's actions in an AGC simulator. We'll start with a simple program, called Moonjs, that runs in a web browser. Open the Moonjs website in a new browser window and you'll see an interface that looks like this:

IMAGE

The digital keypad mimics the DSKY (Display-Keyboard) that the Apollo astronauts used to interact with the AGC. This is the same interface that went blank during the critical moments of the lunar landing.

The astronauts entered commands into the DSKY with a verb-noun syntax. An astronaut would first press the VERB button, followed by a one or two-digit code for a command. Some commands worked on a specific target, which the astronaut would provide by pressing the NOUN button followed by a code that represented the thing the verb would be performed on. Finally, the astronaut would press the ENTR button to initiate the command.

Let's try it now with a simple example that doesn't require a noun: VERB 35. This command was used to test the DSKY by illuminating every light on the display to ensure they're working. To be very explicit, you should enter the command with the following actions:

* Press the VERB key
* Press the 3 key
* Press the 5 key
* Press the ENTR key

As a shorthand, we'll represent these steps as the string V35E. Each letter corresponds to a button press, using "V" for VERB and "E" for ENTR. We'll also use "N" for the NOUN button in a moment. After running V35E, you'll see every light on the DSKY illuminated and some of them blinking.

IMAGE

Now let's try a command that uses a noun. Enter the following sequence into the DSKY to display the time since the AGC started:

V16N36E

Verb 16 monitors the values contained in a register or set of registers, and periodically displays their values on the DSKY. Noun 36 represents the two registers that contain the AGC's master clock. The first register is incremented every 10 milliseconds, and overflows into a second register every 168 seconds.

IMAGE

This is similar to the command Buzz Aldrin used during the Apollo 11 landing to monitor Delta-H, which was V16N68E. Again, V16 was used to monitor some values in memory, but in this case N68 defined the location of the Delta-H value along with some other data that was important to the landing.

Moonjs also includes a simulated launch sequence. The AGC was passive during launch (it didn't control the Saturn V rocket engines), but by following this sequence you'll get a good feeling for how the astronauts used the computer. Perform the following steps in the browser window running Moonjs:

1. Press the Enable IMU button. This will initialize the Inertial Measurement Unit (IMU) simulator, which tells the AGC the direction the craft is pointing. While it's initializing, the NO ATT lamp will illuminate on the DSKY. This indicates that the computer does not yet know it's attitude.
2. Wait about 90 seconds until the NO ATT lamp turns off.
3. Enter V37E01E to initiate major mode P01 (the Prelaunch Initialization program). The digits under the PROG lamp will read "01" to indicate that this program is running.
4. After about 10 seconds, the PITCH value will approach 90 degrees, which indicates that the IMU is fully calibrated. When this happens, major mode P02 (the Prelaunch Gyro-compassing program) will automatically start and the digits under the PROG lamp will show "02".
5. Press the Launch button.

When the computer detects the launch event, it will automatically enter major mode P11 (the Earth Orbit Insertion monitoring program). In this mode, the DSKY will update every 2-seconds with values for velocity and attitude.

IMAGE

When you enter a command like V37E01E into the DSKY, the first part (V37E) instructs the computer to change the major mode (essentially the main program) it's running. This command has no noun; instead, the digits after V37 define the program it will run. In this case, 01 is the prelaunch initialization program (called P01).

It would be nice to enter V37E63E as Aldrin did during the lunar descent, but the Moonjs simulator can't run this program. That's because it's simulating only the software that ran on the Command and Service Module (CSM). There were actually two identical computers on the Apollo spacecraft: one for the CSM and another for the Lunar Module (LM). The DSKY interface and even the internal circuits for each computer were the same, but the software programs they ran were different (on Apollo 13, the LM AGC was used to pilot both crafts, but that's another story).

The software on the CSM was called Colossus. It was passive during launch and then guided the craft from trans-lunar injection (a maneuver that takes the craft out of Earth's orbit), to lunar orbit insertion (which takes place behind the Moon; out of communication with Earth), and finally out of lunar orbit as it returns to Earth.

The software on the LM, called Luminary, guided the craft through the most complex and dangerous phase of the Apollo mission: the lunar landing. After the LM separated from the CSM at about 50,000 feet above the lunar surface, the computer fired the LM's engine to slow its descent and gradually lower it to the Moon before running out of fuel.

Luminary will be the focus of our investigation, because this is the software that generated the 1201 and 1202 alarm codes. In order to examine this software in detail, we'll need a more advanced AGC emulator that runs in a virtual machine instead of a browser. This will allow us to inspect and debug the actual source code written by the software engineers at MIT.

The rest of this post gets a little difficult, but it's optional. If you're not interested in learning how to debug the emulator, you can skip to the next installment in this series. For those who are up to the challenge, we'll take a deep dive into the AGC - not because it is easy, but because it is hard.

---

The original code for the AGC was stored on physical paper, which was meticulously encoded into read-only memory by weaving wires through small holes to represent physical ones and zeros. Fortunately, a dedicated group of hobbyists have scanned the source documents and transferred the code to modern digital files. You can even find the code in several repositories on Github. The repository we'll use is maintained by the Virtual AGC project. You can view the code, with syntax coloring, in the virtualgc Github repository.

The Virtual AGC project hosts both the original source code for the AGC (including the different versions of it that ran on various Apollo missions) as well as an emulator that mimics the AGC hardware and runs the compiled code. For the rest of the series, the term "Virtual AGC" will refer exclusively to the emulator.

It is possible to build and run the Virtual AGC natively on either Windows or Mac, but it's much easier to run a precompiled emulator inside a virtual machine. Thus, your first task is to download and install VirtualBox, a free and open source virtualization app. You can follow the instructions on the VirtualBox website.

Next, download the Virtual AGC machine image from the Virtual AGC website. The file is very large (about 1.2 GB), so the download may take a while. When the it's complete, run the Virtual AGC image by opening it in VirtualBox and clicking the start button.

When the virtual machine starts-up, you'll see a window like this:

IMAGE

The VirtualAGC Simulator Manager allows you to choose the version of the AGC software you want to run (including the code from several different Apollo missions), and you can select either the Lunar Module or Command Module for that mission.

Select "Apollo 11 Lunar Module rev 1" from the "AGC Simulation Type" dropdown list. Then select "Telemetry Downlink Monitor" from the "Interfaces" options. Make sure the DSKY is "Full" and the Downlink is "Normal" in the "Interfaces Style" options. Select "Debugger" for the "AGC code", and finally, click the button labeled "Run!".

When the simulation starts up, several windows will open including a "Simulation Status" window, "yTelemetry" window, "yDSKY" window, and "yAGC" terminal session, which you'll use for debugging the AGC. The DSKY looks like the one displayed on the Moonjs website.

IMAGE

For now, hide or minimize the "Status" and "yTelemetry" windows. Leave the DSKY window visible in the background and select the "yAGC" terminal. The prompt looks like this:

```
Apollo Guidance Computer simulation, ver. 2017-04-17, built Apr 17 2017 13:20:46
Copyright (C) 2003-2009 Ronald S. Burkey, Onno Hommes.
yaAGC is free software, covered by the GNU General Public License, and you are
welcome to change it and/or distribute copies of it under certain conditions.
Refer to http://www.ibiblio.org/apollo/index.html for additional information.
(agc)
```

Because we're running in debug mode, the AGC hasn't completely booted up yet. It starts out paused, which makes it possible to debug the boot sequence. We want to let it run though, so type `cont` and press return.

```
(agc) cont
[New Thread 11]
[Switching to Thread 11]
```

In the background, you'll notice some lights blinking on the DSKY - the system has finished its boot sequence. Let's test it out with a simple command: key-in V16N36E like you did on Moonjs.

IMAGE

Now you've learned how to use the AGC by entering the same commands used by the Apollo astronauts. But if we're going to get to the bottom of the 1201 and 1202 program alarms, you'll need to understand how the computer works from the perspective of its creators. For that, you'll learn how to use the Virtual AGC debugger.

## Debugging an AGC Interrupt

A debugger is a tool used to inspect the execution of a program. It can pause a program, inspect variables, and step through each instruction of your command (instead of letting them run naturally).

When you first started the VirutalAGC emulator, you minimized a terminal window called yaAGC. Open this window now, and press Ctrl+C. The `(agc)` prompt that you saw earlier on will appear again.

```
^CBANKJUMP () at INTER-BANK_COMMUNICATION.agc:72
72		MASK	LOW10
(agc)
```

The Ctrl+C command paused the emulator at an arbitrary point in the code (whatever it was executing at the moment you pressed those keys). In this case it's line 72 of the `INTER-BANK_COMMUNICATION.agc` file. If you try to use the DSKY now, you'll find that nothing happens.

From the yaAGC terminal you can enter the `help all` command to see a list of available commands. One of those is `list`, which prints the code surrounding the point where the program is paused. You can also use `list` to print specific sections of code by passing it an argument. For example, run the following command to see the code on line 47 in the file `THE_LUNAR_LANDING.agc`:

```
(agc) list THE_LUNAR_LANDING.agc:47
```

This line contains a label, `P63LM`, which defines the subroutine that begins the P63 program described in the previous installment of this series. We can also use the `list` command to print these named subroutines. Run the following command to view the `KEYRUPT1` subroutine:

```
(agc) list KEYRUPT1
KEYRUPT,_UPRUPT.agc:
33			BANK	14
34			SETLOC	KEYRUPT
35			BANK
36			COUNT*	$$/KEYUP
37
38	KEYRUPT1	TS	BANKRUPT
39			XCH	Q
40			TS	QRUPT
41			TC	LODSAMPT	# TIME IS SNATCHED IN RUPT FOR NOUN 65.
42	CAF	LOW5
```

This subroutine is used to handle a key press event. When you press a key on the DSKY, such as VERB or 3, it creates an event called an _interrupt_. The interrupt tells the software to stop what it is doing and process the event. We'll discuss interrupts in more detail in the next part of this series.

Let's watch the interrupt in action by setting a breakpoint on the `KEYRUPT1` subroutine. A breakpoint tells the debugger to pause the program when it reaches this line of code. Run the following command:

```
(agc) break KEYRUPT1
Breakpoint 1 at 0x22bc: file KEYRUPT,_UPRUPT.agc, line 38.
```

This created a breakpoint with identifier 1 for the given subroutine. Now allow the program to continue running so we can try it out:

```
(agc) cont
```

Press the VERB key on the DSKY, and watch the yaAGC terminal. You'll notice that it stops the program's execution and returns to the `(agc)` prompt.

```
Breakpoint 1 at KEYRUPT1 () at KEYRUPT,_UPRUPT.agc:38
KEYRUPT1 () at KEYRUPT,_UPRUPT.agc:38
38	KEYRUPT1	TS	BANKRUPT
(agc)
```

The debugger caught your breakpoint. From here you can inspect variables, and step through the code as it executes. Use the `list` command to see what's ahead:

```
(agc) list
39			XCH	Q
40			TS	QRUPT
41			TC	LODSAMPT	# TIME IS SNATCHED IN RUPT FOR NOUN 65.
42			CAF	LOW5
43			EXTEND
44			RAND	MNKEYIN		# CHECK IF KEYS 5M-1M ON
45	KEYCOM		TS	RUPTREG4
46			CS	FLAGWRD5
47			MASK	DSKYFBIT
48			ADS	FLAGWRD5
```

We see that the code is going to execute the `XCH` (or _exchange_) command with the `Q` address. Let's inspect the value of this address:

```
(agc) print Q
$1 = 0
```

The `XCH` command exchanges the value of `Q` with the value at address `A`. Why it does this will be discussed later in this series. The important lesson here is that you know how to watch this kind of execution. Let's inspect the value of the `A` address too:

```
(agc) print A
$1 = 02002
```

Now use the `step` command twice to step forward in the program. This allows the next two instructions to execute:

```
(agc) step
39			XCH	Q
(agc) step
40			TS	QRUPT
```

And check the value of `Q` and `A` again:

```
(agc) print A
$1 = 0
(agc) print Q
$1 = 02002
```

The value of the `A` memory address and the `Q` memory address have been swapped. Before we move on, delete the breakpoint by running this command:

```
(agc) delete 1
```

Now that you understand how to watch the internal execution of the AGC, let's walk through the commands Buzz Aldrin keyed-in during the final minutes of the Apollo 11 landing.

First, press the Reset button (RSET) to clear out any errors that might have been generated during your earlier debugging. Then key-in the same command that Aldrin entered to start P63:

V37E63E

Immediately after pressing ENTR the second time, you'll see the PROG light illuminate. This light indicates that there is a Program Alarm - it's the same indicator that notified Buzz Aldrin of an alarm during his landing. When Aldrin saw the light, he entered a command to display the alarm code on the DSKY. Enter that same command now:

V5N9E

The DSKY displays the value 210, which is a different alarm code than the 1202 and 1201 that Aldrin saw. This alarm is a bit more severe as it indicates that the IMU is not responding. That makes sense, of course, since our emulator does not have an IMU.

Ideally, we would continue our investigation by providing dummy values for the IMU like the Moonjs simulator provided during the  launch sequence. In this way, we could recreate the environment that triggered the 1202 alarm and use the emulator's debugger to track down the root cause. Unfortunately, this is difficult to do - partially because the circumstances were complex but also because the Virtual AGC emulator is not a complete representation of the entire Apollo craft. For example, it does not have physical radars with actual variations in electric currents running through them.

This was a real world dilemma too. The simulators that Neil Armstrong and Buzz Aldrin used for practice had the same deficiency as our emulator, which is why a real 1202 alarm was never encountered during training (an artificial 1202 alarm was tested during a Mission Control dry-run, but it was not trigger by real world conditions). It was also difficult for the programmers who wrote the AGC software - and they were only given a few hours to figure out what had gone wrong.

As soon as the Eagle landed on Tranquility Base, NASA called the team at MIT demanding an explanation and a fix before the LM was scheduled to lift off the lunary surface a few hours later. Frantically, the engineers used their simulators to try and recreate the problem. "We worked all night and time was running short," Fred Martin recalls.

But they were not able to reproduce the alarms. Instead they had to rely on telemetry data from the landing and work their way backwards through the program, which is exactly how we'll proceed.

## All Systems Go

Originally an afterthought, the Apollo software became central to NASA's mission. It mediated the astronauts' interactions with the machine, incorporated data from a variety of sensors, tied together a diverse system of components built by different contractors, and unified teams of people working on subsets of the problem into a cohesive mission. At no time was this more evident than during the lunar landing of Apollo 11.

In the next part of this series, we'll use what you've learned as we get to the bottom of the 1202 and 1201 alarm codes. To do so, we'll need to inspect the AGC source, which will teach you not only how the AGC worked, but also how modern operating systems like Linux work.
