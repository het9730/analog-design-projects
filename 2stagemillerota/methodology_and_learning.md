# What This OTA Really Taught Me

When I started this project, I thought I was designing an amplifier.

By the time I finished, I realized I was actually learning how interconnected analog circuits really are.

The final OTA successfully achieved all target specifications:

## Final Performance Summary

| Parameter              | Target      | Achieved  |
| ---------------------- | ----------- | --------- |
| DC Gain                | ≥ 60 dB     | 77.6 dB   |
| UGB                    | ≥ 10 MHz    | 10.53 MHz |
| Phase Margin           | ≥ 60°       | 64.7°     |
| Power Consumption      | ≤ 100 µW    | 34.7 µW   |
| Positive Slew Rate     | ≥ 5 V/µs    | 6.5 V/µs  |
| Negative Slew Rate     | —           | 12.5 V/µs |
| Load Capacitance       | 1 pF        | 1 pF      |
| Supply Voltage         | 1.8 V       | 1.8 V     |
| Saturation Requirement | All devices | Satisfied |

However, these numbers only tell the final chapter of the story.

The real story is how each specification fought against the others.

---

## The First Illusion: "If I Size It Correctly, It Will Work"

At the beginning, the design process seemed straightforward.

Choose a gm/ID target.

Use lookup tables.

Extract transistor widths.

Run simulation.

The assumption was that transistor sizing was the primary challenge.

The first simulations quickly disproved this idea.

The OTA worked.

The gain was respectable.

Yet several specifications were still not satisfied.

This was the first indication that analog design is not merely transistor sizing.

It is system design.

---

## The Day Gain Stopped Impressing Me

One of the most surprising moments occurred when the OTA achieved excellent gain while still exhibiting poor phase margin.

Initially, every gain improvement felt like progress.

A gain of 70 dB felt better than 60 dB.

A gain of 80 dB felt even better.

Then something became obvious:

A high-gain amplifier with poor stability is not a successful design.

The amplifier may amplify signals, but it may also ring excessively, settle slowly, or even oscillate in closed-loop operation.

This project taught me that gain and stability are fundamentally different objectives.

High gain is desirable.

Stable gain is essential.

---

## Every Improvement Had a Cost

One of the most important lessons from the project was that no specification exists in isolation.

For example:

Increasing current improved transconductance.

Higher transconductance increased bandwidth.

Higher bandwidth moved the unity-gain crossing frequency.

Moving the unity-gain crossing frequency altered phase margin.

Increasing current also increased power consumption.

A change that initially appeared beneficial produced consequences throughout the entire amplifier.

The same pattern appeared repeatedly.

Improving one specification often degraded another.

The design eventually became an exercise in managing trade-offs rather than maximizing individual metrics.

---

## The Most Valuable Equation Was Not a Gain Equation

Initially I expected transistor equations to dominate the project.

Instead, the most influential concepts became:

* Pole locations
* Zero locations
* Compensation
* Feedback stability

The project gradually shifted from transistor-level thinking to system-level thinking.

Eventually the most important questions were no longer:

"What width should I choose?"

but rather:

"Where is the dominant pole?"

"What is limiting phase margin?"

"Why is the unity-gain crossing occurring here?"

This transition represented a major change in my understanding of analog design.

---

## The Nulling Resistor Changed Everything

The largest single improvement in the entire project came from a component that initially appeared insignificant.

The OTA used Miller compensation to achieve stability.

However, Miller compensation introduces a right-half-plane zero that can reduce phase margin.

Using the measured operating point:

gm₉ ≈ 44.2 µS

suggested a nulling resistor near:

Rz ≈ 1/gm₉ ≈ 22.6 kΩ

After investigation and tuning, a value of:

Rz = 27 kΩ

was selected.

This small resistor produced one of the largest improvements observed during the project.

Phase margin improved from approximately 58.6° to 64.7° while preserving gain and bandwidth.

This was the moment I truly appreciated the importance of pole-zero engineering.

The improvement did not come from larger transistors.

It did not come from more current.

It came from understanding the circuit.

---

## An Unexpected Observation: Current Influenced Stability

One observation initially seemed contradictory.

Increasing current was expected to affect:

* gm
* bandwidth
* slew rate

Yet it also influenced phase margin.

The explanation emerged later.

Changing current altered gm₉.

Changing gm₉ altered the compensation network.

Changing the compensation network altered the effective pole-zero locations.

Those pole-zero shifts ultimately influenced phase margin.

The lesson was clear:

In analog design, the effect of a design decision is rarely confined to a single specification.

---

## The Simulator Was Not the Designer

Perhaps the most important lesson of the project was learning the correct role of simulation.

Early in the project, many iterations followed the pattern:

Change parameter.

Run simulation.

Observe result.

Repeat.

Eventually a more effective workflow emerged:

Predict.

Calculate.

Analyze.

Then simulate.

Simulation became a verification tool rather than a discovery tool.

This change dramatically improved both design efficiency and understanding.

---

## What the gm/ID Methodology Did Well

The gm/ID methodology proved extremely effective for obtaining a functional first-pass design.

It provided:

* Systematic transistor sizing
* Strong physical intuition
* Rapid design iteration
* Efficient operating point selection

The OTA became functional remarkably quickly using gm/ID-based sizing.

However, the project also revealed the limitations of the methodology.

gm/ID can size transistors.

It cannot determine stability.

It cannot place poles.

It cannot choose a compensation strategy.

It cannot replace engineering judgment.

The project ultimately demonstrated that gm/ID is an excellent sizing methodology but not a substitute for analog design knowledge.

---

## Final Reflection

The final OTA met every primary specification.

But the most valuable outcome was not achieving:

77.6 dB gain

10.53 MHz UGB

64.7° phase margin

34.7 µW power consumption

The most valuable outcome was learning how those numbers are connected.

This project transformed my view of analog design from a transistor-sizing exercise into a problem of understanding interactions among gain, bandwidth, compensation, stability, power, slew rate, and device physics.

That change in perspective was the most important result of the entire design.
