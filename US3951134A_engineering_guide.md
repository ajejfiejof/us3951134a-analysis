## Engineering Build Guide: Remote Brain Wave Monitor (US3951134A)

Based on the patent disclosure, here's how the device would be engineered from component selection through signal chain implementation.

### 1. Frequency Generation (Transmit Path)

**Oscillator 30** — The master clock:
- Crystal-controlled oscillator at 100 MHz (or tunable via varactor)
- Can be pulse, square-wave, or sinusoidal output
- Feeds RF power divider 34

**RF Power Divider 34** — Three-way VHF/SHF divider:
- Identical signal at each output (37, 39, and intermediate terminal)
- Wilkinson or hybrid divider topology
- Must maintain phase coherence across outputs

**Frequency Doubler 36** — X2 multiplier:
- 100 MHz → 200 MHz
- Standard push-button or balanced mixer doubler

**10 MHz Oscillator 32** — Secondary reference:
- Crystal-controlled for stability
- Feeds mixer 40 alongside 200 MHz

**Mixer 40** — Sum/difference frequency generator:
- Inputs: 200 MHz + 10 MHz → 210 MHz (desired) + 190 MHz (discarded)
- VHF/UHF mixer IC (e.g., SA602, NE602 class)
- Bandpass filter selects 210 MHz for transmission

### 2. Antenna & Transmission

**Antenna Configuration**:
- Option A: Single multi-mode antenna (100 MHz + 210 MHz)
- Option B: Separate antennas 41, 43, 45
- Scanning capability: "reversing motor or electronically energizing elements"
- Scan angle/direction/rate controlled mechanically or electronically

**Transmit Power** — "Typical operating frequencies range from 1 MHz to 40 GHz"
- Must keep "transmitted energy below that which may be harmful"
- SAR analysis required for any real implementation

### 3. Receive Signal Chain

**Antenna 45** → receives modulated 110 MHz signal (brain re-transmitted interference)

**Circulator 38** — Critical isolation component:
- "Signal applied to input port emerges from output port with appropriate phase shift"
- Isolates transmitted signals from received signal
- Enables duplex operation on shared antenna

**Bandpass Filter 42** — Selects 110 MHz:
- Rejects harmonics and extraneous noise
- "Any conventional band pass filter"

**Mixer 44** — Downconversion:
- Inputs: 110 MHz (from filter) + 100 MHz (from source 30)
- Output: 10 MHz intermediate frequency (110 MHz - 100 MHz)
- "Conventional mixer" providing difference frequency

**IF Amplifier 46** — 10 MHz gain:
- Conventional amplifier topology

**Demodulator 48** — Extracts brain wave modulation:
- Type selected by desired brain wave characteristic:
  - Amplitude demodulation
  - Frequency demodulation  
  - Phase demodulation
- "Several different types of demodulators can be provided and used alternately or at same time"

**Audio Amplifiers 50a,b,c** → Display/Computer 60

### 4. Computer Processing (Modern Adaptation)

Patent's computer 60 would today be:
- Microcontroller (ARM, ESP32) or single-board computer (Raspberry Pi, BeagleBone)
- ADC for demodulated signal digitization
- DSP for Fourier analysis, filtering, display
- Memory for brain wave profile storage

**Key software functions**:
- Signal acquisition at 10 MHz IF
- Demodulation (AM/FM/PM as appropriate)
- Spectral analysis (FFT)
- Display on LCD/OLED
- Optional: compensating signal generation

### 5. Compensating Signal Path (Claims 7-11)

**Auxiliary Transmitter 64** — Sends compensating signals back to brain:
- "High frequency type commonly used in radar applications"
- Antenna 66 (can combine with other antennas)
- Transmitter 64 controlled by computer 60

**Closed-loop system**:
1. Measure brain wave activity → compare to standard signal (source 70)
2. Compute difference → generate compensating signal
3. Transmit to brain → "modulates natural brain wave activity toward reproduction of standard signal"
4. "Thereby changing the neurological response of the subject"

### 6. Engineering Constraints & Considerations

**Safety**:
- SAR (Specific Absorption Rate) analysis mandatory
- "Transmitted energy should be kept below that which may be harmful"
- FCC/regulatory compliance for ISM band (100/210 MHz ISMISM bands)

**Signal Fidelity**:
- Phase coherence between 100 MHz and 210 MHz critical
- Circulator/duplexer isolation must be adequate
- Demodulator type must match target brain wave characteristic

**Practical Modern Substitutions**:
- Crystal oscillators → DDS (Direct Digital Synthesis) for tunability
- Discrete mixers → RF integrated mixers (SA612, etc.)
- Circulator → RF duplexer (two-way, not three-way)
- Analog displays → digital LCD/LED with DSP processing
- Vacuum tube circuitry → solid-state equivalents

### 7. Bill of Materials (Estimated)

| Function | 1970s Component | Modern Equivalent |
|----------|-----------------|-------------------|
| Oscillator 30 | Crystal-controlled VFO | DDS module (AD9951, etc.) |
| RF Divider | Wilkinson divider | Hybrid coupler (Mini-Circuits) |
| Frequency Doubler | Balanced doubler | MMIC doubler (Qorvo, etc.) |
| Mixers | Discrete (RCA, etc.) | MMIC mixers (SA612, NE602) |
| Circulator | 3-port ferrite | RF duplexer (2-way) |
| Bandpass | L-C or cavity | Ceramic or crystal filter |
| Amplifier | Discrete transistor | MMIC amplifier (MAX2658, etc.) |
| Demodulator | Discrete + tuned | DSP software + ADC |
| Computer | Analog/digital hybrid | Microcontroller/RBC |
| Antenna | Copper wire/rod | PCB antenna or helical |

### 8. Verification Test Points

1. **Transmit**: 100 MHz + 210 MHz at antenna → verify power levels, no spurious
2. **Receive**: No signal → inject known 110 MHz → verify demodulated output
3. **Interference**: Measure 110 MHz re-transmission from "brain-equivalent" load
4. **Demodulation**: Apply test AM/FM/PM signal → verify correct extraction
5. **SAR**: Power level measurement at specified distance

### Conclusion

The patent discloses a complete, workable engineering architecture for remote brain wave monitoring using frequency-interference principles. A modern build would use:
- DDS/AD9951 for frequency generation
- MMIC mixers/amplifiers for signal chain
- DSP for demodulation and analysis
- RF duplexer (not circulator) for isolation
- Standard safety compliance (SAR, FCC)

The core principle—two frequencies interfering in tissue to produce difference-frequency modulation—is straightforward to implement with modern components, though safety and regulatory constraints would be significant hurdles for any real deployment.