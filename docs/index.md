# ADSR Envelope Generator

![ADSR](./assets/images/ADSR_dark.png#only-dark){: width="144", align="right"}
![ADSR](./assets/images/ADSR_light.png#only-light){: width="144", align="right"}

An envelope generator modeled on the 555-based ADSR originally proposed by Jonathan Jacky. It's worth looking through the evolution of this design - see references below. 

* Module size: 6HP (30mm)
* Power: 10mA (+12V); 9mA (-12V)

!!! repository "Project Source"

    The project files, including schematic and layout, are available on [github](https://github.com/xdylanm/adsr-eg).

## Features

* ADSR controls
* Gate input: control signal, button or loop input
    * Schmitt-trigger buffer for control signals and button
    * Independent rate control for the loop (equivalent to sustain)
* Trigger (rising edge) input: initiate an A/D or A/R cycle
* Loop mode with adjustable sustain period (LFO behaviour if desired)

When a gate signal is removed, the release phase of the cycle will start immediately (independent of the envelope level). When a trigger input is applied, the envelope will always go to its maximum level before transitioning to the decay (gate signal still applied) or release phase (gate signal absent). See the [Design](theory.md) section for more details.

## Documentation

* [Design](theory.md)
* [Assembly](assembly.md)
* [Schematic](assets/schematic.pdf)

## References

1. Jonathan Jacky, "Two-chip generator shapes synthesizer's sounds", Electronics #11, September 1980 : 137-138 [archived on yusynth.net](https://yusynth.net/archives/Electronics/J-Jacky-ADSR-1980.pdf>)
2. Rene Schmitz, "The Fastest Envelope in the West", online [schmitzbits.de](https://www.schmitzbits.de/adsr.html) (ca. 2000) 
3. Yves Usson, "Envelope generator ADSR (newer version)", online [yusynth.net](https://yusynth.net/Modular/EN/ADSR/index_new.html) (2010)
4. Kassutronics, "Precision ADSR", online [kassu2000.blogspot.com](https://kassu2000.blogspot.com/2015/05/precision-adsr.html) (2015)
5. Moritz Klein, "mki x es.edu Envelope", Erica Synths, online [ericasynths.lv](https://www.ericasynths.lv/media/EG_MANUAL_v3.pdf) (2021)
6. Eddy Bergman, "Synthesizer Build part-63", online [eddybergman.com](https://www.eddybergman.com/2024/12/ADSR%20Rene%20Schmitz.html) (2024)



