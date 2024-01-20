# Notes:

## Definitions

OFDMA
: Orthogonal Frequency Division Multiple Access

MIMO
: Multiple Input and Multiple Output

Beamforming
: Beamforming is a technique used in MIMO to improve the signal-to-noise ratio of received signals, eliminate undesirable interference sources, and focus transmitted signals to specific locations. To change the directionality of the array when transmitting, a beamformer controls the phase and relative amplitude of the signal at each transmitter, in order to create a pattern of constructive and destructive interference in the wavefront.

Delay Spread
: TODO

Coherence Bandwidth
: TODO

Frequency Flat Fading
: TODO

Frequency Selective Fading
: TODO

Fast Fading
: TODO

Slow Fading
: TODO

Doppler
: TODO

Passband
: TODO

Baseband
: TODO

Impulse Response
: the output of a system when presented with a brief input signal called an impulse

Channel Impulse Response
: the output of a channel when presented with a brief input signal called an impulse

Multipath
: a signal that arrives at the mobile user through multiple paths  (signal reflecting off of things)

## Wireless modes

| Generation  | Year   | Bandwidth      | technology           | Notes |
| ----------- | ------ | -------------- | -------------------- | ---------- |
| 1G          | 1980   | 30K            | Analog               | Voice Only | 
| 2G          | 1990   | 30K - 1.25M    | Digital              | Voice and low rate. IS-95 CDMA.  IS-136 TDMA | 
| 3G          | 2000   | 3mbps          | Digital              | Wideband.  CDMA2000. | 
| 4G LTE      | 2010   | 100-300Mbps    | Digital              | Verizon operates at 700M, 800M, 1.7/1.9/2.1G.  Channel bandwidth 5M, 1M, 15M or 20Mhz.  MIMO - OFDMA multiplexing. | 
| 5G          | 2020   | 100-300Mbps    | Digital              | Operates at low-band (850M/1.7-2.1G), mid-band(2.4-4G)(c-band 3.7G-3.9G), and high band 24-47Ghz | 


## Channel Modeling

A channel between a mobile receiver and a base station is modeled as a multi-path, multiple-antenna channel.  This is typically described by a channel matrix.

To start you model the basic problem using a single antenna.

Goal is to characterize the channel in terms of the channel impulse reponse.

![Multipath Diagram](images/Channel_Modeling_multipath.png)

At the mobile reciever location at $(d,\theta)$ the received signal is:

$r(t) = \displaystyle\sum_{n=1}^{n(d,\theta)}\beta_n(d,\theta)s(t-\tau_n(d,\theta))$ 

where $s(t)$ is the tramitted signal at time $t$ and
$n(d,\theta)$ is the numer of paths between the base station and the mobile receiver.
$\tau_n(d,\theta)$ is the time delay of the nth path.
Therefore $s(t-\tau_n(d,\theta))$ is the signal minus the time delay
$\beta_n(d,\theta)$ is the path gain of the nth pah (attenuation)
$r(t)$ is the recieved signal at the mobile receiver.

All of the copies of the signal can add together constructively or destructively.