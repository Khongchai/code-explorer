https://github.com/Ardour/ardour/blob/edc0e636e2d405e60e0d6a9dd2ed1af3a6790f15/libs/qm-dsp/dsp/phasevocoder/PhaseVocoder.cpp#L31C17-L31C24

fft is performed here to get the frequency-domain representation so that they can perform their phase vocoder algorithm on it.

[Ardour's phase vocoder use a slight variation of the fft](https://github.com/mborgerding/kissfft)
