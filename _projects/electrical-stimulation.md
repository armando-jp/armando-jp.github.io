---
title: "Electrical Stimulation V3"
excerpt_text: "A custom biomedical device dedicated to targeted electrical stimulation and research prototyping."
cover_image: "/assets/images/projects/electrical-stimulation/cover.jpg"
icon: "◈"
featured: true
order: 5
tags:
  - Biomedical
  - RTOS
  - PCB
  - Hardware
---

## Overview

A custom biomedical instrument built specifically for electrical stimulation research and prototyping. This project explores precision voltage generation and parastaltic pump control with precision hardware and threaded firmware design.

<div style="text-align: center;">
  <img src="/assets/images/projects/electrical-stimulation/e-stimv3-pics/01_top.jpg" alt="Electrical Stimulation" width="600">
</div>

## System Requirements
- Arbitrary waveform generation up to 1kHz
- +/- 1.65V voltage swing
- 10mV voltage resolution
- Parastaltic pump control
- User friendly control interface
- Display for visual feedback
- Buzzer for audio feedback
- Standalone operation
- Powered by USB-C

## History
My wife is pursuing a PhD in Biomedical Engineering and one of her research aims involves electrical stimulation of mice pancreas cells. The goal being to find the right parameters to stimulate the cells to produce insulin -- a task easier said than done.

The board shown at the top of the page (V3) is the latest incantation of the design. The original design dates back to when my wife was pursuing her masters degree. At the time her research also involved the design of a bioreactor which used gravity mediated fluidics to culture cells. At that time my wife inherited a pump control circuit which was a mess of wires and breakout boards connected to an Arduino Uno. It was functional, but far from ideal.

### Pre-V1
<div style="text-align: center;">
  <img src="/assets/images/projects/electrical-stimulation/e-stimv1-pics/origins.jpg" alt="Electrical Stimulation" width="600">
</div>

Look, the setup _technically_ works for driving the pump, but it was a mess to use and prone to errors in an environment where sterility and precision are of utmost importance. So we decided to design a custom board to handle the pump control and electrical stimulation. I present, e-stim v1.

<div style="text-align: center;">
  <img src="/assets/images/projects/electrical-stimulation/e-stimv1-pics/original.jpg" alt="Electrical Stimulation" width="600">
</div>

It features an OLED display for displaying diagnostics, an RGB LED for indicating status, a pump control circuit (look how small it is!) and a relatively simple stimulation circuit. This was great and all for the masters degree, but as the research progressed, the requirements evolved. 

### V1
Fast forward to the PhD, and the original stimulator circuit didn't quite meet the new requirements. The biggest one being the need for a much higher voltage resolution, bipolar operation, and a more arbitrary waveform generation capability. Our original design wasn't going to cut it, so we decided to design a new board.

<div style="text-align: center;">
  <img src="/assets/images/projects/electrical-stimulation/e-stimv1-pics/top.jpg" alt="Electrical Stimulation" width="600">
</div>

### V2
I present, e-stim v2.

<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/electrical-stimulation/e-stimv2-pics/' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="PCB Layer">
          <p class="layer-caption">{{ image.basename | replace: '-', ' ' }}</p>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<script>
  document.addEventListener('DOMContentLoaded', () => {
    const carousels = document.querySelectorAll('.carousel-wrapper');
    carousels.forEach(wrapper => {
      const carousel = wrapper.querySelector('.pcb-carousel');
      const prevBtn = wrapper.querySelector('.prev');
      const nextBtn = wrapper.querySelector('.next');
      
      const scrollAmount = () => {
        const item = carousel.querySelector('.carousel-item');
        // Get the width of one item + the gap (1.5rem = ~24px)
        return item.offsetWidth + 24; 
      };

      prevBtn.addEventListener('click', () => {
        carousel.scrollBy({ left: -scrollAmount(), behavior: 'smooth' });
      });

      nextBtn.addEventListener('click', () => {
        carousel.scrollBy({ left: scrollAmount(), behavior: 'smooth' });
      });
    });
  });
</script>

I won't spend too much time going into V2 because V3 will cover most of the same ground, but with some key improvements on analog front end and a new shielding can. 

## V3

<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/electrical-stimulation/e-stimv3-pics/' %}
        {% unless image.basename contains 'block' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="PCB Layer">
          <p class="layer-caption">{{ image.basename | replace: '-', ' ' | replace: '_', ' ' }}</p>
        </div>
        {% endunless %}
      {% endif %}
    {% endfor %}
  </div>
</div>


Here's a high level block diagram of V3:

<div style="text-align: center;">
  <img src="{{ '/assets/images/projects/electrical-stimulation/e-stimv3-pics/block.jpg' | relative_url }}" alt="Electrical Stimulation" width="600">
</div>

Unfortunately I can't share the detailed schematic or board layout for this project due to the sensitive nature of the research it's intended for, but I am able to talk about some specific decisions and challenges involved in the design of this board.

### Analog Front End
The analog front end is the part of the board that interfaces with the outside world. It's responsible for generating the electrical stimulation signals that interact will cells. In V1 and V2 designs, the analog front end suffered from excessive noise coupling from the on-board switching power supplies as well as from a 60 Hz noise which was picked up from the ambient environment due to poor shielding strategy.

Here's the LTSpice sim for the circuit used in V3

<div style="text-align: center;">
  <img src="{{ '/assets/images/projects/electrical-stimulation/e-stimv3-pics/ltspice_frontend.png' | relative_url }}" alt="Electrical Stimulation" width="600">
</div>

The circuit is composed of 3 stages: 
1. Bias stage: to level shift the 0V to 3.3V ouptut from the DAC to -1.65V and +1.65V.
2. Buffer stage: A buffer which corrects for the non-linearities of the push-pull stage's NPN/PNP pair.
3. The push-pull stage: for driving more current into the potential load. 

The bias stage I found in a data sheet for the DAC which was used in V1 and V2, the MCP4725.
<div style="text-align: center;">
  <img src="{{ '/assets/images/projects/electrical-stimulation/e-stimv3-pics/bias_circuit.png' | relative_url }}" alt="Electrical Stimulation" width="600">
</div>
It worked well for this application so didn't see any need to over-engineer it unless necessary.

The feedback and push-pull combo came after some sleuthing deep in engineering forums. The problem of adding the push pull right after the biasing circuit was that output would suffer from the crossover-distortion required to turn the BJTs on. This non-linearity would have killed us since we needed to have granual control on the mV level. 

By feeding the output of the push-pull stage back into the op-amp correction circuit, we can leverage negative feedback to force the output to match the input voltage, effectively compensating for the 0.7V diode drop. 

### Power Architecture
#### Input Power
There's something incredibly satisfying about being able to power and program a device over a single USB-C cable. Since V1, we've used a straight forward USB-C PD negotiator circuit which allows us to get negotiate for 5V up to 4A! 

The IC used is the CYPD3177. This IC requires 4 P-Channel MOSFETs (you could probbaly get away with 2 if you don't care about having a failsafe voltage supply when negotiation fails) and some caps and strap resistors. 

The IC is configured with straps to determine the voltage and current you'd like to negotiate for. It can negotiate for up to 20V and 5A. 

#### Front-End Power
Here's where we solved a lot of the problems in V1 and V2. 

## Software

## Results
Version 3 is where we finally started getting a clean output on the front end. Prior to this version, we were experiencing a multitude of signal integrity issues from different sources. The biggest offenders were:
1. Power supply switching noise (1-2 MHz range)
2. Ambient noise (60 Hz)

Visually, the improvements between generations is obvious. At an output of +/-100mV we see a removal of nearly almost 200mV of superimposed noise in v1 to v3.

<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/electrical-stimulation/waveforms/100mv/1us/' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="Waveform">
          <p class="layer-caption">{{ image.basename | replace: '-', ' ' | replace: '_', ' ' }}</p>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>


<script>
  document.addEventListener('DOMContentLoaded', () => {
    const carousels = document.querySelectorAll('.carousel-wrapper');
    carousels.forEach(wrapper => {
      const carousel = wrapper.querySelector('.pcb-carousel');
      const prevBtn = wrapper.querySelector('.prev');
      const nextBtn = wrapper.querySelector('.next');

      const scrollAmount = () => {
        const item = carousel.querySelector('.carousel-item');
        // Get the width of one item + the gap (1.5rem = ~24px)
        return item.offsetWidth + 24;
      };

      prevBtn.addEventListener('click', () => {
        carousel.scrollBy({ left: -scrollAmount(), behavior: 'smooth' });
      });

      nextBtn.addEventListener('click', () => {
        carousel.scrollBy({ left: scrollAmount(), behavior: 'smooth' });
      });
    });
  });
</script>

Then we can see between V2 and V3 at an output of +/-10mV even greater improvements.

<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/electrical-stimulation/waveforms/10mv/' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="Waveform">
          <p class="layer-caption">{{ image.basename | replace: '-', ' ' | replace: '_', ' ' }}</p>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

Here's about the current limit of V3. At +/-1mV output, we start to see something closer to +/-2mV, with some high frequency noise as well as overshoot during the transition from the negative to positive maximums. 
<div style="text-align: center;">
  <img src="{{ '/assets/images/projects/electrical-stimulation/waveforms/1mv/estim3_1us_1mv0.png' | relative_url }}" alt="Waveform 1mV">
</div>