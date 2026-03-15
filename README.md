# armando-jp.github.io

Personal portfolio website — hardware design and music production.

Built with [Jekyll](https://jekyllrb.com/) and deployed via GitHub Pages.

## File Structure

```
├── _config.yml              # Site configuration
├── _data/
│   └── navigation.yml       # Header navigation links
├── _includes/
│   ├── head-custom.html     # <head> meta, fonts, CSS
│   ├── header-custom.html   # Site header + nav + dark mode toggle
│   └── footer-custom.html   # Site footer
├── _layouts/
│   ├── default-custom.html  # Base HTML shell
│   ├── home-custom.html     # Landing page
│   ├── page-custom.html     # Generic section pages
│   ├── post-custom.html     # Blog posts
│   └── project-custom.html  # Project detail pages
├── _pages/
│   ├── workshop.md          # Engineering projects grid
│   ├── writing.md           # Blog / deep dives listing
│   └── studio.md            # Music + PCB art showcase
├── _projects/
│   ├── lw-adc.md            # RPi 5 Audio HAT
│   ├── stm32-dsp.md         # STM32G431 DSP algorithms
│   └── guitar-pedal.md      # Custom guitar pedal
├── _posts/                  # Blog posts (YYYY-MM-DD-title.md)
├── assets/
│   └── css/
│       └── main.css         # Complete design system
├── index.md                 # Landing page content
└── Gemfile                  # Ruby dependencies
```

## Local Development

```bash
bundle install
bundle exec jekyll serve
# Open http://localhost:4000
```

## Design

- **Typography**: Space Grotesk (sans-serif)
- **Color**: Dark-mode-first with light mode toggle
- **Aesthetic**: Minimalist, high-contrast, heavy white space
