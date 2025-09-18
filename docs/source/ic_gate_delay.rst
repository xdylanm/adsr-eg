ATTiny13A Gate Delay Design
===========================

Input
-----

* Use the edge triggered interrupt on the pin and a state variable to read rising & falling edges. 
* Read the ADC and calculate the delay time on the leading edge of the gate pulse.

Delay Timing
------------

Ideally, we'd like to calculate :math:`t_d = T_{d0} \exp( a x )` where :math:`T_{d0}` is the minimum delay time (e.g. 2ms), :math:`x` is the ADC value (0-1023) and :math:`a` is a scaling parameter that sets the maximum delay time (e.g. 2s). In practice, this is not well suited for the ATTiny, which doesn't support floating point calculations. Instead, I'll approximate this function using the upper 3 bits of the ADC value to generate an exponential, and use the remaining lower bits to linearly interpolate between.

.. math::

    k_d = k_{d0} 2^{b_{9-7}} (1 + b_{6-0} / 2^{7} )

With a minium delay of :math:`k_{k0}` and a maximum of :math:`k_{d0} 2^8 = 256 k_{d0}`.

Using the upper four bits increases the maximum to :math:`k_{d0} 2^16`, which is possible if kd0 = 1.

.. math::

    k_d = 2^{b_{9-6}} (1 + b_{5-0} / 2^{6} )

While correct mathematically, to implement this we need to take care not to overflow. Expanding

.. math::

    k_d &= 2^{b_{hi}} (1 + b_{lo} / 2^{n_\ell} ) \\
        &= 2^{b_{hi}} + b_{lo} * 2^{b_{hi}} / 2^{n_\ell} \\
        &= 2^{b_{hi}} + b_{lo} * 2^{b_{hi} - n_\ell}

.. code-block:: c++

    uint16_t adc = read_adc();
    uint8_t ubits = (adc & 0x0380) >> 7;
    uint8_t exp_factor = (1 << ubits);
    uint8_t lbits = (adc & 0x007F);
    uint16_t kd;
    if (ubits < 7) { 
        kd = kd0 * exp_factor  + ( ( kd0 * exp_factor * lbits) >> 7  );
    } else {
        kd = kd0 * exp_factor  + kd0 * ( ( exp_factor * lbits) >> 7  );
    }

Here, `kd` is the number of timer ticks to count and `kd0` is the scaling factor that sets the minimum delay.

Timers
------

The internal clock on the ATTiny13A defaults to 9.6MHz. A single timer/counter (TC) is available with prescalings of 8, 64, 256, or 1024. With a prescaling of 8, the timer overflow interrupt (TIM0_OVF_vect) will create a clock with a period of 0.213us.  

Control Flow
------------

Time 0 is indexed to the leading edge of the gate (interrupt). At this event, 

* the current value of the TC (extended) is recorded as the start time, and
* the ADC value is read and the delay (in number of ticks) is calculated. Add this to the start time to get the start of the output pulse (if this results in an overflow, flag it).

On the trailing edge of the gate input (interrupt), compute the gate pulse length and determine the stop time of the output pulse (flag if overflow).

On a TC overflow, increment the upper byte. The upper byte can roll over (total time is around 7s).

Poll the extended TC value to determine when to start and stop the output pulse.


