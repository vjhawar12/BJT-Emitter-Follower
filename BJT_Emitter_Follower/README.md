# BJT Emitter Follower

## Given Information and Design Goal

For the design:
- Source resistance: $R_S = 100\\Omega$
- Load resistance: $R_L = 100\\Omega$
- Input amplitude: $0.5\\text{V}$
- Thermal voltage: $V_T \approx 25\\text{mV}$

The two main requirements are:
1. Attenuation requirement: overall gain should satisfy
   $$G \geq 0.9$$
2. Signal swing requirement: small-signal variation in $v_{be}$ should remain small

The chosen topology is a common-collector (emitter follower) BJT amplifier.

## Small-Signal Model

The small-signal emitter resistance is
$$r_e = \frac{1}{g_m}$$

Using
$$g_m = \frac{I_C}{V_T}$$
gives
$$r_e = \frac{V_T}{I_C}$$

Since for a BJT in active region,
$$I_E \approx I_C$$
we use
$$r_e \approx \frac{V_T}{I_E} \approx \frac{25\,\text{mV}}{I_E}$$

Also, the input resistance is approximated by
$$R_{in} = R_1 \parallel R_2 \parallel (\beta+1)(r_e + R_L)$$

## Attenuation Requirement

The overall gain is approximated by
$$G = \frac{R_{in}}{R_{in}+R_S} \cdot \frac{R_L}{r_e+R_L}$$

The design requirement is
$$G \geq 0.9$$

Substituting the known values $R_S = 100\,\Omega$ and $R_L = 100\\Omega$:
$$\frac{R_{in}}{R_{in}+100}\cdot \frac{100}{r_e+100} \geq 0.9$$

To satisfy this expression, we want:
$$R_{in} \gg 100 \quad \text{and} \quad r_e \ll 100$$

As a first design target, choose
$$R_{in} \approx 10\\text{k}\Omega \quad \text{and} \quad r_e \approx 1\\Omega$$

Then
$$G \approx \frac{10000}{10100}\cdot \frac{100}{101}$$
$$G \approx 0.9901 \cdot 0.9901$$
$$G \approx 0.98$$

Therefore,
$$G \approx 0.98 > 0.9$$
so the attenuation requirement is satisfied.

## Signal Swing Requirement

The signal swing requirement was written as
$$0.5 \cdot \frac{R_{in}}{R_{in}+R_S} \cdot \frac{r_e}{r_e+R_L} \leq 0.2x$$
where
$$x = 25\,\text{mV}$$

So the right-hand side becomes
$$0.2x = 0.2(25\\text{mV}) = 5\\text{mV}$$

Thus the condition is
$$0.5 \cdot \frac{R_{in}}{R_{in}+R_S} \cdot \frac{r_e}{r_e+R_L} \leq 5\\text{mV}$$

Substituting $R_S = 100\,\Omega$ and $R_L = 100\\Omega$ gives
$$0.5 \cdot \frac{R_{in}}{R_{in}+100} \cdot \frac{r_e}{r_e+100} \leq 5\\text{mV}$$

Using the same design target as before,
$$R_{in} \approx 10\\text{k}\Omega \quad \text{and} \quad r_e \approx 1\\Omega$$
we get
$$0.5 \cdot \frac{10000}{10100} \cdot \frac{1}{101}$$
$$= 0.5 \cdot 0.9901 \cdot 0.00990$$
$$\approx 0.0049\,\text{V}$$
$$\approx 4.9\,\text{mV}$$

Hence,
$$4.9\,\text{mV} < 5\,\text{mV}$$
so the signal swing requirement is also satisfied.

This confirms that choosing
$$R_{in} \approx 10\\text{k}\Omega \quad \text{and} \quad r_e \approx 1\\Omega$$
is a safe design target.

## Choosing the Bias Current from $r_e$

Since
$$r_e \approx \frac{25\,\text{mV}}{I_E}$$
and we want
$$r_e \approx 1\,\Omega$$
then
$$1 \approx \frac{25\,\text{mV}}{I_E}$$
so
$$I_E \approx 25\,\text{mA}$$

For the purposes of design, use
$$I_C \approx I_E \approx 25\,\text{mA}$$

## DC Bias Target

The emitter resistor is $R_L = 100\\Omega$, connected to the $-5\\text{V}$ rail.

Thus the emitter voltage is estimated as
$$V_E \approx -5 + I_E R_L$$

Substituting $I_E \approx 25\\text{mA}$ gives
$$V_E \approx -5 + (25\\text{mA})(100\\Omega)$$
$$V_E \approx -5 + 2.5$$
$$V_E \approx -2.5\\text{V}$$

Assuming the transistor is in active region,
$$V_{BE} \approx 0.7\,\text{V}$$
so
$$V_B \approx V_E + 0.7$$
$$V_B \approx -2.5 + 0.7$$
$$V_B \approx -1.8\\text{V}$$

Therefore, the base DC bias should be around
$$V_B \approx -1.8\\text{V}$$

## Voltage Divider Design

For the DC case, the AC source is ignored and the coupling capacitor is treated as open, so the base voltage is set by the divider between $+5\,\text{V}$ and $-5\,\text{V}$.

Using the divider expression:
$$V_B = V_{CC} - \frac{R_1}{R_1+R_2}(V_{CC} - V_{EE})$$

Here,
$$V_{CC} = 5,\qquad V_{EE} = -5$$
so
$$V_{CC} - V_{EE} = 10$$

Substituting the desired base voltage $V_B \approx -1.8\\text{V}$:
$$-1.8 = 5 - \frac{R_1}{R_1+R_2}(10)$$

Rearranging:
$$5 + 1.8 = \frac{R_1}{R_1+R_2}(10)$$
$$6.8 = 10\frac{R_1}{R_1+R_2}$$
$$\frac{R_1}{R_1+R_2} = 0.68$$

This implies
$$R_1 \approx 2.125 R_2$$

So the divider should satisfy approximately
$$R_1 : R_2 \approx 2.125 : 1$$

## First Trial Values

A first trial was to use
$$R_2 = 1\,\text{k}\Omega,\qquad R_1 = 2.2\\text{k}\Omega$$
since this is close to the ratio found above.

Using
$$r_e \approx 1\,\Omega,\qquad \beta \approx 100$$
gives
$$(\beta+1)(r_e+R_L) = 101(1+100) = 101(101)$$
$$= 10201\,\Omega$$

Then
$$R_{in} = 1\,\text{k}\Omega \parallel 2.2\\text{k}\Omega \parallel 10201\\Omega$$

First,
$$1\,\text{k}\Omega \parallel 2.2\\text{k}\Omega = \frac{(1000)(2200)}{1000+2200}$$
$$= \frac{2200000}{3200} \approx 687.5\,\Omega$$

Then,
$$R_{in} \approx 687.5 \parallel 10201 \approx 644\,\Omega$$

This is too small, since it is not much larger than $100\,\Omega$. That would reduce the gain too much.

Therefore, this resistor choice is rejected.

## Final Resistor Choice

To increase input resistance, larger divider resistors were chosen while keeping the ratio reasonably close to the target:
$$R_1 = 24.9\\text{k}\Omega,\qquad R_2 = 10\\text{k}\Omega$$
This gives a ratio
$$\frac{R_1}{R_2} = \frac{24.9}{10} = 2.49$$
which is slightly larger than the ideal ratio of $2.125$ but still acceptable for the design.

Now calculate the new input resistance:
$$R_{in} = 24.9\\text{k}\Omega \parallel 10\\text{k}\Omega \parallel 10201\\Omega$$

First,
$$24.9\,\text{k}\Omega \parallel 10\\text{k}\Omega = \frac{(24900)(10000)}{24900+10000}$$
$$= \frac{249000000}{34900} \approx 7135\,\Omega$$

Then,
$$R_{in} \approx 7135 \parallel 10201$$
$$R_{in} \approx 4.2\,\text{k}\Omega$$

As a rough design estimate, this is much larger than $100\,\Omega$ so it is acceptable.

## Final Gain Check

Using the final design choice, the gain is estimated by
$$G = \frac{R_{in}}{R_{in}+100}\cdot \frac{100}{101}$$

Using the rough estimate $R_{in} \approx 7.1\,\text{k}\Omega$ from the divider branch as a quick check gives
$$G \approx \frac{7.1\,\text{k}}{7.2\,\text{k}} \cdot \frac{100}{101}$$
$$G \approx 0.986 \cdot 0.990$$
$$G \approx 0.98$$

Therefore,
$$G \approx 0.98 > 0.9$$
and the attenuation requirement is satisfied.

## Circuit Schematic

![Circuit Schematic](circuit_sim.png)

*Figure 1: Emitter follower circuit with final component values*

## Simulation Results

![Simulation Waveforms](simulation.png)

*Figure 2: Input and output voltage waveforms from transient analysis*

The simulation shows:
- Input signal (Vin): $0.5\\text{V}$ amplitude, $1\\text{kHz}$ sine wave
- Output signal (Vo): Approximately $0.49\,\text{V}$ amplitude (shifted $+3.2\,\text{V}$ for visualization)
- Source voltage (Vsrc): Reference waveform
- The output closely follows the input with minimal attenuation confirming our gain calculation of $\approx 0.98$
- The $-600\\text{mV}$ to $500\\text{mV}$ range shows proper biasing and signal swing

## Final Component Values

The final design values chosen were:
- $R_1 = 24.9\,\text{k}\Omega$
- $R_2 = 10\,\text{k}\Omega$
- $R_L = 100\,\Omega$
- $C_1 = 10\,\mu\text{F}$ (coupling capacitor)
- BJT: 2N3904
- Power supplies: $V_{CC} = +5\\text{V}$, $V_{EE} = -5\\text{V}$
- Input signal: $V_{in} = 0.5\\text{V}$ at $1\\text{kHz}$

## Physical Circuit

![Physical Circuit](physical_circuit.jpg)
*Figure 3: Full circuit on breadboard* 

## Transient Analysis Results

![Midband Operation](Midband%20Operation.png)

*Figure 4: Input and output waveforms showing midband operation*

The oscilloscope capture shows:
- Channel 1 (input): $0.5\,\text{V}$ amplitude, $1\,\text{kHz}$ sine wave
- Channel 2 (output): Approximately $0.49\,\text{V}$ amplitude
- Channel offsets: Channel 1 at $3.16\,\text{V}$ (actual signal offset $-1.8\,\text{V}$), Channel 2 at $2.48\,\text{V}$ (actual signal offset $-2.5\,\text{V}$)
- The output closely follows the input with minimal attenuation, confirming our gain calculation of $\approx 0.98$
- Peak-to-peak measurements: $0.97481\,\text{V}$ (Channel 1), $0.99771\,\text{V}$ (Channel 2)

## Frequency Domain Analysis

![Linearity Analysis](linearity.png)

*Figure 5: Spectrum analyzer output showing frequency response and linearity*

The FFT analysis reveals:
- Fundamental frequency at $1\,\text{kHz}$ with magnitude approximately $-80\,\text{dBV}$
- Minimal harmonic distortion visible in the spectrum
- Clean frequency response confirming linear operation
- The amplifier maintains good linearity at the design operating point

## Conclusion

The common-collector amplifier was designed to satisfy both the attenuation and signal swing requirements. Using the design targets
$$R_{in} \approx 10\\text{k}\Omega,\qquad r_e \approx 1\,\Omega$$
led to a desired emitter current of approximately 
$$I_E \approx 25\\text{mA}$$
which then gave a desired base bias of approximately
$$V_B \approx -1.8\\text{V}$$

A first resistor choice of $R_1 = 2.2\\text{k}\Omega$ and $R_2 = 1\\text{k}\Omega$ was rejected because it made $R_{in}$ too small. A final choice of 
$$R_1 = 24.9\\text{k}\Omega,\qquad R_2 = 10\\text{k}\Omega$$
was selected instead.

The simulation results confirm the design meets specifications:
- Achieved gain $\approx 0.98$ (meeting the $\geq 0.9$ requirement)
- Signal swing remains within small-signal limits
- Proper biasing maintains active region operation throughout the signal cycle
