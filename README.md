

# N-ary | An environment for playable sounds


<img align="left"
     width="300" 
     style="border-radius: 16px;"
     alt="N-ary" 
     src="https://github.com/user-attachments/assets/c3d93ad2-d051-45e1-9476-f1c2ca9b0455">
<br clear="left">

Welcome! This is **N-ary**, a web-based live coding environment focused on playable, real-time interaction. It combines code with manual event placement, allowing sounds and processes to be shaped while they are being performed.


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
  - [Delay Microlooping](#dual-delay-feedback-microlooping--phase-shifting)

- [Credits](#credits)
  
- [Licence](#licence)


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

# Code conventions

N-ary is not case-sensitive.

Use `//` to comment a line or everything that follows it.

Use `Ctrl + /` or `Ctrl + Ç` (*Spanish keyboard*) to comment or uncomment the current line or selected lines.

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
| Synths (Korg Monologue)  | `boat_note`, `buau_guagua`, `buau_noise`, `buau_vocoder`, `cof_bass`, `deep_bass`, `high_bass`, `high_pop`,` kick_bass`, `nintendo`, `pip`, `reso_piano`, `ring_mod_bass`, `supersaw`, `supersaw_dark`, `tv1`|
| Textures | `white`, `pink`, `vinyl`, `rain`, `conversation`, `birds`|
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

`delay` — adds delayed repetitions of the incoming signal.

**Parameters:**
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

When `dt` changes while delayed audio is still sounding, the delay line is stretched or compressed, producing a Doppler-like pitch shift.

**Example 3:** Smooth delay-time transitions  
```
track > source zen loop 6 
 delay 0.6 dt 100 200 500 dfb 0.8 dt_glide 0.8
```
`dt_glide 0.8` makes each transition between delay times take 0.8 seconds.
This smooths the Doppler-like pitch shift produced when `dt` changes.

**Exemple 4:** Combining and chaining delays  
Each delay is an independent effect instance with its own parameters. Multiple delays can be placed anywhere in the effect chain, and sequenced parameters can use different numbers of values. This allows complex effect chains to evolve at different rates within the same loop.
```
t > source piano loop 4 len 0.2
pitch 0 5 12 19 rotate 1
delay 1 dt 150 dfb 0.95
lpf 800
delay 1 dt 153 dfb 0.92
reverb .8 size .9 predelay 20
delay .4 dt 700 1100 900 dfb .55
```


## Reverb  

`reverb` — adds reverberation to the incoming signal.

**Parameters:**  

  - `reverb` — Dry/wet amount. (range=0-1, default=0
  - `size` — Controls the reverb decay time and perceived space size. (range=0-1, default=0.7)
  -  `predelay` — Delay between the dry sound and the start of the reverb, in milliseconds. (range=`≥0`, default=`0`)
  - `decay` — Damping amount. Reserved for a future algorithmic reverb implementation; currently has no effect.

**Implementation note:** N-ary currently uses `Tone.Reverb`, a convolution-based reverb. The reverb amount can be sequenced, while `decay` and `predelay` are treated as static parameters because changing them requires regenerating the impulse response. A future implementation may replace this with an algorithmic Dattorro reverb, allowing these parameters to be modulated continuously in real time.

**Example 1:** Basic reverb  
```
guit1 > source guitar loop 2 
 reverb 0.7 size .8 predelay 30
```

**Example 2:** Dry/wet sequencing  
The reverb amount can be sequenced, but changes are currently abrupt and are therefore not recommended for smooth transitions.
```
voice > source piano loop 8 
 reverb 0.1 0.8 1 size 0.8 predelay 30
```

**Example 3:** Stacked reverbs  
Reverb can also be chained with other effects, including other reverb instances:
```
track > source guitar loop 4 len 0.5
 pitch 0 24 5 12 rotate 1
 delay 0.7 dt 300 dfb 0.7 
 reverb 0.6 size 0.5 predelay 20 
 lpf 200 350 800 500
 reverb 0.85 size 0.9 predelay 60 
 delay 0.4 dt 1000 998 1001 dfb 0.65
```

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

## Dual-delay feedback microlooping / phase-shifting
**To be implemented.**
Technique based on two very short, slightly different feedback-delay loops running in parallel, creating evolving phase relationships and tape-like textures.  

 Refs:  
 https://www.youtube.com/watch?v=78wMNdnCBs8&list=LL&index=23&pp=iAQBsAgC  
 https://www.youtube.com/watch?v=uyzIqt-dUeY&list=LL  
 https://docs.vongon.com/polyphrase.pdf?utm_source=chatgpt.com  



# Credits
N-ary would not exist without the many tools, ideas and artists that inspired it.

Special thanks to the communities and creators behind **SuperCollider**, **Tidal Cycles**, **Strudel** and **Mercury**, whose work helped shape my understanding of live coding and musical systems.

I am also deeply indebted to artists such as Aphex Twin, Burial, Bogdan Raczynski, Vegyn, Four Tet, Loukeman, Brian Eno and Boards of Canada for years of inspiration.

Thank you.

# Licence
Nary is released under the GNU General Public License v3.0 (GPL-3.0).

This project is intended to remain free and open. You are welcome to use, modify and share it, but any derivative work must remain equally free and open under the same licence.











--------------------------------------------------------------------------




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



