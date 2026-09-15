

# N-ary | An environment for playable sounds

<img width="300" height="300" alt="Bo_2 1" src="https://github.com/user-attachments/assets/c3d93ad2-d051-45e1-9476-f1c2ca9b0455" />

<!--
TO DO WITH SHORT EXPLANATION OF THE FILOSOPHY
Isard is the Catalan name for the Pyrenean chamois, whose agility, balance and ability to navigate rugged terrain mirror the qualities of live coding and inspired the name of this project.

<img width="500" height="375" alt="image" src="https://github.com/user-attachments/assets/c289d271-740c-4160-a5eb-fd23cb2fffe2" /> (make a pixel art version)

Isard is a web-based environment where sounds are organized into playable structures and performed in real time using a minimal language.
-->

## Contents

- [Getting Started](#getting-started)
  
- [Loading Samples](#loading-samples)
  - [Preloaded Samples](#preloaded-samples)
  - [Import Your Own Samples](#import-your-own-samples)
    
- [Sample Controls](#sample-controls)
  - [Pitch](#pitch)
  - [Gain](#gain)
  - [Length](#length)
 
- [Functions](#functions)
  - [Sequencing](#sequencing)
  - [Rotate](#rotate)
  
- [Effects](#effects)
  - [Low-pass Filter](#low-pass-filter)
  - [Delay](#delay)
  - [Reverb](#reverb)

- [Techniques](#techniques)
  - [Doppler Pitch Shifting](#doppler-pitch-shifting)
  - [Microlooping](#microlooping)

- [Credits](#credits)
  
- [License](#license)


# Getting started

Use N-ary directly in your browser:

https://nary.yagoraymond.cat/

- **`Ctrl + Enter`** — execute
- **`Ctrl + .`** — stop

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
| Family | Name of the sample |
| --- | :--- |
| Percussion<sup>1</sup> | `kick`(x5), `snare`(x4), `hat`(x2), `clap`, `shaker`(x2) |
| Drum Loops | `boombap_20s`<sup>2</sup> |
| Instruments | `guitar`, `violin`, `piano`<sup>3</sup>|
| Vocals | `choir`, `uhhh` |
| Synths  | TBD |
| Textures | `pink`, `vinyl`, `rain`, `conversation`, `birds`|
| Pads | `field_c`, `city_c`, `zen`, `buzz` |

 **(xN) indicates that there are N variants of this sample. The first variant has no number suffix (e.g. `kick`, `kick2`, `kick3`, ...).*

**Third-party sample credits and usage terms:**

<sub>
<sup>1</sup> 99Sounds — drum samples, royalty-free for commercial and non-commercial use: https://99sounds.org/  <br>
<sup>2</sup> holizna / Freesound — drum loop, CC0: https://freesound.org/people/holizna/sounds/629139/  <br>
<sup>3</sup> University of Iowa Musical Instrument Samples — piano sample, free to use without restrictions: https://theremin.music.uiowa.edu/mispiano.html
</sub>

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

## Gain
`gain` — Controls the amplitude of the track. (default=1)
```
drums > source kick loop 2
 gain 0.5
```
Like other track parameters, gain can be sequenced:
```
drums > source guitar loop 4
 gain 1 0.5 0.2 0.8
```

## Length
`len` — Sets the duration of each event in seconds.
```
voice > source uhhh
  len 0.5
```


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

## Rotate
`rotate` — Circularly shifts the values of the immediately preceding sequenced parameter by a fixed number of positions after each completed loop.

Positive values rotate forward, negative values rotate backward.

```
guitar > source guitar loop 4
pitch 0 3 7 12 rotate 1
```
This produces the following loops in pitch:

  > Loop 1: | 0  | 3  | 7  | 12 |  
    Loop 2: | 3  | 7  | 12 | 0  |  
    Loop 3: | 7  | 12 | 0  | 3  |  
    Loop 4: | 12 | 0  | 3  | 7  |  
    Loop 5 = Loop 1  

`rotate` applies only to the parameter immediately preceding it. Different sequenced parameters can therefore rotate independently.

```
guitar > source guitar loop 4
pitch 0 3 7 12 rotate 1
reverb 0 0.2 0.5 1 rotate -1
```
Here, `pitch` rotates forward by one position per loop, while `reverb` rotates backward by one position per loop.

Negative values rotate in the opposite direction:

```
guitar > source guitar loop 4
pitch 0 3 7 12 rotate -1
```

# Effects
Effects in N-ary behave like pedals in a physical effects chain.

Each track has its own ordered effect chain. This implies:

1. Effects can be freely chained, including multiple instances of the same effect.
2. Each effect instance remains independent and has its own parameters.
3. Effects process all audio passing through the track, including sounds that were triggered before an effect parameter changed.
4. Effects are connected in the same order in which they are written, so order matters. `Delay 1 → Reverb → Delay 2` is not equivalent to `Delay 1 → Delay 2 → Reverb`.


## Low-pass Filter
`lpf` — Low-pass filter cutoff frequency in Hz. Frequencies above the cutoff are progressively attenuated. If no `lpf` is added, the signal is not filtered.

**Parameters:**
- `lpf` — Cutoff frequency in Hz. (range=`20–22050`)
- `lpf_glide` —  Time in seconds used to smoothly transition between lpf values. (default=`0.03`)

**Example 1:** Fixed cutoff
```
guitar > source guitar loop 4 lpf 800
```
The cutoff remains at `800 Hz` throughout the loop.

**Example 2:** Sequenced cutoff
```
track > source choir loop 6 len 6 lpf 200 800 9000
```
The loop is divided into three equal regions, so the cutoff changes every 2 seconds: `200 → 800 → 9000 Hz`.

Because the sample is still playing when the cutoff changes (region change), each new cutoff value changes the sound of that same ongoing sample.

**Example 3:** Smooth transitions
```
track > source choir loop 6 len 6 lpf 200 800 9000 lpf_glide 0.8
```
`lpf_glide 0.8` makes each cutoff transition take 0.8 seconds instead of changing almost immediately.

**Exemple 4:** Sequenced glide
```
track > source choir loop 5 len 5 
lpf 300 9000 300 9000 300 9000 300 9000 300 9000 300 9000 300 9000 300 9000 300 9000 300 9000
lpf_glide 0.05 0.3 3
```
`lpf_glide` can also be modulated. This allows the speed of the cutoff transitions to evolve independently across the loop.

As with other sequenced parameters, functions such as rotate can also be applied to lpf and lpf_glide (*e.g.* `lpf_glide 0.05 0.3 3 rotate 1`)

## Delay
Each track has its own delay effect.

Parameters:

- `delay` — Amount of delayed signal (dry/wet). (range=`0-1`, default=`0`)
- `dt` — Delay time in milliseconds. (default=`400`)
- `dfb` — Feedback amount. Higher values produce more repetitions. (range=`0-1`, default=`0.5`)
- `dt_glide` — Time in seconds used to smoothly transition between `dt` values. (default=`0.3`)

**Example 1:** Basic delay
```
guitar > source guitar loop 4 delay 0.6 dt 250 dfb 0.7
```
This adds a 250 ms delay with 0.7 feedback, mixed at 0.6 with the dry signal.

**Example 2:** Sequenced delay time
```
track > source zen loop 6 
 delay 0.6 dt 100 200 500 dfb 0.8
```
The loop is divided into three equal regions, so the delay time changes every 2 seconds: 100 → 200 → 500 ms.

When dt changes while delayed audio is still sounding, the delay line is stretched or compressed, producing a Doppler-like pitch shift.

**Example 3:** Smooth delay-time transitions
```
track > source zen loop 6 
 delay 0.6 dt 100 200 500 dfb 0.8 dt_glide 0.8
```
`dt_glide 0.8` makes each transition between delay times take 0.8 seconds.
This smooths the Doppler-like pitch shift produced when `dt` changes.



TO DO:
Exemple 4: combinar sequenciacio de tots els parametres amb n diferents de valors i rotate
Exemple 5: encadenar varios delays
Exemple 6: aconseguir microtuning https://www.youtube.com/watch?v=78wMNdnCBs8&list=LL&index=22

```
decay 10
predelay 300

drums > source kick loop 2
 reverb 0.4
voice > source choir loop 10
 len 2
 reverb 0.8
```

## Reverb
All tracks share the same reverb space (bus).

Global parameters:
  - `decay` — Length of the reverb tail in seconds. (default: `4`)
  - `predelay` — Delay between the dry sound and the start of the reverb in milliseconds. (range=0-300, default=0)

Track parameter: 
  - `reverb` — Amount of signal sent to the reverb bus (dry/wet). (range=0-1, default=0)

# Techniques
## Doppler Pitch Shifting
Changing `dt` while delayed audio is sounding produces a Doppler-like pitch shift. Decreasing `dt` shifts the sound upwards; increasing it shifts the sound downwards.

The resulting interval **depends on the speed of the delay-time change** , not on the original note. The same transition can therefore shift different source notes by approximately the same musical interval.

To calculate the required change in delay time:
```math
\Delta dt = 1000 \cdot dt_{glide} \left(1 - 2^{n/12}\right)
```
where:

- $\Delta dt = dt_{new} - dt_{old}$, in milliseconds.
- $dt_{glide}$ is the transition time in seconds.
- $n$ is the desired interval in semitones. Positive values shift upwards; negative values shift downwards.

The following table summarises approximate `dt` changes ($\Delta dt$) required to obtain common musical intervals for different `dt_glide` values:

| Interval | `dt_glide 0.1` | `dt_glide 0.2` | `dt_glide 0.4` | `dt_glide 0.8` |
| --- | ---: | ---: | ---: | ---: |
| +12 st · octave | `−100 ms` | `−200 ms` | `−400 ms` | `−800 ms` |
| +7 st · perfect fifth | `−50 ms` | `−100 ms` | `−199 ms` | `−399 ms` |
| +5 st · perfect fourth | `−33 ms` | `−67 ms` | `−134 ms` | `−268 ms` |
| +4 st · major third | `−26 ms` | `−52 ms` | `−104 ms` | `−208 ms` |
| +3 st · minor third | `−19 ms` | `−38 ms` | `−76 ms` | `−151 ms` |
| −3 st · minor third | `+16 ms` | `+32 ms` | `+64 ms` | `+127 ms` |
| −4 st · major third | `+21 ms` | `+41 ms` | `+83 ms` | `+165 ms` |
| −5 st · perfect fourth | `+25 ms` | `+50 ms` | `+100 ms` | `+201 ms` |
| −7 st · perfect fifth | `+33 ms` | `+67 ms` | `+133 ms` | `+266 ms` |
| −12 st · octave | `+50 ms` | `+100 ms` | `+200 ms` | `+400 ms` |


For example, with `dt_glide 0.4`, decreasing from `dt 400` to `dt 200` (−200 ms) produces approximately a perfect fifth upwards. Then, increasing from `dt 200` to `dt 333` (+133 ms) produces approximately a perfect fifth downwards. 
```
track2 > source zen loop 3 len 2
delay 0.8 dt 400 200 333 dt_glide 0.4
```
⚠️ Each interval is determined by the change from one `dt` value to the next, not by their distance from the first value in the sequence.

```
drum > source snare2 loop 4
 delay 0 0.3 0.6 1
 dt 100 200 400 800
 dfb 0.2 0.4 0.6 0.8
```

## Microlooping


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




Functions to be reviewed (past first version, maybe recovered maybe deleted)
````
## Functions & Attributes

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


## Examples
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



