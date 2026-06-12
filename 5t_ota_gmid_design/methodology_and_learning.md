# Methodology and Learnings – 5T OTA Design Using gm/ID Methodology

## Introduction

This project was not simply about designing a 5-Transistor Operational Transconductance Amplifier (5T OTA). It was an exercise in developing analog design intuition, understanding specification-driven design, learning to make engineering tradeoffs, and using simulation as a verification tool rather than a discovery tool.

The most valuable outcome was not the final gain, bandwidth, or power consumption. The real outcome was understanding how an analog designer thinks.

Throughout the project, every design decision was questioned:

* Why this current?
* Why this gm/ID?
* Why this length?
* Why this bias voltage?
* Why this load capacitance?
* Why this specification?
* Why should a non-ideality be fixed?
* When should it be ignored?

These questions gradually transformed the design process from transistor sizing into engineering reasoning.

---

# Initial Design Philosophy

One statement guided the entire project:

> The best design is the one that meets the specification with the lowest cost in area, power, speed, and complexity.

This statement seems simple but fundamentally changes how analog circuits are designed.

A beginner often tries to maximize:

* Gain
* Bandwidth
* Current source quality
* Output resistance

An analog designer asks:

> Does the specification require this improvement?

If not, additional optimization may simply waste resources.

---

# Why a 5T OTA?

The 5T OTA was selected because it is one of the simplest and most important analog building blocks.

The circuit consists of:

* NMOS differential pair
* PMOS current mirror load
* NMOS tail current source

Despite its simplicity, the circuit introduces several fundamental analog concepts:

* Differential amplification
* Current steering
* Active loads
* Current mirrors
* Bias generation
* Gain-bandwidth tradeoffs
* Saturation constraints
* Headroom management

The 5T OTA serves as the foundation for understanding more advanced circuits such as:

* Two-stage OTAs
* Folded cascodes
* Telescopic OTAs
* Operational amplifiers

---

# Specification Definition

The first major lesson occurred before any transistor was sized.

Initially, a specification was written:

* Gain ≥ 35 dB
* GBW ≥ 5 MHz
* Power ≤ 50 µW

At first glance, this seemed sufficient.

However, several important questions emerged.

---

## What Load Capacitance?

Bandwidth is meaningless without specifying load capacitance.

The same OTA driving:

* 100 fF
* 1 pF
* 10 pF

will exhibit completely different bandwidths.

This led to the realization:

> Bandwidth specifications are incomplete unless the load capacitance is specified.

Therefore:

CL = 1 pF

was added.

---

## What Type of Gain?

Initially:

Gain ≥ 35 dB

was specified.

However, another question emerged:

> Differential gain or single-ended gain?

A differential amplifier can be measured in multiple ways.

Single-ended excitation:

VINP = AC 1
VINM = AC 0

Differential excitation:

VINP = AC +0.5
VINM = AC −0.5

The measured gain depends on the chosen method.

This revealed another important lesson:

> A specification is incomplete unless the measurement methodology is defined.

---

# Understanding Current as a Resource

One of the most important discussions occurred when selecting tail current.

A bandwidth target of:

GBW ≥ 5 MHz

was given.

Calculations showed:

4–5 µA tail current was sufficient.

However, the power budget allowed significantly more current.

The question became:

> Why not increase current to 10 µA?

This produced one of the most important insights of the project.

Increasing current:

Advantages:

* Higher gm
* Higher bandwidth

Disadvantages:

* Higher power
* Potentially lower gain efficiency
* More difficult gain optimization

This led to the realization:

> Analog design is resource allocation.

Current should not be spent merely because it is available.

It should only be spent when the specifications require it.

---

# Choosing gm/ID

The selected operating point was:

gm/ID = 15

This placed the devices in moderate inversion.

The initial reasoning:

* Good gm efficiency
* Low power
* Strong intrinsic gain
* Suitable for analog design

However, another important question emerged:

> Why use gm/ID = 15 for every transistor?

This led to understanding that different devices perform different functions.

Input Pair:

* Generates transconductance

PMOS Load:

* Provides output resistance

Tail Device:

* Behaves as a current source

The realization was:

> Different devices may deserve different operating points.

A designer should think about transistor function rather than blindly applying identical operating conditions.

---

# LUT Data Used

## NMOS (gm/ID = 15, L = 0.5 µm)

* VGS = 749.7 mV
* VDSAT = 108 mV
* gm/gds = 153.1
* ID/W = 1.337 µA/µm

## PMOS (gm/ID = 15, L = 0.5 µm)

* VSG = 847.8 mV
* VDSAT = 107 mV
* gm/gds = 238.8
* ID/W = 381 nA/µm

These values became the foundation of the entire design.

---

# Width Calculation

Tail current target:

Itail = 5 µA

Branch current:

ID = 2.5 µA

Calculated widths:

Input NMOS:
≈ 2 µm

PMOS Load:
≈ 6.5 µm

Tail NMOS:
≈ 4 µm

Reference NMOS:
≈ 4 µm

---

# Prediction Before Simulation

A major mindset shift occurred here.

The goal was not:

> Run simulation and discover the answer.

Instead:

> Predict the answer and use simulation to verify it.

Predictions:

vbias ≈ 0.75 V

vsource ≈ 0.15 V

vout ≈ 1.0–1.2 V

Gain ≈ 39 dB

GBW ≈ 5–6 MHz

Tail current ≈ 5 µA

Only after these predictions were made was simulation performed.

This was one of the most important learning experiences of the project.

---

# Bias Generation

Initially, a simple DC voltage source could have been used to generate the tail bias voltage.

However, a more realistic approach was chosen:

Current mirror bias generation.

This introduced a powerful concept:

> Current can generate voltage.

A diode-connected transistor carrying a known current automatically generates the required VGS.

That VGS can then be copied to another transistor.

This is one of the fundamental principles behind analog biasing circuits.

---

# Why Was the Tail Device Not Made Longer?

A common analog design rule states:

> Current source devices should use longer channel lengths.

The immediate question was:

> Why?

If all specifications are already satisfied using minimum length devices, increasing length introduces:

* Area penalty
* Additional capacitance

without necessarily improving any required specification.

This led to another important lesson:

> Design decisions should be justified by specifications, not habits.

---

# Current Mirror Observation

One of the most valuable observations occurred during operating-point verification.

Measured:

XM6 ≈ 5.02 µA

XM5 ≈ 4.53 µA

At first this seemed surprising because:

* Same W/L
* Same gate voltage
* Same gm/ID target

Yet currents differed.

Investigation revealed:

XM6:
VDS ≈ 746 mV

XM5:
VDS ≈ 151 mV

This produced one of the most important practical lessons of the project:

> Identical W/L and identical VGS do not guarantee identical current.

For ideal mirroring:

* Same W/L
* Same VGS
* Same VDS

must all be satisfied.

Because XM6 and XM5 experienced different drain voltages, channel-length modulation introduced current mirror error.

This was one of the first practical encounters with non-ideal analog behavior.

---

# Another Important Lesson

The immediate reaction was:

> Should we fix the mirror error?

This produced another major insight.

There was no specification requiring:

* Mirror error < 1%
* Current accuracy < 5%

Therefore:

> Not every non-ideality requires optimization.

The proper engineering question became:

> Does this non-ideality violate any specification?

The answer was no.

Therefore the design was left unchanged.

---

# Saturation Verification

Every transistor was individually checked.

A particularly interesting observation was XM5.

XM5:

VDS ≈ 151 mV

VDSAT ≈ 107 mV

Margin:

≈ 44 mV

XM5 was saturated, but not deeply saturated.

This highlighted the importance of headroom management.

The tail device was identified as the transistor closest to leaving saturation.

---

# Gain Verification

Predicted:

≈ 39 dB

Measured:

≈ 39 dB

This close agreement demonstrated the effectiveness of LUT-based design and hand calculations.

---

# Bandwidth Verification

Measured:

−3 dB Bandwidth ≈ 58.8 kHz

Using:

GBW ≈ Gain × BW

Gain:

≈ 89 V/V

Result:

GBW ≈ 5.2 MHz

Requirement:

≥ 5 MHz

PASS

This validated the earlier decision not to waste additional current.

---

# Why Phase Margin Was Not Included

Another interesting question emerged:

> Why is phase margin not specified?

The answer:

This OTA was being evaluated open-loop.

Phase margin becomes meaningful when feedback is introduced.

Examples:

* Two-stage OTA
* Unity-gain buffer
* Operational amplifier

Therefore phase margin was intentionally excluded from the first OTA project.

---

# Major Mindset Shifts

## Before

Run simulation and wait for results.

## After

Predict results and verify them.

---

## Before

Increase performance whenever possible.

## After

Increase performance only when specifications require it.

---

## Before

Think in voltages.

## After

Think in currents.

---

## Before

Try to eliminate every non-ideality.

## After

Determine whether the non-ideality matters.

---

## Before

Follow design rules blindly.

## After

Question every design decision.

---

# Final Results

Technology:
GF180MCU

Supply Voltage:
1.8 V

DC Gain:
≈ 39 dB

GBW:
≈ 5.2 MHz

Power:
≈ 8.15 µW

Tail Current:
≈ 4.53 µA

VOCM:
≈ 1.095 V

Load Capacitance:
1 pF

All Devices Saturated:
YES

Specifications Met:
YES

---

# Final Reflection

The most important outcome of this project was not the OTA itself.

It was the gradual transition from thinking like someone who sizes transistors to thinking like someone who designs circuits.

The project demonstrated that analog design is fundamentally an exercise in understanding tradeoffs, allocating resources intelligently, predicting behavior before simulation, and making decisions based on specifications rather than assumptions.
