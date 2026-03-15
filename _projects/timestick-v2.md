---
title: "Time Stick V2"
excerpt_text: "Funsize PTP and 2.5G Ethernet."
cover_image: "/assets/images/projects/timestick-v2/cover.png"
icon: "◈"
featured: true
order: 3
tags:
  - Timing
  - Embedded
  - PCB
---
I've put these boards up for sale on Tindie! <br> You can get one here: [Time Stick V2 on Tindie](https://www.tindie.com/products/timeappliances/time-stick-v2/)

## Overview

There are many USB to ethernet adapters out there, but not all of them are designed with precition timing in mind. 

<div style="text-align: center;">
  <img src="/assets/images/projects/timestick-v2/cover.png" alt="Time Stick V2" width="600">
</div>


This project uses the AX88279 chipset from ASIX which has built in support for IEEE 1588 PTP (Precision Time Protocol) and 2.5G ethernet. This allows the adapter to syntonize (frequency align) and synchronize (phase/time align) its internal PHC (Physical Hardware Clock) with a PTP master clock on the network.

Where this project differs even further from other PTP adapters is the inclusion of a 1PPS (one pulse per second) input or output via SMA. This allows the adapter to be used as a reference clock for other devices that need a precise 1PPS signal. Not only that, but there's also a connector for interfacing with a GNSS module to get TOD (Time of Day) information and PPS signals.

## Architecture
The architecture is straight forward, the AX88279 is connected to a USB 3.2 Gen 1 interface via a USB-A connector. The AX88279 also has an integrated 2.5G ethernet MAC and PHY, which is then connected to a RJ45 connector. 

A single IO from the AX88279 can be configured via register writes as a 1PPS input or output. This pin is connected to an SMA connector via several muxes and buffers to support both input and output modes, as well as support +5V signaling levels. The muxes are configured via a 4 position dip switch. 

<div style="text-align: center;">
  <img src="/assets/images/projects/timestick-v2/block.drawio.png" alt="Time Stick V2" width="600">
</div>

Here is the schematic implementation:

<iframe 
  src="{{ '/assets/images/projects/timestick-v2/schematic.pdf' | relative_url }}" 
  width="100%" 
  height="600px" 
  style="border: none;">
  <p>This browser does not support PDF embedding. 
     <a href="{{ '/assets/images/projects/timestick-v2/schematic.pdf' | relative_url }}">Download the schematic PDF</a>.
  </p>
</iframe>

## PCB Design

One of the aspect of this design which made it fun was the unique shape of the board. Because this design would be part of the OCP Time Appliances Project, we thought it would be fun for the shape of the board to be similar to the Time Card logo.

<div style="text-align: center;">
  <img src="/assets/images/projects/timestick-v2/logo_grey.png" alt="Time Stick V2" width="600">
</div>

The design is a 4 layer stackup with a total target thickness of 1.6mm. We have controlled impedance traces for the USB (90-ohm differential) and ethernet interfaces (100-ohm differential), as well as the SMA connector (50-ohm single ended). Proper grounding is paramount so layers 1 and 4 are the primary routing layers while 2 and 3 are used for power and ground planes.
<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/timestick-v2/pcb_layers/' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="PCB Layer">
          <p class="layer-caption">{{ image.basename | replace: '-', ' ' }}</p>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="carousel-wrapper">
  <button class="carousel-nav prev" aria-label="Previous image">←</button>
  <button class="carousel-nav next" aria-label="Next image">→</button>
  <div class="pcb-carousel">
    {% for image in site.static_files %}
      {% if image.path contains 'assets/images/projects/timestick-v2/board/' %}
        <div class="carousel-item">
          <img src="{{ image.path | relative_url }}" alt="Board Photo">
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

The logos on the back feature ASIX (generously provided the AX88279 chips for the build), WiWi Stamp (for funding the build), and Elecrow (PCB and PCBA contract manufacturer) for sponsoring the build. The result is we end up with a clean looking design that was both functional and aesthetically pleasing.

The canadian flag is there to commemorate the fact that this board was featured in the ISPCS 2025 Time Stick servo loop contest which was hosted in Canada.

## Testing
To test the functionality of the device we needed to download the AX88279 "Linux TOD Suite" from ASIX's GitLab repository and build the device driver which has PTP support. 

I wrote a supplementary test script to run a PTP test in a leader/follower configuration. The script automates the configuration of a Time Sticks to establish a link between devices. It handles the critical low-level tasks—reloading specific USB-Ethernet kernel modules, enforcing static IP assignment, and validating connectivity—before launching the ptp4l engine to synchronize system clocks.

<div style="text-align: center;">
  <img src="/assets/images/projects/timestick-v2/test.jpg" alt="Time Stick V2" width="600">
</div>

A scope is set into infinite persistence mode and the test is run. The waveforms below show that the devices were able to synchronize their clocks to within 100ns of each other. Channel 2 is the 1PPS signal from the master and channel 3 is the 1PPS signal from the follower. We see the follower tends to lag within 100ns of the master.

<div style="text-align: center;">
  <img src="/assets/images/projects/timestick-v2/waveforms.jpg" alt="Time Stick V2 Waveforms" width="600">
</div>
