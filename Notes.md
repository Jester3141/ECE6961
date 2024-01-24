# Notes:

## Defnitions

Tap Delay Line Model
: TODO

Delay Spread
: The spread of delays in a system defined as $T_d = T_{ds} = \tau_{max} - \tau_{min}$.  This depends on the environment.  In addition, the lower the frequency, the higher the delay spread.  This is because higher frequencies attenuate more over distance.  In an urban setting a signal at 2g is in the order of a few microseconds ($\mu s$). In an indoor setting, that would be smaller on the order of a few undred of nanoseconds ($ns$)

Delay Profile
: A graph of the magnitued of the impulse responses $|h(t,\tau)|$ vs the delay $\tau$

Nyquist-Shannon sampling theorem
: The theorem states that the sample rate must be at least twice the bandwidth of the signal to avoid aliasing. In practice, it is used to select band-limiting filters to keep aliasing below an acceptable amount when an analog signal is sampled or when sample rates are changed within a digital signal processing function.

Sinc
: $\displaystyle sinc x = \frac{sin x}{x}$



##  Review.  Complex baseband impulse response.

$h(t,\tau) = \displaystyle\sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)}\delta(\tau-\tau_n(t))$

$y(t) = \displaystyle \int_{-\inf}^{\inf}h(t,\tau)x(t-\tau)d\tau$

$\phi_n = -2\pi f_c \tau_n$

We are going to first look at the frequency flat fading case to see how that simplifies things.  Then we will go in the tap delay line channel model.

Small scale changes to movement on the order of a few wavelengths $\lambda_c$.  In this case, $N(d,\theta), \beta_n(d,\theta), \tau_n(d,\theta) \approx constant$.  But this gets multiplied by $f_c$ so the phase $\phi_n$ can change significantly.  $\therefore h_b(\tau)$ can change significantly even with small scale changes.

**NOTE: From now on we are just considering the baseband channel impulse response.**

For the following graph, the x axis is $\tau$ and the y axis is the magnitude of the impulse response $|h(t,\tau)|$.  This is a delay profile.  Often times these delay profiles wil have a clustered structure.  This is due to reflectors reflecting with many different delays. 

![Delay Profile](images/delay_profile.png)

A big magnitue at $\tau = 0$ indicates a line of sight.

The book uses $T_d$ for delay spread.  The instructor is going to use $\tau_{DS}$  Delay spread is the longest delay minuse the minimum delay. $T_d = \tau_{DS} = \tau_{max} - \tau_{min}$

Typical delay spread for 4G LTE was microseconds.  The underwater channel was much slower (milliseconds).

$y(t) = \displaystyle \int_{-\inf}^{\inf}h(t,\tau)x(t-\tau)d\tau$

Now we are going to simplify this. We kind of shift everything over.

$y(t) = \displaystyle \int_{0}^{\tau_{DS}}h(t,\tau)x(t-\tau)d\tau$

Now we make some assumptions.  We will consider the frequency flat case.  If $x(t) \approx constant$ over the duration $[0,\tau_{DS}]$  That means that it no longer varies with $\tau$, and can thusly be pulled outside the integral.


$y(t) \approx \displaystyle x(t)\int_{0}^{\tau_{DS}}h(t,\tau)d\tau$

We are going to call the remaining integral $\displaystyle \int_{0}^{\tau_{DS}}h(t,\tau)d\tau = V_0(t)$

$y(t) \approx \displaystyle x(t) V_0(t)$ (frequency flat multiplicative relationship)

Assume a symbol duration of $T_s$.  (in the time domain) If the $T_s \gg \tau_{DS}$, then the time delay shift wont cause much harm to the signal. 

In the frequency domain if $\frac{1}{T_s} \ll \frac{1}{\tau_{DS}}$, then the time delay shift won cause much harm to the signal.  (Reminder that the bandwidth $W$ is the $\frac{1}{T_s}$ and $\frac{1}{\tau_{DS}}$ roughly gives you the coherence bandwidth)  Therefore $W \ll W_c$

$y(t) = x(T)\cdot V_0(t)$

$V_0(t) = \displaystyle \int_0^{\tau_{DS}} \sum_n \alpha_n e^{j\phi_n(t)} \delta(\tau-\tau_n) d\tau$

Remember that the integration of a delta is 1.  Next we rearange.  That is $\displaystyle \int_0^{\tau_{DS}}  \delta(\tau-\tau_n) d\tau = 1$

$V_0(t) = \displaystyle \sum_n \alpha_n e^{j\phi_n(t)} \int_0^{\tau_{DS}}  \delta(\tau-\tau_n) d\tau$

$V_0(t) = \displaystyle \sum_n \alpha_n e^{j\phi_n(t)}$
This is a single tap.

$y(t) = x(T)\cdot V_0(t)$ is a one-tap channel.  All the paths contribute to the same tap.


-------

Most of the time we are not going to have a frequency flat channel.  Instead it will be a channel with frequency selective fading.  That means we will need multiple taps.  

$L = Number\space of\space Taps$

**Tapped Delay Line Model**
$y(t) = \displaystyle \sum_{l=0}^{L-1} x(t-\frac{l}{W}) V_l(t)$


![Tapped Delay Line Model](images/tapped_delay_line_model.png)

$\displaystyle\frac{1}{W} = resolution$ 

Now We want to see which paths correspond to witch taps, and how do we determine the V function for each taps.  Also how to we determine the number of taps.

![Band Limited](images/band_limited.png)

This is the forier domain image of the transmitted signal.

Assume we have a signal $|x(f)|$ with Bandwidth $W$.

The Nyquist theorem specifies that a sinuisoidal function in time or distance can be regenerated with no loss of information as long as it is sampled at a frequency greater than or equal to twice per cycle.

**Iterpolation formula**
$x(\tau) = \displaystyle \sum_{l=-\infin}^{\infin} x(lT)sinc(\frac{\tau-lT}{T})$

$T = \frac{1}{W}$

Now we derive the interpolation formula to get the tap delay line formula.

We are going to rewrite $x(t)$ as $x(t-\tau)$ where $t$ is a fixed constant, and we are sampling the $\tau$

$\displaystyle lT = l \cdot\frac{1}{W} = \frac{l}{W}$

$x(t-\tau) = \displaystyle \sum_{l=-\infin}^{\infin} x(t-\frac{l}{W}) sinc(W(\tau-\frac{l}{W}))$

Now we substitue $x(t-\tau)$ into the filtering formula.

$y(t) = \displaystyle \int_{0}^{\tau_{DS}}h(t,\tau)\sum_{l=-\infin}^{\infin} x(t-\frac{l}{W}) sinc(W(\tau-\frac{l}{W}))   d\tau$
Now we do more subsitution for $h(t,\tau)$ from the fomula above:

$h(t,\tau) = \displaystyle\sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)}\delta(\tau-\tau_n(t))$

This gives

$y(t) = \displaystyle \int_{0}^{\tau_{DS}}\displaystyle\sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)}\delta(\tau-\tau_n(t))\sum_{l=-\infin}^{\infin} x(t-\frac{l}{W}) sinc(W(\tau-\frac{l}{W}))   d\tau$

Now we rearrange

$y(t) = \displaystyle \sum_{l=-\infin}^{\infin} x(t-\frac{l}{W}) \sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)} \int_{0}^{\tau_{DS}} \delta(\tau-\tau_n(t)) sinc(W(\tau-\frac{l}{W})) d\tau$

Now we simplify the integral.

If we have $\displaystyle \int_0^{\tau_{DS}} \delta(\tau-\tau_n)\cdot f(\tau) d\tau$ we can replace $\delta(\tau-\tau_n)\cdot f(\tau)$ with $\delta(\tau-\tau_n)\cdot f(\tau_n )$

Then we can bring out the $f(\tau_n)$ function out of the integral because $\tau_n$ is at a specific N and is constant.

Using this back on our main function we evaluate the sinc fuction at $\tau_n$, then the integral evaluates to 1.

$y(t) = \displaystyle \sum_{l=-\infin}^{\infin} x(t-\frac{l}{W}) \sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)} sinc(W(\tau_n-\frac{l}{W}))$

The second part of the sum is the $V_l(t)$ tap.

$V_l(t) = \displaystyle \sum_{n=1}^{n(t)}\alpha_n(t)e^{j\phi_n(t)} sinc(W(\tau_n-\frac{l}{W}))$

Left off at 53:00







































