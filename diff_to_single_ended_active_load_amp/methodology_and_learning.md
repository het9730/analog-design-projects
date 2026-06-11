# Design of a Differential-to-Single-Ended Active-Load Differential Amplifier in GF180MCU Using the gm/ID Methodology

## Introduction

After designing a Common Source Amplifier, a Current Mirror, and a Basic NMOS Differential Pair, the next natural step was to design a Differential Pair with a PMOS Current Mirror Active Load.

At first glance, this circuit appears to be only a small modification of a differential pair.

However, during the design process, it became clear that this topology introduces some of the most important concepts in analog IC design:

✅ Active loads

✅ Differential-to-single-ended conversion

✅ Gain enhancement through output resistance

✅ Design tradeoffs between gain, area, speed, and bandwidth

✅ The importance of prediction before simulation

More importantly, this project completely changed the way I think about analog design.

One lesson stood out above all:

> A beginner runs simulation and waits for the answer.
>
> An analog designer predicts the answer first and uses simulation only to verify the prediction.

That idea became the foundation of this entire project.

---

# Technology and Design Environment

Technology:

• GF180MCU PDK

Corner:

• TT

Temperature:

• 27°C

Supply Voltage:

• VDD = 1.8 V

Design Methodology:

• gm/ID Methodology

Simulation Environment:

• Xschem
• NgSpice

---

# Project Objective

To design and verify a Differential-to-Single-Ended Active-Load Differential Amplifier that satisfies the required gain, power, output common-mode voltage, and output swing specifications through systematic transistor sizing using the gm/ID methodology.

---

# Final Specifications

Design Specifications:

• Differential Gain Ad ≥ 50 V/V

• Tail Current Itail = 20 µA

• Power Consumption ≤ 40 µW

• Output Common-Mode Voltage:
0.8 V – 1.2 V

• Single-Ended Output Swing ≥ ±300 mV

• All MOSFETs must operate in Saturation Region

---

# Why Active Loads?

The previous differential pair used resistive loads.

The gain was approximately:

Ad ≈ 11.2 V/V

Although the circuit worked correctly, the gain was relatively low.

The question became:

How can gain be increased without increasing power?

The answer is:

Active Loads.

Instead of using resistors, PMOS transistors are used as loads.

The gain equation changes from:

Gain ≈ gm × RD

to

Gain ≈ gm × Rout

where:

Rout ≈ ron || rop

Since transistor output resistances can be very large, significantly higher gain can be achieved without consuming additional current.

---

# First Important Analog Design Lesson

Many students think:

Higher Gain = Better Design

This project taught the opposite.

The best design is NOT the one with the highest gain.

The best design is:

> The design that meets the specification with the lowest cost in area, power, speed, and complexity.

This idea influenced every design decision that followed.

---

# Understanding the Topology

The circuit consists of:

• NMOS Differential Pair

• PMOS Current Mirror Active Load

• Ideal Tail Current Source

This topology performs two functions simultaneously:

1. Differential Amplification

2. Differential-to-Single-Ended Conversion

This is one of the key reasons PMOS current mirrors are widely used in analog IC design.

---

# Design Flow

Rather than directly drawing the schematic, the design started from specifications.

The first step was asking:

What is the dominant requirement?

The answer:

Gain ≥ 50 V/V

Therefore the design effort focused primarily on increasing output resistance while maintaining low current consumption.

---

# Initial Design Candidate

Tail Current:

Itail = 20 µA

Balanced Condition:

ID = Itail / 2

ID = 10 µA

per branch.

Initial choices:

NMOS:

gm/ID = 15

L = 1 µm

PMOS:

gm/ID = 15

L = 1 µm

---

# Extracting NMOS LUT Data

For:

gm/ID = 15

L = 1 µm

NMOS LUT data:

VGS = 739.4 mV

ID/W = 691.5 nA/µm

gm/gds = 337.2

VDSAT = 107.4 mV

---

# Extracting PMOS LUT Data

For:

gm/ID = 15

L = 1 µm

PMOS LUT data:

VSG = 845 mV

ID/W = 156 nA/µm

gm/gds = 573.8

VDSAT = 95 mV

---

# First Hand Calculations

## Calculating gm

gm = (gm/ID) × ID

gm = 15 × 10 µA

gm = 150 µS

---

## Estimating Output Resistance

NMOS:

ro ≈ 337 / 150µS

ro ≈ 2.25 MΩ

PMOS:

ro ≈ 574 / 150µS

ro ≈ 3.83 MΩ

Output Resistance:

Rout ≈ ron || rop

Rout ≈ 1.4 MΩ

---

## Estimated Gain

Ad ≈ gm × Rout

Ad ≈ 150µS × 1.4MΩ

Ad ≈ 200 V/V (rough estimate)

Even accounting for second-order effects, the gain target of 50 V/V appeared very achievable.

---

# Learning How Designers Think

At this point an important question was asked:

Should PMOS use the same gm/ID and length as NMOS?

Answer:

Not necessarily.

A beginner often assumes:

NMOS and PMOS should match.

A designer asks:

Which device is limiting performance?

The answer depends on:

• Gain

• Area

• Speed

• Swing

• Power

Every design choice is made in service of a system objective, not symmetry.

---

# Exploring PMOS Length

PMOS was then evaluated at:

L = 0.5 µm

LUT Results:

gm/gds = 238.8

ID/W = 381 nA/µm

VSG ≈ 848 mV

The gain capability reduced, but area and capacitance improved dramatically.

This introduced another critical lesson.

---

# Tradeoff: Gain vs Area vs Speed

Increasing Length:

Advantages:

✅ Higher gain

✅ Higher output resistance

Disadvantages:

❌ Larger area

❌ Larger capacitance

❌ Lower bandwidth

❌ Lower speed

Reducing Length:

Advantages:

✅ Smaller area

✅ Smaller capacitance

✅ Better speed

✅ Better bandwidth

Disadvantages:

❌ Lower gain

This tradeoff appears repeatedly throughout analog design.

---

# First Successful Design Candidate

Chosen Dimensions:

NMOS:

W = 15 µm

L = 1 µm

PMOS:

W = 26 µm

L = 0.5 µm

---

# Operating Point Intuition

Before simulation, node voltages were predicted.

For NMOS:

VG = 0.9 V

VGS ≈ 739 mV

Therefore:

VS ≈ 0.16 V

Prediction:

VS ≈ 160 mV

Simulation:

VS ≈ 163 mV

Excellent agreement.

This reinforced another lesson:

> Simulation should verify predictions, not create them.

---

# Why VOUT Was Expected Near Mid-Rail

Many beginners ask:

How can we know where output will settle?

An experienced designer thinks:

The output node must satisfy both:

• PMOS current requirements

• NMOS current requirements

The output voltage naturally moves until both devices conduct the required current.

Since VDD = 1.8 V, a value near mid-supply is expected.

Simulation:

VOUT ≈ 1.075 V

Exactly as predicted.

---

# Design Optimization

At this stage:

Gain ≈ 126 V/V

Specification:

Gain ≥ 50 V/V

The question became:

Do we really need 126 V/V?

This is where optimization begins.

---

# Second Design Candidate

NMOS Length was reduced.

New LUT:

L = 0.5 µm

VGS = 749.7 mV

ID/W = 1.337 µA/µm

gm/gds = 153.1

VDSAT = 108 mV

New Width:

W ≈ 7.5 µm

Final NMOS:

W = 7.5 µm

L = 0.5 µm

---

# Why Recalculate Width?

One of the most important gm/ID lessons learned:

Whenever gm/ID or Length changes:

ID/W changes.

Therefore Width must be recalculated.

A common beginner mistake is changing Length and leaving Width unchanged.

This shifts the operating point and destroys the original design assumptions.

---

# New Gain Prediction

Using:

NMOS gm/gds = 153

PMOS gm/gds = 239

Predicted Gain:

≈ 93 V/V

Predicted:

≈ 39 dB

---

# Simulation Verification

AC Simulation:

Measured Gain:

≈ 39 dB

≈ 89 V/V

Prediction and simulation matched closely.

This was one of the most satisfying moments of the project.

---

# Why .OP Was Run Again

A natural question arose:

"If gain already meets the specification, why rerun .op?"

Answer:

Because analog design starts with bias.

Whenever a transistor size changes, we must verify:

• Current

• Node voltages

• Saturation

Only after the bias point is correct should gain and bandwidth be trusted.

---

# Saturation Verification

NMOS:

VDS ≈ 925 mV

VDSAT ≈ 108 mV

NMOS remained deeply saturated.

PMOS:

VSD ≈ 725 mV

VDSAT ≈ 95 mV

PMOS remained deeply saturated.

Specification satisfied.

---

# Understanding Distortion and Clipping

During transient analysis, clipping was observed.

Initially this looked like a design problem.

It wasn't.

The actual issue was signal amplitude.

A larger differential input was applied than intended.

This pushed the differential pair beyond small-signal operation.

---

# What Happens Physically?

A differential pair works by current steering.

At equilibrium:

MN1 ≈ 10 µA

MN2 ≈ 10 µA

As differential input increases:

Current shifts.

Eventually:

MN1 ≈ 20 µA

MN2 ≈ 0 µA

or vice versa.

This is known as complete current steering.

At that point:

• Gain becomes nonlinear

• Distortion increases

• One side may leave saturation

The amplifier is no longer operating in its linear region.

This was a valuable lesson in large-signal behavior.

---

# Final Design Results

Differential Gain:

≈ 89 V/V

≈ 39 dB

Power Consumption:

≈ 36 µW

Tail Current:

20 µA

Output Common-Mode Voltage:

≈ 1.075 V

Single-Ended Output Swing:

≈ ±335 mV

All MOSFETs in Saturation:

PASS

---

# Final Specification Verification

✅ Differential Gain ≥ 50 V/V

Measured: 89 V/V

PASS

✅ Power ≤ 40 µW

Measured: 36 µW

PASS

✅ Tail Current = 20 µA

Measured: 20 µA

PASS

✅ Output Common-Mode Voltage

Measured: 1.075 V

PASS

✅ Single-Ended Output Swing ≥ ±300 mV

Measured: ±335 mV

PASS

✅ All MOSFETs Operate in Saturation

PASS

---

# Key Lessons Learned

🔹 Simulation should verify predictions, not discover answers.

🔹 The best design is the one that meets specifications with minimum cost.

🔹 Active loads increase gain through higher output resistance.

🔹 Higher gain is not always better.

🔹 Gain, area, bandwidth, and speed are tightly coupled.

🔹 Every transistor dimension represents a tradeoff.

🔹 Current is often the fastest way to debug an analog circuit.

🔹 Analog design starts with understanding bias.

🔹 gm/ID provides a systematic framework for transistor sizing.

🔹 Prediction followed by verification is one of the most valuable habits an analog designer can develop.

---

# Conclusion

This project successfully demonstrated the design and verification of a Differential-to-Single-Ended Active-Load Differential Amplifier using the GF180MCU process and the gm/ID methodology.

Beyond meeting specifications, the project provided valuable insight into how experienced analog designers think, predict circuit behavior, evaluate tradeoffs, and make design decisions.

The journey reinforced a simple but powerful principle:

> Analog design is not about making circuits bigger, stronger, or more complex.

> It is about understanding tradeoffs and meeting specifications with elegance and efficiency.

This project serves as a foundation for the next step in the learning journey:

🚀 5-Transistor OTA Design
