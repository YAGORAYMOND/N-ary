

# N-ary/Ordit/Ordal | An environment for playable sounds
N-ary is a web-based environment where sounds are organized into playable structures and performed in real time using a minimal language.

## Contents

- [Getting Started](#getting-started)
- [Loading Samples](#loading-samples)
  - [Preloaded Samples](#preloaded-samples)
  - [Import Your Own Samples](#import-your-own-samples)
- [Sample Controls](#sample-controls)
  - [Pitch](#pitch)
  - [Pitch(n)](#pitchn-tbd)
  - [Length](#length)
- [Global Effects](#global-effects)
  - [Reverb](#reverb)
- [Local Effects](#local-effects)
  - [Low-pass Filter](#low-pass-filter)
  - [Delay](#delay)
- [Sequencing](#sequencing)
- [Credits](#credits)
- [License](#license)


# Getting started:

## Installation/start/stop
1. Download the `N-ary.html` file.
2. Open it in any web browser.
4. Press **`Ctrl + Enter`** to execute.
5. Press **`Ctrl + .`** to stop.

## Basic structure
Each track is defined as follows:
> *track_name* `> source` *sample_name* `loop` *loop_duration (sec)*

Example:

<img width="168" height="24" alt="image" src="https://github.com/user-attachments/assets/cb269241-1f62-46b6-93e8-b014a0ef3710" />

When executing, a **loop bar** appears for each track.

The moving yellow line shows the current position in the loop.

Click on the loop bar to select it, then use:
  - `p` — Play the sample once
  - `w` — Write an event on the current position
  - `e` — Erase event
  - `delete` — Clear all events

Next, let's load some samples.

# Loading samples
There are two ways to use samples:

## Preloaded samples
Use any of the samples already included in the project:
| Fammily | Name of the sample |
| --- | :--- |
| Percussion<sup>1</sup> | `kick`(x5), `snare`(x4), `hat`(x2), `clap`, `shaker`(x2) |
| Instruments | `guitar`, `violin`, `piano`<sup>2</sup>|
| Vocals | `choir`, `uhhh` |
| Synths  | TBD |
| Textures | `pink`, `vinyl`, `rain`, `conversation`, `birds`|
| Pads | `field_c`, `city_c` |

 *<sup>1</sup>These drum samples were sourced from 99Sounds: https://99sounds.org/*
 
 *<sup>2</sup>This piano sample was sourced from: https://theremin.music.uiowa.edu/mispiano.html*
 
 **(xN) indicates that there are N variants of this sample. The first variant has no number suffix (e.g. `kick`, `kick2`, `kick3`, ...).*

## Import your own samples
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



# Sample controls
## Pitch
`pitch` — Transposes the source in 12-tone equal temperament steps.
Then, `pitch 12` raises the source by one octave.
```
inst > guitar len 4
 pitch 0 3 7 12
```
<!--
## Pitch(n) (TBD)
`pitch(n)` — Transposes the source in n equal divisions of the octave.
`pitch` is equivalent to `pitch(12)`.
This allows alternative tuning systems and microtonal music.
```
inst > source guitar
  pitch(5) 0 1 2 4 5
```
-->

## Length
`len` — Sets the duration of each event in seconds.
```
voice > source uhhh
  len 0.5
```

# Global Effects
Global effects use shared buses. This means that all tracks can send signal to the same effect instance.
## Reverb
All tracks share the same reverb space (bus).

Global parameters:
  - `decay` — Length of the reverb tail in seconds. (default: `4`)
  - `predelay` — Delay between the dry sound and the start of the reverb in milliseconds. (range=0-300, default=0)

Track parameter: 
  - `reverb` — Amount of signal sent to the reverb bus (dry/wet). (range=0-1, default=0)

```
decay 10
predelay 300

drums > source kick loop 2
 reverb 0.4
voice > source choir loop 10
 len 2
 reverb 0.8
```

# Local Effects
Effects individuallyl applied to each track.

## Low-pass Filter
`lpf` — Low-pass filter cutoff frequency in Hz. By default no filter is applied.

```
guitar > source guitar loop 2
guitar_lpf > source guitar loop 2 
 lpf 200
```


## Delay


# Functions
## Sequencing

When multiple values are provided for a parameter, the loop is divided into equal regions.

Events inherit the value of the region in which they occur.

> pitch 0 3 7 12

creates 4 equal regions:

> | 0 | 3 | 7 | 12 |

The same applies to any parameter:

```
guitar > source guitar loop 4
pitch 0 3 7 12
lpf 500 1000 4000 12000
reverb 0 0.2 0.5 1
```

Rather than defining sequences of events, N-ary defines regions in time. Multiple parameter regions combine to create sonic territories across the loop timeline.

<!--
## Glide(time) TBD
Idea: delay 0.8
      dt 20 60 glide(50) (for now all in ms)
-->


# Credits
N-ary would not exist without the many tools, ideas and artists that inspired it.

Special thanks to the communities and creators behind **SuperCollider**, **Tidal Cycles**, **Strudel** and **Mercury**, whose work helped shape my understanding of live coding and musical systems.

I am also deeply indebted to artists such as Aphex Twin, Burial, Bogdan Raczynski, Vegyn, Four Tet, Loukeman, Brian Eno and Boards of Canada for years of inspiration.

Thank you.

# Licence
Nary is released under the GNU General Public License v3.0 (GPL-3.0).

This project is intended to remain free and open. You are welcome to use, modify and share it, but any derivative work must remain equally free and open under the same license.

<!--
TO DO:
  - Possibilitar que un mateix track pugui tenir varios delays o varios reverbs, reenfocar la visió dels efectes com a pedals que s'encadenen al track i no com a parámetres del track
  - Crear la variable send i el concepte de bus. Per poder der track1 > (...) send 0.5 all i despres definir all > reverb etc etc.
-->
--------------------------------------------------------------------------


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



