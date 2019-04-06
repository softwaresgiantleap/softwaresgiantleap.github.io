---
layout: post
title:  "The Software That Saved Apollo 11"
date:   2019-04-08 9:32:00
---

While Neil Armstrong was taking his first small steps onto the Moon, a group of software engineers in Cambridge, MA were frantically debugging two alarms triggered during the Apollo 11 landing sequence. NASA wanted to know what had gone wrong and needed an answer before the Lunar Module (LM) was scheduled to lift off the Moon's surface only a few hours later.

In 1969, this was an entirely new kind of problem - not only because it was happening on the Moon but also because the concept of software was new. The term software engineering was coined during the Apollo program, and suddenly there were self-ascribed software engineers trying to solve one of the most pressing technical problems in the world.
The software developers of the Apollo Project were pioneers. They wrote revolutionary programs using techniques that have become common place in modern computers. As a result, the software that ran the Apollo Guidance Computer (AGC) represents a microcosm of today's computer fundamentals. By learning how it was written and how it worked, you'll develop a better understanding of the device on which you're reading these words.

Before we dive into the technical details, let's take a closer look at the situation those early software engineers faced on the night of July 20, 1969.

## More Than Just Mankind

The story of the Apollo 11 landing has been told many times, but almost always with the astronauts as the main characters. What follows is a retelling of this famous story with a focus on the computer.

We'll begin shortly after 5:00 PM Houston, TX time on July 20, 1969. Neil Armstrong and Buzz Aldrin have already climbed into the Lunar Module (LM), undocked from the Command Module (CM), and turned on the craft's guidance computer: a 70-pound machine that contains more integrated circuits than anything like it on Earth.

The astronauts calibrate the computer by entering a few commands using the digital keypad, and start their journey to the lunar surface.

## 5:05 PM

The descent begins when Buzz Aldrin selects program P63 on the Apollo Guidance Computer to start the craft's Powered Descent Ignition (PDI) sequence. In a few minutes, the computer will run a subroutine called BURNBABY, which ignites the descent engine, controls it's thrust using the computer's orbital calculations, and guides the LM towards the Moon's surface.

Aldrin's job from this point on is to watch the computer and ensure the data it reports agrees with other sources of data. Armstrong is looking out the window of the LM, trying to spot features of the lunar surface that he's meticulously memorized.

A few minutes after PDI starts, Armstrong spots a crater called Maskelyne W., but it comes into his field of view 3 seconds earlier than excepted. This is the first indication that the landing will be something other than perfect.

After Armstrong reports his observation to Mission Control, Gene Kranz checks with his ground team: "Go to continue powered descent?"

"FIDO: Go"

"Guidance: Go"

"EECOM: Go"

"Surgeon: Go"

Kranz gives a quick "Roger" to Charlie Duke, the capsule communicator (CAPCOM). Duke, the only person on the ground who's allowed to communicate with the astronauts, relays the message to the crew: "You are Go to continue powered descent."

## 5:09 PM

The LM completes a yaw maneuver, which rotates the craft such that Armstrong's window is now pointing away from the Moon. The crew is flying blind for the moment, but the computer isn't. This move points the landing radar toward the Moon so it can measure the craft's distance to the lunar surface. Aldrin will compare this value to a value the computer has calculated using only orbital mechanics and inertial sensors. The result is called Delta-H.

"Delta-H is minus 2,900 feet" Aldrin reports. Anything within 3,000 feet is considered good, which means the computer has done a fine job. Now the computer can include the radar's data when doing it's calculations - making them more accurate.

But at just that moment, trouble begins.

## 5:10 PM

"Program alarm", reports Armstrong.

An alarm lights-up on the computer's display. The crew have no idea what it means and Mission Control goes silent for 15 seconds while they try to make sense of it.
In Cambridge, the software team is watching. Someone says "executive overflow - no core sets." The computer is overloaded, but no one knows why. "Something is stealing time" another person says.

In Houston, the ground team is trying to decide if they should abort the mission or continue with the landing. Fortunately, a 26-year-old guidance officer, Steve Bales, recently reviewed this exact alarm code and quickly gives a nervous "Go" for landing.

Armstrong, becoming impatient, demands "Give us a reading on the 1202 program alarm."

"We're Go on that alarm" Charlie Duke replies.

Despite the alarms, the computer continues to run its calculations and Delta-H decreases - it's doing its job. Inside the computer, an incredible technological feat allows it to continue processing despite the errors. This mechanism will be the subject of discussion in the next part of this series.

A few seconds later, the engine throttles down as expected, eliciting relief from Aldrin and Armstrong.

"Wow! Throttle down," says Aldrin.

"Throttle down on time!" says Armstrong.

These are the only exclamation points used in the official mission transcript.

In Cambridge, there is a great sense of relief too. The throttle down event means the navigation and guidance systems are working as they should.
A short while later, the computer automatically switches to program P64, which uses the same set of guidance equations but with different target conditions that are better suited to the lower altitude. The LM is approaching 3000 feet now, but there's more trouble.

## 5:14 PM

"Program alarm", reports Aldrin. This time it's a 1201.

After several minutes without an alarm, the 1201 jolts the team in Cambridge. Their heads are spinning, but Mission Control in Houston is confident and gives the crew a "Go" for landing.

The alarms keep coming - three within a minute - both 1201 and 1202. Houston gives a "Go" after each one. They can see the craft's telemetry and they know the computer is still making the correct calculations. Furthermore, an abort sequence would be especially dangerous at this point in the landing. It's best for them to keep going as long as the data looks good.

##5:15 PM

At just under 2000 feet, the computer's display goes blank. There's no record of this on the mission transcripts, and it wasn't discussed until after the mission. But it undoubtedly adds to the tension as Armstrong realizes the target landing site is strewn with boulders.

When the display does finally come back on, it shows another 1202 alarm. But all the while, it continues to perform the most critical guidance calculations.
Seventeen seconds later, around 800 feet, it happens again. The display goes blank for nearly two seconds. Armstrong's heart rate rises to 150 beats per minute.

Around this time, Armstrong decides that the landing site isn't safe and switches the computer into a semi-manual mode called program P66. In this mode, Armstrong controls the direction of the craft using a joystick instead of the computer's guidance equations. But each of his movements are checked by the computer to ensure he doesn't over-correct. Together, Armstrong and the computer move the LM away from the boulder field.

## 5:16 PM

The rest of the story is pure heroics. Armstrong pilots the LM passed the boulder field, and sets the craft down with only a few seconds of fuel remaining.

"You got a bunch of guys about to turn blue. We're breathing again", CAPCOM Charlie Duke told the crew.

But there's no time to relax for the software engineers. The 1202 and 1201 alarms were unsettling, and the team doubted the computer's health. Was it safe to rely on it for the ascent phase? No one could celebrate until the astronauts were back and in the ocean.

The initial reaction from most of the NASA staff was that the computer had malfunctioned in some way. But the in next part in this series will examine how the computer was actually the hero of the story. It saved the mission and the lives of the astronauts by continuing to process critical guidance equations in the face of these alarms.

##It's a Feature Not a Bug

The alarm Armstrong saw wasn't a bug. It was a warning that the operating system was discarding a flood of less important radar events to ensure it could process the higher priority landing events in real-time. Today we call this priority scheduling and it's used in nearly every operating system - from the Windows running on your laptop, to the iOS running on your Phone, to the Linux kernel running this website. But in 1969 this algorithm was the vanguard of computer science.

The AGC pioneered many new software concepts like priority scheduling, and it's designers were the first to use the phrase "software engineering" to describe their work. In this series, we'll examine each of the AGC's software components and you'll learn not only how they work, but also their significance in advancing software development as a whole.

In the next part of this series, XXX, you'll learn about what was happening inside the computer during these final moments of the Apollo 11 lunar landing. Then in XXX you'll find out how to the software team decided the computer was safe for a return to Earth.
