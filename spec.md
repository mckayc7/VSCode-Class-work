# Concrete Slabs

## Purpose
The app needs to compute the conrete needed for a rectangular slab

## Inputs
The user enters the slab length and width (ft), thickness (in), a waste factor (%), and the price of concrete per cubic yard

## Calculations
added_concrete = waste factor (%) / 100 + 1
real volume (yd^3) = = length (ft) * width (ft) * (thickness(in)/12) / 3 (ft)
volume (yd^3) = real volume (yd^3) + (real volume (yd^3) * added_concrete)
total cost ($) = volume (yd^3) * price of concrete per cubic yard


## Outputs
The app shows the volume in cubic yards (rounded up to the nearest quarter yard, which is how concrete is ordered) and the total cost

## Look and feel
colors are green, blue, and grey
should have a visual of dimensions of volume of concrete with the concrete as grey color

## Validation
At least one test case you have worked by hand: the exact inputs,
and the outputs the app must produce.

## Out of scope
Things the app will NOT do, so the agent doesn't add them.