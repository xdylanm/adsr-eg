.. role:: strike
    :class: strike

ADSR Envelope Generator
=======================

Overview
^^^^^^^^

An envelope generator modeled on the 555-based ADSR originally proposed by Jonathan Jacky. It's worth looking through the evolution of this design - see references below. There are a few highlights are noted below.

Off state
---------

The original variants don't really decay back to 0V in the off state. Between the small signal diodes and the VCE of the input buffer, I could only get it to about 200mV. The only discussion I found on this was on Eddy Bergman's site (thank you), and is resolved with precision diode implementation in the Kassutronics version (very clever). Eddy Bergman came up with a partial fix by switching to Schottky diodes, which could be used to free up another opamp, but concludes that the precision diode version (Kassutronics) is better. 

The Extra Opamp
---------------

Rene Schmitz's version uses two OPAs (nicely satisfied with a TL072). Later designs (YuSynth+) added an inverting output (also nicely featured in Moritz Klein's non-555 version), but that leaves one more...

  * Blink an LED (YuSynth, Kassutronics). In my opinion, better replaced with a BJT (see Moritz Klein's version), but the Kassutronics version neatly combines the inverting and blinking and uses the other for the precision diode.
  * Loop trigger (Moritz Klein). In combination with the precision diode for the release path, I had to give up the inverting output.
  * Inverted output. This comes in a few forms, but the attenuverter stage is probably the most flexible. I liked the way that Moritz Klein did it with the offset, giving an inverted control signal, e.g. for a "normally open" filter.

Decay vs. Release
-----------------

When the gate is removed before the decay completes, Yves Usson added a MOSFET to force the capacitor to discharge through the release path. Fancy.

Various Input Stages
--------------------

Generally, a combination of Schmitt-trigger buffer and glitch generator are used, but the details change from design to design. I tried a classic Schmitt trigger (see Kassutronics or YuSynth), but was concerned that it would hold up the off-state voltage (before I found the precision diode design) and went back to Rene Schmitz's buffer. 

Manual Gate
-----------

This is an easy addition, see e.g. Kassutronics.

(Re-)Triggering
---------------

There's some discussion about how no true :strike:`Scotsman` musician could do without a re-triggerable ADSR. I'm a hack, and I couldn't figure it out easily, so maybe next time? The MFOS version looks like a good place to start.


References
^^^^^^^^^^

1. Jonathan Jacky, "Two-chip generator shapes synthesizer's sounds", Electronics #11, September 1980 : 137-138 [`archived on yusynth.net <https://yusynth.net/archives/Electronics/J-Jacky-ADSR-1980.pdf>`_]
2. Rene Schmitz, "The Fastest Envelope in the West", online [`schmitzbits.de <https://www.schmitzbits.de/adsr.html>`_] (ca. 2000) 
3. Yves Usson, "Envelope generator ADSR (newer version)", online [`yusynth.net <https://yusynth.net/Modular/EN/ADSR/index_new.html>`_] (2010)
4. Kassutronics, "Precision ADSR", online [`kassu2000.blogspot.com <https://kassu2000.blogspot.com/2015/05/precision-adsr.html>`_] (2015)
5. Moritz Klein, "mki x es.edu Envelope", Erica Synths, online [`ericasynths.lv <https://www.ericasynths.lv/media/EG_MANUAL_v3.pdf>_`] (2021)
6. Eddy Bergman, "Synthesizer Build part-63", online [`eddybergman.com <https://www.eddybergman.com/2024/12/ADSR%20Rene%20Schmitz.html>`_] (2024)

Other Variants
^^^^^^^^^^^^^^

There are some other interesting design ideas out there that I found while investigating the design of this ADSR.

Befaco VC ADSR
--------------

    * [`befaco.org <https://www.befaco.org/vc-adsr/>`_]
    * Discrete logic based ADSR sequence (latches & flip-flops)
    * voltage control on all stages
    * switch between gate and trigger mode
    * shape-controllable between exponential and linear rise and fall.

MFOS (Ray Wilson) ADSR Envelope Generator
-----------------------------------------

    * [`musicfromouterspace.com <https://musicfromouterspace.com/analogsynth_new/ADSR001/ADSR001.html>`_]
    * discrete logic based ADSR sequence (latches)
    * re-triggerable A+D/R

