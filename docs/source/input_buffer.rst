Input Buffer
============

References
----------
1. Rene Schmitz, "Third ADSR Schematic", [`schmitzbits.de <https://schmitzbits.de/adsr.html>`_]
2. P. Horowitz and W. Hill, *The Art of Electronics*, 3rd ed., Cambridge University Press (2016)
3. Kassutronics, "Precision ADSR", [`kassu2000.blogspot.com <https://kassu2000.blogspot.com/2015/05/precision-adsr.html>`_]

Design
------

The gate input is passed through a buffer. In the schmitzbits.de design [1], the input is passed through two cascaded BJT inverters, with an optional feedback resistor to increase sensitivity for slowly varying inputs. I experimented with an alternative Schmitt trigger buffer (Section 2.2 in ref. 2):

.. image:: ./_static/images/input_buffer_schmitt.png

The buffer has a rising edge threshold of approximately :math:`V_{be} + \Delta V`, where :math:`\Delta V \equiv R_4/R_3 V_{CC}`. When :math:`R_3 = 22`, :math:`\Delta V \approx 50\mathrm{mV}`, and :math:`\Delta V \approx 200\mathrm{mV}` for :math:`R_3 = 100`. The low output will also have an offset of approximately :math:`V_{CE,sat} + \Delta V`, where :math:`V_{CE,sat} \approx 100\mathrm{mV}`.

.. csv-table:: Input Buffer Thresholds 
    :header: "R3 (Ω)", "ΔV (mV)", "Rising threshold (V)"
    :widths: 20, 20, 20

    22, 50, 0.85
    100, 200, 1.1

A circuit simulation can be found on `falstad.com <https://tinyurl.com/26fdct39>`_. 

Testing
-------

To check the circuit, I applied a 10Vpp triangle wave (centered at 0V) and varied R3. 

.. image:: ./_static/images/schmitt_buffer_22R_wide.png
    :width: 800px

With R3=22, :math:`V_{th,R} = 0.87` V, :math:`V_{th,F} = 0.82` V, :math:`\Delta V = 50` mV, and the low-level output is approximately 150mV.

.. image:: ./_static/images/schmitt_buffer_22R_zoom.png
    :width: 800px

With R3=100, :math:`V_{th,R} = 1.2` V, :math:`V_{th,F} = 1.0` V, :math:`\Delta V = 200` mV, and the low-level output is approximately 350mV.

.. image:: ./_static/images/schmitt_buffer_100R_zoom.png
    :width: 800px

Conclusions
-----------

The problem with this circuit is the low-level (OFF) voltage: the release path decays to this level, which creates a long tail on the envelope and holds the output of the envelope at that level. In Rene Schmitz's design, the low output is pulled down to the :math:`V_{CE,sat.}` of the output transistor in the buffer, which should be 50-80mV. This is similar to what you'd expect for a rail-to-rail comparator (e.g. `TLV360x <https://www.ti.com/lit/ds/symlink/tlv3601.pdf>`_), but the level is still held up by the very small current though the small-signal diode. In the Kassutronics circuit, a classic Schmitt-trigger buffer is used (albeit with a :math:`10\Omega` resistor from the emitter to ground), so clearly this is OK in practice when the diode issue is addressed.

