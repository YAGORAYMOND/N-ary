

# N-ary/Ordit/Ordal | An environment for playable sounds

N-ary is a web-based environment where sounds are organized into playable structures and performed in real time using a minimal language.

--------------------------------------------
# Getting started:

## Installation/start/stop
1. Download the `N-ary.html` file.
2. Open it in any web browser.
4. Press **`Ctrl + Enter`** to execute.
5. Press **`Ctrl + .`** to stop.

## Basic structure
Each track is defined as follows:
> *track_name* `> source` *sample_name* `loop` *loop_duration (sec)*
When executing, a **loop bar** appears.
In this bar, there is a yellow **timeline**




## 

# Samples:
## Import samples:
**Way 1**: Multiple samples from a same repo:
```ruby
repo "https://raw.githubusercontent.com/YAGORAYMOND/samples/main"
import fatbass_1 "perc/fatbass/fatbass_1.wav"
import ah_E3 "ah/ah_E3.wav"
```
**Way 2**: Multiple samples from different repos:
```ruby
import fatbass_1 "https://raw.githubusercontent.com/YAGORAYMOND/samples/refs/heads/main/perc/fatbass/fatbass_1.wav"
import ah_E3 "https://raw.githubusercontent.com/YAGORAYMOND/samples/main/ah/ah_E3.wav"
```

## Default samples:
TBD

# Sample controls:

# Effects:















## 🔊 Getting Started

1. Download the `N-ary.html` file and open it in any web browser.
2. Type your code in the editor.
3. Press **`Ctrl + Enter`** to **render** or update the sound.
4. Press **`Ctrl + .`** to **stop** the engine.
5. ENJOY!

---

## 📄 Language Syntax

The language is based on **Fluxes**. A flux is defined by a name followed by the `>` symbol and a list of indented attributes.
```n-ary
cloud_name > source triangle
  rate 10
  grain 500ms

````
## 🎛️ Functions & Attributes

| Attribute | Description | Recommended Values |
| ---- | --- | --- |
| **source** | The base waveform of the generator. | `sine`, `triangle`, `sawtooth`, `square` |
| **rate** | How many grains are generated per second (speed). | `1` to `60` |
| **grain** | The duration of each individual sound fragment. | `10ms` to `5000ms` |
| **jitter** | Temporal chaos. Randomly offsets the start of each grain. | `0` to `0.5` |
| **pitch** | Transposition of the base note (in semitones). | `-24` to `24` |
| **fine_detune** | Fine pitch adjustment to create "chorus" or thickness. | `0` to `50` |
| **reverb** | Spatial depth and filter darkness. | `0` (dry) to `1.0` (infinite) |
| **activity** | Probability of a grain being triggered. | `0` to `1.0` |

 
## 🤓⌨️ Examples
1. Atmospheric Cloud (Ambient Pad)
  A soft, deep texture that evolves slowly.
```
atmosphere > source sine
  rate 12
  grain 1500ms
  jitter 0.1
  pitch -12
  reverb 0.95
  fine_detune 15
```
2. Electric Sparks (Glitchy)
  Short, high-pitched sounds with random rhythmic cuts.
```
sparks > source triangle
  rate 4
  activity 0.3
  grain 40ms
  pitch 24
  reverb 0.2
```

3. Deep Detuned Bass
  A solid sound with organic movement.
```
bass > source sawtooth
  rate 20
  grain 800ms
  pitch -2
  fine_detune 30
  reverb 0.4
```
## 📜 License
Project created for sonic experimentation. Free to use and modify.



