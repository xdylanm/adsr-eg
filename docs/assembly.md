# Assembly Guide

## SMT Components

If manually placing SMT components, use of a stencil and solder paste is recommended. 

1. U1 and U2
2. D5 and D6
3. R24 and R25 (ferrites)
4. remaining SMT diodes D1-D3, D7-D8
5. Ceramic capacitors C1, C3, C4, C6-C10
6. Tantalum capacitors C2 and C5
7. BJTs Q1-Q5 and MOSFET Q6
8. Resistors R1-R17, R21-R22, R26-R28 (R18-R20 and R23 don't exist)

!!! note

    If the input latches up, replace D7 with a 100k pull-down.

## THT Components

Once all SMT components are placed, solder

1. C11 and C12 (back side)
2. J4 (back side)
3. SW2
4. J1 and J2
5. RV1-RV5 (note values *and* taper)
6. SW1

Place and solder the 3mm LED once the faceplate is fitted.

## BOM

[Download (.csv)](assets/bom.csv)

{%include-markdown "assets/bom.md"%}
