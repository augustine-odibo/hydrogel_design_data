# Protocol: Measuring the Attenuation Coefficient of Hydrogels

_As of October 27, 2026_

## 1. Purpose and principle

This protocol measures the ultrasonic attenuation coefficient α of a hydrogel, in dB/cm and dB/cm/MHz, by pulse-echo (Method A) or substitution (Method B). An optional optical method is in section 7.

A wave's amplitude decays exponentially as it travels through the gel:

$$A(x) = A_0 \, e^{-\alpha x}$$

So for a path length d through the gel:

$$\alpha_{\mathrm{dB/cm}} = \frac{20}{d}\,\log_{10}\!\left(\frac{A_0}{A}\right) = 8.686 \, \alpha_{\mathrm{Np/cm}}$$

Attenuation combines absorption and scattering. In hydrogels it depends mainly on polymer concentration, crosslink density, porosity, temperature and frequency.

## 2. Materials and equipment

| Item | Specification / purpose |
| --- | --- |
| Ultrasonic pulser-receiver | Broadband pulse; adjustable gain and damping |
| Transducer(s) | Immersion, 1–10 MHz centre frequency; two matched units for Method B |
| Digital oscilloscope or DAQ | ≥ 100 MS/s sampling (≥ 10× the transducer frequency); waveform export |
| Water tank | Degassed, deionised water; transducer holder and sample stage |
| Reflector plate | Polished steel or glass, flat (Method A) |
| Thermometer | ±0.1 °C; record water and sample temperature |
| Calipers or micrometer | ±0.01 mm, for sample thickness |
| Moulds | Flat-faced, at least 3 thicknesses (e.g. 3, 5, 8 mm), diameter > 2× transducer face |
| Vacuum desiccator or sonicator | Degassing the precursor solution |
| Balance | ±0.1 mg, for density (ρ) and swelling |
| Hydrogel precursors | Polymer, crosslinker, initiator; record lot numbers |
| Software | Python/MATLAB for FFT and fitting |

## 3. Sample preparation

Bubbles and uneven thickness are the main sources of error, so degas every precursor and cast flat, parallel faces.

1. Prepare the precursor at the target concentration (% w/v). Record the polymer, crosslinker and initiator amounts.
2. Degas the precursor for 10–15 min under vacuum or in a sonicator until no bubbles rise.
3. Pour slowly down the mould wall to avoid trapping air. Cast at least three thicknesses per formulation, n ≥ 3 samples each.
4. Cure by your standard method (UV, thermal, ionic). Record the time, light intensity or temperature.
5. Equilibrate in degassed water or buffer for 24 h, or until mass is stable, at the measurement temperature.
6. Measure thickness d at 5 points per sample, without compressing the gel, and record the mean ± SD. Reject samples whose faces differ by more than 2%.
7. Measure density ρ (mass ÷ volume, or by buoyancy), which you need for the acoustic impedance.
8. Keep samples hydrated and measure them within the same day.

## 4. Method A: pulse-echo

One transducer sends a pulse and receives the echoes from the gel's top surface and from the reflector beneath it.

1. Fill the tank with degassed water and let it reach a stable temperature (e.g. 25.0 ± 0.2 °C).
2. Mount the transducer facing down, normal to the reflector plate. Position it so the gel sits in the far field, beyond the transducer's near-field distance N = D²/(4λ).
3. **Reference:** with no sample, record the echo from the reflector, A_ref. Tilt the transducer until this echo is at its maximum.
4. **Sample:** place the gel flat on the reflector without trapping air. Do not change the gain, the damping or the transducer height.
5. Record two echoes:
    - A₁, from the water–gel top surface
    - A₂, from the gel–reflector interface, after crossing the gel twice
6. Average at least 64 waveforms per acquisition. Save the full time-domain signals, not just the peak values.
7. Measure the time between the two echoes, Δt. The sound speed is c = 2d/Δt.
8. Repeat at 3 positions per sample and on every thickness.
9. Record the water temperature at the start and end of each run.

## 5. Method B: substitution (through-transmission)

Two coaxial transducers face each other in water. You compare the signal with and without the gel in the beam path.

1. Align the transmitter and receiver coaxially, with the receiver in the transmitter's far field. Adjust the alignment until the received signal is at its maximum.
2. **Reference:** record the signal through water only, A_ref(t).
3. **Sample:** insert the gel perpendicular to the beam, centred, in a holder that leaves both faces in water. Keep all settings unchanged.
4. Record A_s(t) and the arrival-time shift Δt relative to the reference.
5. Compute the sound speed in the gel from the time shift:

$$c_s = \frac{d}{\,d/c_w - \Delta t\,}$$

Here c_w is the speed of sound in water at the measured temperature, and Δt > 0 means the gel signal arrives earlier.

6. Average at least 64 waveforms. Repeat at 3 positions and on every thickness.
7. Re-record the water reference at the end to check for drift. Repeat the run if the two references differ by more than 2%.

## 6. Data analysis

Report α in dB/cm at each frequency, and fit α₀ (dB/cm/MHz) and n over the transducer's −6 dB bandwidth.

### 6.1 Acoustic impedance and interface coefficients

For each material, Z = ρ·c. With water (w), gel (s) and reflector (r):

$$R_{ws} = \frac{Z_s - Z_w}{Z_s + Z_w}, \qquad T_{ws}T_{sw} = 1 - R_{ws}^2, \qquad R_{sr} = \frac{Z_r - Z_s}{Z_r + Z_s}$$

Hydrogels have Z close to water's, so R_ws is small and the top-surface echo A₁ is weak. That makes the reflector-referenced form below more reliable than using A₁/A₂.

### 6.2 Method A (pulse-echo)

Use the reflector echo without the sample (A_ref) and the back-wall echo with the sample (A₂):

$$\alpha_s = \frac{1}{2d}\,\ln\!\left(\frac{A_{\mathrm{ref}}}{A_2}\cdot\frac{(1-R_{ws}^2)\,R_{sr}}{R_{wr}}\right) + \alpha_w \quad [\mathrm{Np/cm}]$$

The α_w term accounts for the water path the gel replaces; it is negligible below about 5 MHz. Multiply by 8.686 for dB/cm.

### 6.3 Method B (substitution)

$$\alpha_s = \alpha_w + \frac{1}{d}\,\ln\!\left(\frac{A_{\mathrm{ref}}}{A_s}\cdot T_{ws}T_{sw}\right) \quad [\mathrm{Np/cm}]$$

Water attenuation is about 0.0022 · f² dB/cm at 20 °C, with f in MHz.

### 6.4 Multi-thickness check (recommended)

Plot ln(A₂ or A_s) against path length (2d for Method A, d for Method B) across your three thicknesses. The slope is −α, and the interface losses fall into the intercept. This removes the need for exact R and T values, so use it as the primary result when the plot is linear (R² > 0.98).

### 6.5 Frequency-dependent attenuation

1. Gate each echo with a Hann or Tukey window of the same length.
2. Take the FFT and get the magnitude spectra |S_ref(f)| and |S_s(f)|.
3. Apply the equations above at each frequency inside the −6 dB bandwidth.
4. Fit α(f) = α₀ · fⁿ by linear regression of log α against log f.
5. For focused or small transducers, apply a diffraction correction, or use the multi-thickness method, which largely cancels it.

```python
import numpy as np

def spectrum(sig, fs, t0, t1):
    seg = sig[int(t0*fs):int(t1*fs)] * np.hanning(int(t1*fs) - int(t0*fs))
    f = np.fft.rfftfreq(len(seg), 1/fs)
    return f, np.abs(np.fft.rfft(seg))

# Method B, Np/cm -> dB/cm; d in cm, fs in Hz
f, S_ref = spectrum(ref, fs, t0_ref, t1_ref)
_, S_s = spectrum(samp, fs, t0_s, t1_s)
f_MHz = f / 1e6
alpha_w = 0.0022 * f_MHz**2                       # dB/cm
alpha = 8.686 / d * np.log((S_ref / S_s) * TwsTsw) + alpha_w

band = (f_MHz > f_lo) & (f_MHz < f_hi)            # -6 dB bandwidth
n, log_a0 = np.polyfit(np.log(f_MHz[band]), np.log(alpha[band]), 1)
alpha0 = np.exp(log_a0)                           # dB/cm/MHz^n
```

## 7. Optional: optical attenuation by UV-vis

Use this method when you need light penetration, for example to judge photocuring depth or transparency.

1. Cast the gel directly in a cuvette, or cut slabs of known path length d (1–10 mm).
2. Blank the spectrophotometer with a cuvette of water or buffer.
3. Record the absorbance A from 300 to 800 nm, or at your curing wavelength (e.g. 365 or 405 nm).
4. Calculate the attenuation coefficient:

$$\mu = \frac{\ln(I_0/I)}{d} = \frac{2.303\,A}{d} \quad [\mathrm{cm^{-1}}]$$

5. The penetration depth is 1/μ.

In turbid gels the result combines absorption and scattering, so report it as attenuation, not absorption. Use at least two path lengths to confirm A rises linearly with d.

## 8. Quality control and reporting

A result is valid only if the water references are stable, the samples are bubble-free and the multi-thickness fit is linear.

- [ ] Validate the setup on a reference material of known α (e.g. a commercial tissue-mimicking phantom, or a 3% agar gel) before testing new gels.
- [ ] Water temperature stayed within ±0.2 °C during each run.
- [ ] Start and end water references agree within 2%.
- [ ] No visible bubbles or delamination; any sample with a bubble in the beam path is discarded.
- [ ] Signals neither clip nor sit in the noise floor (SNR > 20 dB).
- [ ] At least 3 samples × 3 positions per formulation; report mean ± SD.
- [ ] Multi-thickness fit R² > 0.98.

Report for each formulation: polymer and concentration (% w/v), crosslinker ratio, cure conditions, swelling state, temperature, transducer centre frequency and bandwidth, method (A or B), d, ρ, c, α at the centre frequency (dB/cm), α₀ (dB/cm/MHz) and n.

**Data sheet (one row per sample position)**

| Sample ID | Formulation | Thickness d (mm) | Density ρ (g/cm³) | Temp (°C) | Method | Sound speed c (m/s) | α at f_c (dB/cm) | α₀ (dB/cm/MHz) | n | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
